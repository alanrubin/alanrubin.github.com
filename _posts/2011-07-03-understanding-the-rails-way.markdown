--- 
layout: post
title: Understanding the Rails Way
tags:
- ruby
- rails
- activerecord
- method_missing
- NoMethodError
- cache_classes
description: Alan Rubin shows a problematic rails code and talk about why developers should always try to understand the Rails Way
---
One of the most commons things I see in my daily work with Ruby and Rails, specially among beginners, is the lacking of understanding of the Ruby/Rails Way. Today I had to fix an issue that showed up in our test server but did not happen in our development environment and was directly related to this lack of knowledge. Let me share it with you.

**The issue:** When creating a new ActiveRecord object from class *EwsWorkspace*, we saw that some of the methods defined in the object definition were missing in the object instance, thus causing *NoMethodError* in different parts of the code. The issue was occurring whenever *config.cache_classes* was set to true (in test environment for example). It happens also when I run it in rails console. 

If you are not aware of it, what [config.cache_classes](http://railsguts.com/initialization.html) does is to tell Rails that all of your models, controllers and other classes will be loaded up as part of application initialization. There are [many](http://www.ruby-forum.com/topic/164670) [different](http://www.google.com/search?q=NoMethodError+rails+cache_classes+true) [discussion](http://railsforum.com/viewtopic.php?id=28830) threads about the cache_classes property around, but many of them are related to problems caused when this property is set to false - my problem was exactly the opposite. Based on the threads, my intuition was that the problem was connected to some sort of loading order causing problems to the application.

After checking some code and writing some puts statements around, I got to the *EwsWorkspace* model class code, which had a very bad smell - check it out and see if you can smell it
{% highlight ruby %}
class EwsWorkspace < ActiveRecord::Base
  include EwsAccessControlMixin
  # tip: stinks from here...
  if EwsPage.column_names.include?('position')
    has_many :ews_pages, :dependent => :destroy, :order => 'position, id'
  else
    has_many :ews_pages, :dependent => :destroy, :order => 'id'
    logger.info '>>> Please check that DB migrate: 900008_add_columns_to_ews_pages.rb was executed.'
  end
  belongs_to :user
  validates_presence_of :user_id, :message => 'must have a valid workspace owner'
  validates_presence_of :name, :message => 'name must not be blank'
  
  #... public methods code such as below ...
  
  def restrictions_security
  {
      :user_id => self.user_id
  }
  end

  private

  #... Other private methods until end...

end
{% endhighlight %}
Ok, if you didn't get it, take a look at the logger.info line statement. Do you understand what is the problem ? 

Basically the code is checking if a migration was executed that created a additional column ('position') in a different model and if it was, it is creating a relation ordered by specific fields; if migration was not executed, the code still creates a relation but also log an info "message" to user that he has to run the missing migration.

By now, I think that you already realized the problem. This is so against the Rails way ! If someone is not updated with his migrations he should fail now ! The running code should not be responsible for detecting if migrations are up to date. Let's think about the hypothetical situation where we need to check everywhere if any ever existing migration was executed or not... This would be crazy...

**Back to the problem:** Although I did not get any explicit error from ruby/rails, my guess is that by using cache_classes = true, Rails is loading all the classes in some specific order so the *EwsWorkspace* class is being loaded before *EwsPage* class. Because of that, it seems to me that EwsWorkspace is failing to load the subsequent lines below the if statement, thus not including the methods that are executed afterwards. By replacing all the if/else statement by a :has_many statement, the application now works fine.


{% highlight ruby %}
class EwsWorkspace < ActiveRecord::Base
  include EwsAccessControlMixin
  has_many :ews_pages, :dependent => :destroy, :order => 'position, id'
  belongs_to :user
  
  #... Other code from here...

end
{% endhighlight %}

This is my best guess, as no initialization errors were displayed in the logs. Feel free to drop a line in comments below if you have a better explanation for this issue. 

**The bottom line:** Learn the Ruby/Rails Way. When developing with Ruby/Rails, sit down with yourself (or other experienced devs) for 5 minutes and think if what you will do is reasonable. This can save you (and me) a lot of headache.