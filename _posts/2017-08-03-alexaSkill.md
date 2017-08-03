---
layout: post
title: My First Alexa App - Third Degree Sports
---

With AI and voice enabled devices slowly beginning to emerge into our everyday lives, I decided to take on the challenge of building an Alexa skill myself. My goal with this post is to share just how simple and easy it is to get started. 
 
Modern day app building has become simpler than ever. A simple google search is all it took for me to get started. Doing research is essential to learning anything new and for software engineers like me, google search is like the gifts from the Gods.  The wealth of resources and documentation online can help get you started in the right direction. In fact, Amazon does a great job internally to get us started, and building a simple skills app can be done in a jiff…Well almost in a jiff, if you know what you’re doing. 

I sarted by forking a sample project from github that had the layout of a trivia game set in place. I have provided the link below.  

https://github.com/alexa/skill-sample-nodejs-trivia

The documentation for this repo does a great job of walking us through the setup process so I wont bore you with the details and you guys can check it out yourselves. 

Next, I had to come up with my idea of trivia app. For me sports debates and tirvia haave always been the topic of converstation amongst my friends. So I figured it would be really cool to have sports trivia trainer. I wanted to start small so I came up with 5 random questions out of the 50 or so possible(Again, I wanted to start small and not complicate things) that the user can ask Alexa and choose from the multiple choice ansewers. Alexa would keep score of the right and wrong answers as well as reply with the correct answer when the user got it wrong.

After writing the code, I uploaded it to Amazon's free cloud computing service, Lamda. This allows seemless inteaction between my code and Alexa. Now came the fun part, testing! I tested the app using a cool and awesome website that gives a vitrual Echo to test our skill on. 

https://echosim.io/

I have submitted my app to Amazon and it should be in the Alexa skills store soon. Well that was simple right? And really it is that simple and that is the point. What seems like a very complicated process, has been made simple largely due to Amazon's own documentation and templates that can get anyone with little programming knowlege to start building exiting and fun apps.  