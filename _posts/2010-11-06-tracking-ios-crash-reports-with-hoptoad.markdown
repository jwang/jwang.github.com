--- 
layout: post
title: Tracking iOS Crash Reports with Hoptoad
tags: 
- crashbucket
- hoptoad
- ios
- ruby on rails
categories: blog
---
The folks over at thoughtbot have an great web app called <a href="http://hoptoadapp.com/">Hoptoad</a> to monitor exceptions. They've had support for a bunch of languages (PHP, Python, Ruby, etc) and frameworks (Merb, rails, Sinatra, etc) through their API. Until recently, I had been using a product called <a href="http://crashbucket.com/">CrashBucket</a> from the guys over at <a href="http://guicocoa.com/">GUI Cocoa</a> and <a href="http://twoguys.us/">Two Guys</a> to track my iOS apps crash reports. Thoughtbot has now partnered with GUI Cocoa and Two Guys to bring the CrashBucket functionality into Hoptoad. The result of this partnership is the <a href="http://hoptoadapp.com/pages/ios-notifier">Hoptoad iOS Notifier</a> and it was just announced on November 5th on <a href="http://robots.thoughtbot.com/post/1487918632/track-ios-crashes-with-hoptoad">Thoughtbot's blog</a>.
<!--more-->
While Crash Reports are available through the iTunesConnect portal, Hoptoad takes it a level further for developers. With the iOS Notifier, the crash reports are broken down by environment. You can also get crash reports for Adhoc app releases. Having these options are great for allowing the tracking down of crashes and bugs during development and beta testing. It's also great for those In-House apps that never make it to the App Store. That's the small requirement that iTunesConnect needs show reports.

[caption id="attachment_874" align="alignnone" width="300" caption="Image from Thoughtbot"]<a href="http://robots.thoughtbot.com/post/1487918632/track-ios-crashes-with-hoptoad"><img src="http://johntwang.com/blogturbo.paulstamatiou.com/uploads/2010/11/ios-notifier-post-iphone-and-ht-composed-300x162.png" alt="iOS Notifier" title="ios notifier" width="300" height="162" class="size-medium wp-image-874" /></a>[/caption]

Luckily for me, almost all of my apps are a combination of Ruby on Rails3 web apps and iOS apps. This makes it easy to see all of the reports for a bug in one place for both the Rails and iOS errors.

Hopefully Android support is on the way for Hoptoad as well.
