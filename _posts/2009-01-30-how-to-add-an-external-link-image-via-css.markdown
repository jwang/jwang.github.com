--- 
layout: post
title: "How To: Add an external link image via CSS"
tags: 
- css
categories: blog
---
Pop up links suck. The twitterverse also responded when asked: <a href="http://www.johntwang.com/blog/2009/01/22/should-links-open-in-a-new-window-twitter-web-designers-voice-in/">Should Links Open in a New Window? Twitter Web Designers Voice In</a>. But sometimes you need to use them. Sometimes you are required by your clients to use them. Or sometimes it is best to use them. It would be nice to at least let the person know that clicking on the link will open a new window/tab. Here's an easy way to add a little image to the external links using CSS.

<strong>The CSS</strong>
[sourcecode language='css']
a[rel="external"], a.external {
white-space: nowrap;
padding-right: 15px;
background: url(/images/external.gif) no-repeat 100% 50%;
zoom: 1;
}
[/sourcecode]

<strong>The HTML</strong>
To use the CSS, you would add the rel=&quot;external&quot; to your html link like so:
[sourcecode language='html']
<a href="http://www.google.com" rel="external" target="_blank">External link</a>
[/sourcecode]

<strong>The Result</strong>
Here's what the final result looks like after using the CSS and HTML. <em>(This is an image! Not a real link.)</em>
<a href="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/01/ext-link-example1.png"><img src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/01/ext-link-example1.png" alt="External Link CSS Example" title="External Link CSS Example" width="104" height="28" class="alignnone size-full wp-image-520" /></a>

<h3>Do you do this differently? Have a better solution? Please feel free share thoughts, feedback and comments.</h3>
