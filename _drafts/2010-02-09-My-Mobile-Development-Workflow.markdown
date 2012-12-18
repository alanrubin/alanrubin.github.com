--- 
layout: post
title: Tools for mobile development
tags:
- javascript
- mobile
- chrome
- safari 
- webkit
- tools
description: Alan Rubin talks about nice tools his is currently using for mobile development
---                                                                                                            
Briefly, my development workflow is:

+ I test the mobile js application in my browser. This makes debugging and updating the application code easy - no need for deployment and we can use debug tools. As iOS, Android and BlackBerry WebWorks are all based on [Webkit opensource web browser engine](http://www.webkit.org/), I probably get the closest result for the device rendering. Be aware that some differences in rendering still exist, so don't be too strict on fixing small issues (specially with css). The only issue is that we cannot use phonegap API's (Well we can - see Ripple tool below).
+ Along with the step above, I test from time to time the application in my device browser. For that, I access the application from the device browser's (in iOS you can add it as vanilla app to your home screen) by pointing it to the url of my local web server. Very good to catch small browser rendering differences - and almost no time is wasted with deployment.
+ After I get some confidence, I build the app with phonegap and test it in the device simulator. This is much faster then deploying to the device and you can test (most) phonegap API's.
+ Finally, I deploy to the device and run through all scenarios in order to make sure things are working.

The below workflow must be flexible and it may change depending on features being tested. Some feature such as Notification Push, CSS3 animations, Camera Phonegap API's and others cannot be tested in specific configuration. Also some devices have very buggy (BB Playbook) and/or slow simulators (Android) so sometimes is better to run directly in the device.