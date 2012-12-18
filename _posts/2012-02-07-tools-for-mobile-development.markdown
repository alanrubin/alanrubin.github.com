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
As described in [The Pragmatic Programmer: From Journeyman to Master](http://www.amazon.com/Pragmatic-Programmer-Journeyman-Master/dp/020161622X), *"Every craftsman starts his or her journey with a basic set of good-quality tools... Tools amplify your talent. The better your tools, and the better you know how to use them, the more productive you can be... Always be on the lookout for better ways of doing things."*

Coming from a web development background to the mobile scene and developing in (relatively) new technologies such as [Sencha Touch](http://www.sencha.com/products/touch) and [Phonegap](http://phonegap.com/) I find that good tools to help you in the job and improve your productivity are far more scarce and much more needed.

Much more needed because, by nature, mobile debugging is more complex and time consuming. You don't have a debug/console browser desktop window available all the time as your application runs in the device (or simulator) and deployment to devices naturally takes more time then running in your browser from a local webserver.

After getting some suggestions from friends and doing some research, I have compiled a list of nice tools and advices that I would be happy to share.

####[Chrome](https://www.google.com/chrome) vs. [Safari](http://www.apple.com/safari/)
Safari was my natural choice as I was initially focusing in Phonegap for iOS. But I have found out the Safari has some limitations. First, it is not updated frequently as Chrome, thus many nice improvements and bug fixes for the Developer Tools are lacking. Second there are less extensions option for Safari. After working with Safari for some time, I now stick with Chrome.

####[Window Resizer Chrome Plugin](https://chrome.google.com/webstore/detail/kkelicaakdanhinjdeammmilcgefonfh)
The next step is to have your browser window with the same size of the device. Don't forget to get into account the status bar for specific devices (iOS for example has 20px status bar). [Window Resizer Chrome Plugin](https://chrome.google.com/webstore/detail/kkelicaakdanhinjdeammmilcgefonfh) is a good options for Chrome, but most of the device's resolutions you will need to configure by yourself. Don't forget to choose "Mobile Device" when configuring a new device, so it won't take into account the window size but the browser's viewport size instead. **Update**: Just found a new plugin - [Resolution Test](https://chrome.google.com/webstore/detail/idhfcdbheobinplaamokffboaccidbal). You can try it also.

####[Responsive Design - Resize Browser Bookmarklets](http://www.benjaminkeen.com/misc/bricss/)
The resizer plugin above has some limitations. First (in my Mac) it cannot resize the browser window below 400px - so you won't be able to simulate some phone devices. Second you cannot have different windows sizes at the same time on the same page - this is specially useful in an Android world, where devices probably have different resolutions and you want to test side by side. Some people have created different [bookmarklets](http://www.bookmarklets.com/about/) (which are tiny js program contained in a bookmark) for solve those issues. Most remarkable ones are [David Chambers](http://davidchambersdesign.com/resize-browser-window-to-match-iphone-viewport-dimensions/), [Been Keen Bookmarklet genereator](http://www.benjaminkeen.com/misc/bricss/) and [Resizer](http://codebomber.com/jquery/resizer/).

####[Ripple](http://blackberry.github.com/ripple/index.html)
Another nice tool is [Ripple](http://blackberry.github.com/ripple/index.html). Although now owned by Blackberry, it is can be used on different platforms to simulate APIs such as Phonegap and you can use standard browser (Developer Tools) to debug the application. Honestly I have used it in the past and I was not happy with its stability - this seems to have improved now. May be worth to use it if you are having issues with Phonegap API and deployment to your simulator or device is a slow process.

####[xScope mirror](http://xscopeapp.com/)
A new tool that was just released is xScope mirror. It is part of [xScope app](http://xscopeapp.com/), which is "a powerful set of tools that are ideal for measuring, inspecting and testing on-screen graphics and layout". [xScope Mirror](http://xscopeapp.com/features) basically let you mirror a browser desktop window in your iOS device over the network. I don't think you would buy xScope only because of the mirror feature, but the app itself is very recommended for designer and developers - it is really worth 20 bucks (now in sale at [AppStore](http://itunes.apple.com/app/xscope/id447661441?mt=12&ign-mpt=uo%3D4)).

####Weinre
[Weinre](http://phonegap.github.com/weinre/) is a very nice server-side solution to web inspect and run js code remotely on devices and simulators. Its UI is very similar to the Webkit's Developer Tools. I consider [Weinre](http://phonegap.github.com/weinre/) the tool that provides the most realistic inspecting/debug capabilities, as you can use it in real running mobile apps. Together with that, it has some drawbacks - first one is that you cannot really debug (like using breakpoints and so on) but only inspect elements and run code in the console; second is that it tends to mess with complex Sencha applications, so in some situations you just cannot use it.

####BlackBerry's WebWorks Web Inspector
I'm not a fan of BlackBerry, but for those of you who are using Blackberry 7 devices such as Playbook I recommend taking a look at [Web Inspector](http://devblog.blackberry.com/2011/06/debugging-blackberry-web-apps/). It could be a good option in addition to [Weinre](http://phonegap.github.com/weinre/).

Tools are very important in order to improve your productivity, specially in the mobile scene, where there are so many different platforms and devices.

Hope the list above will help you - I will be happy to get more tools sugestions and reviews in below comments.