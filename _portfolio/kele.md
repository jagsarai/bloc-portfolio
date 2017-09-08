---
layout: post
title: Kele
thumbnail-path: img/Kele-Main.png
short-description: Kele is a Ruby Gem API client to access the Bloc API

---

{:.center}
![]({{site.baseurl}}/img/Kele-Main.png)

## Summary 

Kele is a Ruby Gem API client that will retrieve JSON data from the Bloc API. Users will be able to make REST calls and get specific desired information.

## Explanation

The goal of this project was to help make it easier to get information from the Bloc API. My role in this project was to code out all of the methods that would aid the user in obtaining the following information. 

1. Verify user authenticity 
2. Get user information on Bloc
3. Get Bloc mentor availability 
4. Get all user messages sent and received on Bloc
5. Create new message 
6. Create submission for assignments

## Problem/Challange 

The main challenges for this project were to find the right way to create a new message and create a checkpoint submission. The goal was to do this from the command line. 

## Solution

The first step in creating this project was to get an easier way of making API calls from the command line using irb. So, for that I used the HTTParty Gem. Next, I needed to get the data as a JSON blob and to ease the transition, I used the json Gem. 

After the setup was complete, I was able to code the first 4 requirements with ease. Then came the tougher challenges of creating a new message. For this I needed create a function that would take in the sender’s email, the recipients id, the message token, the subject, and finally the message the user would like to send.

```Ruby
  def create_message(sender=@user_email, recipient_id, token, subject, message)
    create_message = self.class.post(
      "/messages",
      body: {
        :sender => sender,
        :recipient_id => recipient_id,
        :subject => subject,
        :token => token,
        :stripped_text => message
      },
      headers:{
        :content_type => 'application/json',
        :authorization => @auth_token
      })

    p create_message
    puts create_message.body
  end
 ```

Next, I needed to find a way to create a checkpoint submission. For that I needed a function that would take in the assignment branch, the commit link, checkpoint id, and an optional comet. The user’s enrollment id would be taken by making a call to the `get_me` method defined earlier in the code.

 ```Ruby
   def create_submission(assignment_branch, assignment_commit_link, checkpoint_id, comment=nil)
    @enrollment_id = self.get_me["id"]

    submission = self.class.post(
    "/checkpoint_submissions",
    body: {
      :assignment_branch => assignment_branch,
      :assignment_commit_link => assignment_commit_link,
      :checkpoint_id => checkpoint_id,
      :comment => comment,
      :enrollment_id => @enrollment_id
    },
    headers: {
      :authorization => @auth_token
    })

    p submission
    puts submission.body
  end
  ```

## Result

The Kele gem API client works as expected and we are able to make calls from the command line and get the results that we need. 

![]({{site.baseurl}}/img/Kele-1.png)

![]({{site.baseurl}}/img/Kele-2.png)

![]({{site.baseurl}}/img/Kele-3.png)

## Conclusion 

This project really helped me reinforce my backend skills and allowed me to work on making API calls. I was able to broaden my skills in using the command line. This was a fun and awesome project. Click [here](https://github.com/jagsarai/Kele) for the Github code.

