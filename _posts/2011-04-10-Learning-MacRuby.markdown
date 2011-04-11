--- 
layout: post
title: Learning MacRuby
tags: [ruby, macruby, cocoa, objective-c]
categories: blog
---
I recently started learning [MacRuby](http://macruby.org). The successor to [RubyCocoa](http://en.wikipedia.org/wiki/RubyCocoa), which when I last looked at, was messy to get into. Since then, MacRuby has come a long way and will be a private framework in Apple's OS X Lion (10.7). This primarily just means that it will be present, but you will still need to package MacRuby within your app for bit longer.

If you're interested in the differences between the two, there's a great resource on the MacRuby Documentation section titled [Essential Differences between RubyCocoa and MacRuby](http://www.macruby.org/documentation/rubycocoa-to-macruby.html).

So far it's been fairly painless and a great learning experience. It's also likely a lot easier to pick up if you already know a bit of Objective-C and/or have had some [Ruby](http://www.ruby-lang.org/en/) exposure. That way you're not trying to learn two completely different languages and trying to bind them together. That's not to say, you can't. It's very much possible and you can learn Objective-C along the way.

The [MacRuby team](http://www.macruby.org/project.html) does a great job at pushing out fixes and getting back to developers on their [Mailing List](http://lists.macosforge.org/mailman/listinfo.cgi/macruby-devel) and IRC channel #macruby. They've also just recently migrated over to [GitHub](https://github.com/macruby) which made me much happier about digging into [the source](https://github.com/MacRuby/MacRuby) and getting started a bit better.

I'm hoping to push out at least a couple of apps to the the Mac App Store using MacRuby and see how that goes. I also plan to convert the [PGPane preference pane](https://github.com/jwang/pgpane) to MacRuby in a separate branch as an exercise.

Here are some of the resources I have been using to learn MacRuby.
###Websites
*    Apple's [Developing Cocoa Applications Using MacRuby](http://developer.apple.com/library/mac/#featuredarticles/UsingMacRuby/_index.html#//apple_ref/doc/uid/TP40010259) at their Mac OS X Developer Library.
*    [MacRuby.org's Documentation section](http://www.macruby.org/documentation.html) has some great tutorials, screencasts and more.

###Screencasts
*    PeepCode's [PeepOpen app](http://peepcode.com/products/peepopen) and [Meet MacRuby screencast](http://peepcode.com/products/meet-macruby).
*    Mike Clark's [MacRuby](http://pragmaticstudio.com/screencasts/6-macruby) Screencast for Pragmatic Programmers
*    Mike Clark's [Embedding MacRuby](http://pragmaticstudio.com/screencasts/7-embedding-macruby) screencast for Pragmatic Programmers

###Books
* [MacRuby: The Definitive Guide by Matt Aimonetti](http://ofps.oreilly.com/titles/9781449380373/)
* [MacRuby in Action by Brendan Lim and Paul Crawford](http://www.manning.com/lim/)

I also found this nice step by step blog post while listening to [The Ruby Show](http://rubyshow.com/). [MacRuby & Xcode 4: Build a Self-Contained MacRuby Application](http://redwoodapp.posterous.com/macruby-and-xcode-4-build-a-self-contained-ma)


