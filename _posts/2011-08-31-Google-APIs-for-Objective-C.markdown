--- 
layout: post
title: New Google APIs Client Library for Objective-C
tags: [ios objective-c gdata]
categories: blog
---

A while back, I wrote a [small tutorial on how to use Google's GData Client library for iPhone](http://johntwang.com/blog/2009/06/08/how-to-use-google-apis-with-iphone-sdk/). It's a pretty popular tutorial that constantly gets questions. Today, [Google announced a new version of the Google Data library for Objective-C](http://googlecode.blogspot.com/2011/08/new-objective-c-library-for-new.html). I'm pretty exicted about this for a couple of reasons. The [new Client library](http://code.google.com/p/google-api-objectivec-client/) utilizes JSON, which I'm a big fan of and it also the new [OAuth2 for authentication](http://googlecode.blogspot.com/2011/03/making-auth-easier-oauth-20-for-google.html).

It looks just amazing.

{% highlight %}
#import "GTLBooks.h"

GTLServiceBooks *service = [[GTLServiceBooks alloc] init];

GTLQueryBooks *query = 
  [GTLQueryBooks queryForVolumesListWithQ:@"Mark Twain"];
query.filter = kGTLBooksFilterFreeEbooks;

[service executeQuery:query
    completionHandler:^(GTLServiceTicket *ticket,
                        id object, NSError *error) {
      // callback
      if (error == nil) {
        GTLBooksVolumes *results = object;
        for (GTLBooksVolume *volume in results) {
          NSLog(@"%@", volume.volumeInfo.title);
        }
      }
    }];
{% endhighlight %}

And it also follows the [Google Objective-C Style Guide](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml) which I'm also a fan of.

Check out the new version of the [Client library](http://code.google.com/p/google-api-objectivec-client/) at the Google Project page.

I'm going to try and get a new updated tutorial in a couple of days.
