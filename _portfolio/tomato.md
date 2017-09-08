---
layout: post
title: Tomato
thumbnail-path: img/Tomato-Main.png
short-description: Tomato is an Alexa skill that allows users to find local farmers market information.

---

{:.center}
![]({{site.baseurl}}/img/Tomato-Main.png)

## Summary

Tomato is an Alexa skills app that allows users to get information on local farmers’ markets. Users are able to invoke the skill with any Alexa enabled device. Tomato will tell the user the results of the top 5 markets nearby and users can then get further details on any one of the 5. 

## Explanation 

The goal of this project was to help people find local farmers markets near them. There wasn’t any voice enabled skills that did an adequate job at providing details on local markets. Alexa own responses were severely limited and there was a pressing need for a good solution. Hence the idea of Tomato was created. My role for this project was to create the entire voice enabled using Node JS. 

## Problem/Challenges

Tomato needed to provide accurate and precise details on farmers’ markets in order to become a useful solution to users. So, I had to find a source of data that would give me access to reliable farmers’ market information nationwide. After doing some research, I came across the USDA API which gave information on the registered farmers markets across the US. I found the source to be trustworthy and decided to use that as my source of information. As a bonus, the API was free of cost. 

Now that I had my data, the main challenge was coming up with a way to persist the data from one session to the next. In its default stage, once a session command was completed, the information inside is no longer available. Even if we set global variables to the results, after the session ends those global variables equate to null. I needed a way to gather and store the results of the API call to the USDA. This information would then be used in the next session when the user is prompted to select the market they would like specific details on (i.e. hours of operation, products sold, etc.). 

The next major challenge was to provide a way for users to interact with Alexa naturally. We wanted Tomato to aid the user and help get all of the required information if partial information was provided by the user. We needed a way to store that partial information and keep asking the user for any additional information till the user was able to completely answer what Tomato needed.  

## Solution

After doing some in-depth research on the topic of data persistence, I was able to find some useful information in the Alexa skills developer docs. Session attributes provided temporary storage solution that would allow us to take information from one session and use it another. The limitation was that we could only store information from one session. However, that was all we needed as we needed to get for our app. Once the user asked Tomato to find a farmers market near them or near a particular city, we would make a call to the USDA API and store the market results as attributes for the next prompt/session. In the code below, `this.attributes['GetDetails'] = result` is storing the resulting market data into the session for the `GetDetails` function. So, when the `GetDetails` function is called, the results of the `SearchNearCity` call would be available thus allowing us to get specific market details of the farmers market the user chooses.


```Javascript
	'SearchNearCity': function () {
       var slotCollection = delegateSlotCollection.call(this);
       console.log("attributes inside search near city", this.attributes['GetDetails'])

       var searchCity;
       console.log("city is: ", this.event.request.intent.slots.citySearch.value)
       
       //Check to see if the there is a value for city that we can search
       this.event.request.intent.slots.citySearch.value !== undefined ? searchCity = this.event.request.intent.slots.citySearch.value
                                                                      : searchCity = null;

       console.log("deviceId: ", this.event.context.System.device.deviceId);

       //Check to see if there is a device id
       this.event.context.System.device.deviceId !== undefined ? deviceId = this.event.context.System.device.deviceId
                                                               : deviceId = undefined;
       console.log("permissions: " , this.event.context.System.user.permissions);
       
       //Check to see if user permissions are given. 
       this.event.context.System.user.permissions !== undefined ? consentToken = this.event.context.System.user.permissions.consentToken 
                                                                : consentToken = undefined;

       console.log("consentToken: ", consentToken);
        if(deviceId !== undefined && consentToken !== undefined && searchCity !== undefined){
            getCity(searchCity, (city) => {
                getFarmersMarkets(null, city, null, (result) =>{
                    //Attributes collected for the GetDetails intent. 
                    this.attributes['GetDetails'] = result;
                    var answerString = '';
                    console.log("result is: ", result)
                    var finalOutput = result.map((answer) => {
                      return answer["1"]
                            || answer["2"]
                            || answer["3"]
                            || answer["4"]
                            || answer["5"]
                    }).map((market) => {
                      return answerString + market["name"] 
                    });
                    console.log("final output is: " + finalOutput);
                    var finalString = addNumToEachResult(finalOutput);
                    var outputSpeech = `Here are the markets near ${searchCity}: ${finalOutput}. Please tell me if you would like details on one. You can say something like, the first one.`
                    var repromptSpeech = `Here are the markets near ${searchCity}: ${finalOutput}. You can say something like, the first one.`
                    var cardTitle = `Markets near ${searchCity}`
                    var cardContent = `Here are the markets near ${searchCity}:\n ${finalString}`
                    this.emit(":askWithCard", outputSpeech, repromptSpeech, cardTitle, cardContent, null);
                })
            })               
        }
        else if(deviceId){
        	var permissions = ['read::alexa:device:all:address:country_and_postal_code']
            this.emit(":tellWithPermissionCard", "Please use the Alexa skills app to enable your location", permissions);
       }
       else{
            this.emit(":tell", "Please use a proper device to access this skill. Goodbye!")
       }  
    },
```
Alexa's skills builder tool allows us to interact with the user in a much more conversational way. We are able extract bits and pieces of information from the user. This ensures that the user is not overwhelmed with all of the information that is being asked of them. In addition, it also safeguards against an accidental omission of information that we need to collect. In order to make sure all of the required information is provided by the user I had to come up with a function that would wait until all of the required slots were completed with information before running the next piece of code. In the above example, `var slotCollection = delegateSlotCollection.call(this)` checks for complete information and if there is information missing we specify prompts in the Alexa skills builder tool that would ask the user for the required information.

```Javascript
function delegateSlotCollection(){
  console.log("in delegateSlotCollection");
  console.log("current dialogState: "+this.event.request.dialogState);
    if (this.event.request.dialogState === "STARTED") {
      console.log("in Beginning");
      var updatedIntent=this.event.request.intent;
      //optionally pre-fill slots: update the intent object with slot values for which
      //you have defaults, then return Dialog.Delegate with this updated intent
      // in the updatedIntent property
      this.emit(":delegate", updatedIntent);
    } else if (this.event.request.dialogState !== "COMPLETED") {
      console.log("in not completed");
      // return a Dialog.Delegate directive with no updatedIntent property.
      this.emit(":delegate");
    } else {
      console.log("in completed");
      console.log("returning: "+ JSON.stringify(this.event.request.intent));
      // Dialog is now complete and all required slots should be filled,
      // so call your normal intent handler.
      return this.event.request.intent;
    }
}
```

## Results 

After testing the app, it has been submitted to the Alexa team for certification. Locally, the app was able to seamlessly make call and get the desired information. Once the skill is live, I will be able to accurately gage its success. 

![]({{site.baseurl}}/img/Tomato-1.png)

![]({{site.baseurl}}/img/Tomato-2.png)

## Conclusion

This project was really complex and required a lot of research, but I was able to learn so much from having spent several hours on it. A feature that I really wanted to add was the ability to receive market location via text. The feature was completely developed using the Twilio API, however, given financial constraints (Twilio charges per each text sent) I was unable to implement it into this app. In the future, that would be something that I would like to explore further. 