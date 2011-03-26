--- 
layout: post
title: Simple WordPress Security Measures?
tags: 
- security
- wordpress
categories: blog
---
With the recent <a href="http://blog.wired.com/27bstroke6/2009/01/professed-twitt.html">Twitter hacking fiasco</a>, web designer <a href="http://webdazzling.com/about/">Chris Herbert</a> (<a href="http://www.twitter.com/ChrisHebert">@ChrisHerbert</a>) provided us with <a href="http://webdazzling.com/can-you-be-hacked/">some helpful tips for securing WordPress</a>.

One of the tips is regarding security measures. These tips come from <a href="http://www.mattcutts.com/">head of Google's Webspam team, Matt Cutts</a>. Matt talks about <a href="http://www.mattcutts.com/blog/three-tips-to-protect-your-wordpress-installation/">securing your wp-admin directory, creating a wp-content/plugins/index.html, and subscribing to the WordPress development blog</a>.

The one tip I have for securing your WordPress instance is:

<h3>Don't use the <em>admin</em> account</h3>
Using the default <em>admin</em> account normally leaves you open to <a href="http://www.codinghorror.com/blog/archives/001206.html">Dictionary attacks</a> depending on your password. Instead, create a very strong password for the <em>admin</em> account and create a separate administrator account of your own. See <a href="http://www.uxbooth.com/">UX Booth</a>'s <a href="http://www.uxbooth.com/blog/password-usability/">How To Pick Passwords That Protect Your Online Experience</a>. You can also downgrade the authority level of the <em>admin</em> account if you so chose.

I would also recommend regularly using an <em>author</em> or <em>editor</em> account if you don't need any of the administrative power.

<strong>Got any WordPress security tips? Please share them in the comments.</strong>
