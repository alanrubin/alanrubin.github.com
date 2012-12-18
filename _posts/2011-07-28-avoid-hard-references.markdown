--- 
layout: post
title: Avoid hard references
tags:
- javascript
- frontend
- browser
- web 
- reference
- hard
- pattern
- observer
description: Alan Rubin talks about avoiding hard references in javascript code as he regrets about using in his past web project
---                                                                                                            
I believe that one of the most important personal traits is the ability to analyze past actions or decisions and be able to regret (and also to decide and make a change). I think that this is true regarding personal life and also professional decisions.

One of the things I have regretted lately, in a project my team has been developing, is the way we have handled references. The project is basically a dashboard project, something similar to [iGoogle](http://www.igoogle.com) and [NetVibes](http://www.netvibes.com), but for enterprise usage. (oh God, I hate the "enterprise" word!)  

Technically speaking, the dashboard, which has client side developed in javascript using [jQuery](http://www.jquery.com) and [jQuery UI](http://www.jqueryui.com), can contain several widgets with relevant content applications rendered using [OpenSocial](http://code.google.com/apis/opensocial/) standard. Those widgets are grouped in pages and can be resized or dragged in order to obtain flexible layouts.
                                                  
Javascript speaking, the application is divided into modules (i.e. namespaces). As an example, a module take care of loading pages from server and respective callbacks; other is responsible for rendering the layout of the pages; other module renders content and events for the gallery of available applications; another is responsible for the welcome screen, which are guidance messages for new users on empty pages. Many other modules have different functions across the application.

Let's drill down with an example so you can understand the problem. As said before, our dashboard can have pages (organized in tabs) and those pages are lazy loaded - when user selects the tab, the page content is requested from server and loaded using an AJAX call. Let's say that we want, when a page is loaded, that:

+ The pages tab should be resized so the name of the current page is fully displayed 
+ The layout for the current page should be rendered
+ A welcome message will be presented if page is empty
+ The gallery of applications should be closed if it is open
+ A new user's guide tooltip should be attached to the page's widgets now loaded 
 
So our first implementation was to call all those different modules directly from the success callback of the AJAX for loading the page:
{% highlight javascript %}      
// Callback after loading page from AJAX                  
goToPageCallback : function(opts) {
    if (Ews.WorkspaceManager.pageWasLoadedByAJAX(opts)) {
        var parsedData = jQuery.parseJSON(opts.data),
        // New Page being loaded from server's JSON
        newPage    = new Page(parsedData);
    }
    // Pages tab should be resized so current page name is fully displayed
    Portal.html.tabsHover();    

    //.... some code

    //Collapse gallery if it's opened
    if (Portal.gallery.isOpen()){
        Portal.gallery.hide();
    }    

    // ... some code
    // Render page layout
    Ews.WorkspaceManager.currentPage().layout.controller.renderLayout({},opts.pageId);

    // ... some code

    // Enforce guide is displayed if new page was loaded by AJAX
    if(Ews.WorkspaceManager.pageWasLoadedByAJAX(opts)) {
        Portal.guide.onPageChanged(opts.pageId);
    }

    // If page is empty, display page initialization screen
    if(Portal.dom.getData('widgets').length == 0) {
        Portal.welcome.pageInit();
    }
}
{% endhighlight %}                                                                          

What's the problem with the code above ? 

From my experience, the main problem is that you are creating hard references between namespaces in your application, or in other words, **you have a high coupled application with many decentralized coupling junctions**. Why a module that is responsible for the loading of a page should know about the existence of a gallery or a welcome screen? This is conceptually very bad, you have a high coupling between parts of your application which turns to be a nightmare for maintenance and future development.

This leads also for many bugs, specially regarding DOM elements animation. Let's say that the render layout call renders the different widgets in the current page using an animation. It is possible that when the render guide call (which needs to act on widgets already rendered) starts executing, the layout rendering is still not finished placing DOM elements in the correct places because of the animation, thus the render guide fails (or works badly). The worst thing is that because of the nature of javascript (one thread), this may happen or not depending of the computer and browser speed. A nightmare !

How do we solve that ? **Observer Pattern**. Use events instead of hard references. We can create a "post office" event service, which will be responsible to centralize all the coupling across the application. In the example above, after the page was loaded, we would trigger a "page.loaded.finish" event and all the other modules, which have subscribed previously to this event would execute callbacks inside their own modules. Simple, centralized and clean.

Apart from avoiding coupling, this event service could have also many other useful features, such as:

+ Know beforehand all the events supported, thus if someone subscribe (by mistake) to an undefined event, it raises an exception 
+ Unsubscribe from an event
+ Help debugging by providing a debug console 
+ Pass, together with the event data, the originator of the event, so subscribers can ignore it depending on whom has initiated it
+ Pass publishing time of the event
+ Asynchronous & Synchronous callbacks : The subscribers will be called one after the other or together asynchronously. We could use jQuery [Deferred](http://api.jquery.com/category/deferred-object/) [Objects](http://www.erichynds.com/jquery/using-deferreds-in-jquery/) feature for implementing this 
+ A subscriber blocks other execution, so we won't have DOM animation issues
+ A subscriber can cancel the event, thus event will not bubble to other subsequent subscribers 
+ Raise exceptions on callback errors in a clear manner
+ A subscriber callback is only executed after the event occurs n times
+ We can add constraints/filters for events above subscribers
   
Many other major js frameworks [already](http://docs.sencha.com/ext-js/4-0/#/api/Ext.util.Observable) [implemented](http://dojotoolkit.org/reference-guide/dojo/subscribe.html#dojo-subscribe) this pattern and it is a very common pattern in a client side application.

For [jQuery](http://www.jquery.com), because of its simplicity nature, the standard way for implementing this functionality is by using the [trigger](http://api.jquery.com/trigger/) and [bind](http://api.jquery.com/bind/) functions for custom events. It is very simple and does not provide many of the functionalities of major frameworks. I have found only [one](https://gist.github.com/860672) [serious](http://margenn.com/tubal/jquery/pubsub.html) [jQuery plugin](http://plugins.jquery.com/files/jquery.pubsub.js_.txt) implementing some of the features described above. I did not use it in production - if you have some experience about any jQuery observer plugins please drop a comment below.

So, in this big blog article, we saw the problem with hard references and how to avoid them by using the Observer pattern. I have also proposed a possible event service implementation - What a good suggestion for a [pet project](http://en.wiktionary.org/wiki/pet_project) !