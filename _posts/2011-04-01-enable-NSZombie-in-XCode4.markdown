---
layout: post
title: Enable NSZombie in XCode 4
categories: blog
tags: [xcode4, ios, iphone]
categories: blog
---

Cocoa Developers, especially new iOS and Mac developers have all likely run into the famous _EXC\_BAD\_ACCESS_ error message in XCode. I'm not going to go into what that error message is and how it gets trigger. There's plenty of content on that online.

The way we normally debug those errors, are through NSZombies. You can go through Instruments and find those, but you can also just use the XCode Debugger while you're running the app in Debug by turning on the Environment Variable.

In XCode 3.x, you would simply right-click on the .app file in the Product category and choose _Get Info_ then set the variable NSZombieEnabled to YES in the _Arguments_ tab.

In XCode 4, this has been moved. The right-click menu no longer has the _Get Info_ option, and there's also no Inspector to help you. Instead you must now go to, the top menu and select _Product_-> _Edit Scheme_ or use the " ⌘ <" keyboard shortcut to bring up the menu.

![Product Menu](http://farm6.static.flickr.com/5053/5581705586_72ea92b8d4_d.jpg "Product Menu")

From here, you can simply verify that the _Run_ scheme is selected and set the environment variable in the tab as you normally have in XCode 3.

![Scheme Menu](http://farm6.static.flickr.com/5306/5581705556_9e368ac41f_z_d.jpg "Scheme Menu")