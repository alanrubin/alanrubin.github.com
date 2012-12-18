--- 
layout: post
title: 'TIP: Hosts file in Mac OSX and POW!!'
tags:
- mac
- tip
- hosts
- dns
- rack
- pow
description: Alan Rubin writes about host files in Mac and also about a nice rack server called POW
---
This is an old tip, but I keep going back to google in order to find it out. So I decided to put it here in my blog in order to save google queries and also to give you some nice rack server connected to rails.

So if you need to configure additional dns names for different ip's in your local computer, open your terminal and write:
{% highlight bash %}
sudo nano /private/etc/hosts
{% endhighlight %}
You can also use any editor you want instead of 'nano', this includes [textmate](http://macromates.com/) and textedit.

Edit the hosts file as you need : for each line, enter the ip and the host name you want to configure. For commented lines, use # char at the beginning of the line, as shown below. Exit 'nano' and save the file (Crtl-X and Yes). 

{% highlight bash %}
127.0.0.1      portalondemand
# this line is commented line and also the line below
#10.23.22.30   deactivate.name
{% endhighlight %}

After saving, you will need to refresh the dns cache of your computer by running:

{% highlight bash %}
dscacheutil -flushcache
{% endhighlight %}
... and you are done.

Why editing host files ? Sometimes you need to associate an ip to a hostname in your local computer. For example, if you are developing an application locally and you want to test it with a domain name in your browser instead of using 'localhost' or '127.0.0.1'.

If this is your scenario, I recommend you also to take a look at 37Signals [POW](http://pow.cx/), a very nice zero configuration Rack server for Mac OSX. [POW](http://pow.cx/) saves you additional work by using convention over configuration so you can get all your applications running with different hostnames.

If you are interested about UI design like I am, you can also enjoy the nice design of the [POW](http://pow.cx/)'s website and checkout additional info about its designer [Jamie Dihiansan](http://37signals.com/svn/posts/1210-introducing-our-new-designer-jamie-dihiansan).

<a href="http://pow.cx/"><img src="/images/logo-pow.png"></a>