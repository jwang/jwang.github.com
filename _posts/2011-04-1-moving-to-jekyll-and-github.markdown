---
layout: post
title: Moved to Jekyll and GitHub
categories: blog
tags: [jekyll, ruby, github]
categories: blog
---

I just finished moving my website and blog over to [Jekyll](http://jekyllrb.com) from WordPress. I also moved the hosting over to [GitHub](http://github.com). The process wasn't overly complex with the Jekyll [migration options](https://github.com/mojombo/jekyll/wiki/Blog-Migrations) available, but it was a bit time consuming, making sure things were back to normal. Converting the WordPress theme to the [Liquid](http://www.liquidmarkup.org) template system took a bit of time as well.

### Why Did I Move?

*   I wanted to learn something new. Jekyll is a static site generator written in Ruby.
*   I was having trouble accessing the old site due to some DNS issues with my provider which was preventing updates and new posts.
*   I want to have the content under source control, and primarily use Git and GitHub.
*   I like writing in [Markdown](http://daringfireball.net/projects/markdown), and WordPress was doing some funky formatting with my content.
*   I got tired of having to update WordPress and it's plugins whenever there was some security issue with it or new version that came out.
*   I'm addicted to GitHub, though, this won't be a reason for everyone.

As always, there's pros and cons to any system you use. These are the ones that I've had so far on the move.

### Pros
*   It's on GitHub
*   It's super fast with a 100 out of 100 score on Google [PageSpeed](http://pagespeed.googlelabs.com/#url=johnwang.com&mobile=false)
*   I can move it to [Heroku](http://heroku.com) or [Engine Yard AppCloud](http://www.engineyard.com/products/appcloud) if I ever need to with [Rack-Jekyll](https://github.com/bry4n/rack-jekyll)
*   Focus on Content and not worry about system updates
*   Not PHP

### Cons
*   Loss of Dynamic content management, ie. Photo Gallery, Comments, Search, etc.
*   Required paid GitHub account (cheapest is $7 per month.)
*   No Database - if you need a database and all.

### Overall
I'm very happy with having Jekyll as the platform and GitHub as the hosting. It's allowing me to focus on what's important. Content. While there are some annoyances and issues with the losses, I've found that many of them are rather trivial and simple to overcome. 

#### Overcoming the Cons
Comments are set up through [Disqus](http://disqus.com) which I was previously using anyway. 

Search is now handled by [Google Custom Search](http://www.google.com/cse/) instead of the default WordPress search. 

Tags and Categories are still an issue I plan to look at eventually, though there are plugins and extensions to Jekyll out there. Jekyll does support categories, but my permalinks structure is currently blocking use of the categories. 

For the image hosting, I'm currently considering using [Google Storage](http://code.google.com/apis/storage/), Amazon's [S3](http://aws.amazon.com/s3/) and [CloudFront](http://aws.amazon.com/cloudfront/) or just [Flickr](http://flickr.com). Leaning towards Google Storage, primarily because learning is fun.

