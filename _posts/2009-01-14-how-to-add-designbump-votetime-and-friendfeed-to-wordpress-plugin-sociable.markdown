--- 
layout: post
title: How to Add DesignBump, Vot.eti.me, and FriendFeed to WordPress plugin Sociable
tags: 
- designbump
- friendfeed
- sociable
- votetime
- wordpress
categories: blog
---
<img src="http://www.johntwang.com/blogturbo.paulstamatiou.com/uploads/2009/01/wplogo-stacked-rgb-300x186.png" alt="wplogo-stacked-rgb" title="wplogo-stacked-rgb" width="300" height="186" class="aligncenter size-medium wp-image-366" />
Since I tend to write mostly about web design and development, sometimes people like to "bump" or "vote" for articles. <a href="http://yoast.com/wordpress/sociable/">Sociable</a> currently only has the <a href="http://www.dzone.com/">DZone</a> and <a href="http://www.designfloat.com">DesignFloat</a> links by default. So here's how you can add <a href="http://www.designbump.com">DesignBump</a>, <a href="http://vot.eti.me">Vot.eti.me</a>, and <a href="http://www.friendfeed.com">FriendFeed</a> links and icons to Sociable.
<h3>DesignBump</h3>
<ol>
	<li>Save the icon: <img class="alignnone size-full wp-image-337" title="design bump" src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/01/designbump1.png" alt="design bump" /> and copy it to <code>sociable/images/</code>directory.</li>
	<li>Open <code>sociable.php</code>.</li>
	<li>Find the array called <code>$sociable_known_sites</code>.</li>
	<li>Copy / paste the code below
[sourcecode language='php'] 'Design Bump' => Array(
'favicon' => 'designbump.png',
'url' => 'http://designbump.com/node/add/drigg?url=PERMALINK&amp;amp;title=TITLE',
),[/sourcecode]</li>
	<li>Save the file.</li>
</ol>
<h3>Vot.eti.me</h3>
<ol>
	<li>Save the icon: <img class="alignnone size-full wp-image-338" title="votetime" src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/01/votetime1.gif" alt="votetime" width="16" height="16" /> and copy it to <code>sociable/images/</code>directory.</li>
	<li>Open <code>sociable.php</code>.</li>
	<li>Find the array called <code>$sociable_known_sites</code>.</li>
	<li>Copy / paste the code below:
[sourcecode language='php'] 'Votetime' => Array(
'favicon' => 'votetime.gif',
'url' => 'http://vot.eti.me/login.php?return=/submit.php?url=PERMALINK&amp;amp;title=TITLE',
), [/sourcecode]</li>
	<li>Save the file.</li>
</ol>
<h3>FriendFeed</h3>
<ol>
	<li>Save the icon: <img class="alignnone size-full wp-image-338" title="votetime" src="http://www.johntwang.com/blogturbo.paulstamatiou.com/uploads/2009/01/friendfeed.png" alt="friendfeed" width="16" height="16" /> and copy it to <code>sociable/images/</code>directory.</li>
	<li>Open <code>sociable.php</code>.</li>
	<li>Find the array called <code>$sociable_known_sites</code>.</li>
	<li>Copy / paste the code below:
[sourcecode language='php'] 'FriendFeed' => Array(
'favicon' => 'friendfeed.png',
'url' => 'http://friendfeed.com/?url=PERMALINK&amp;amp;title=TITLE',
), [/sourcecode]</li>
	<li>Save the file and reactivate the plug-in.</li>
</ol>
<h3>Results</h3>
This is what the finished version looks like with the 2 services enabled through Sociable Options.
<img src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/01/sociable_bump_vote1.png" alt="sociable bump and vote" title="sociable bump and vote" width="250" height="49" class="alignnone size-full wp-image-357" />
And working version of the icons are used in this website. See below for an example.

<h3>Final Thoughts</h3>
I have also submitted the code to <a href="http://wordpress.org/extend/plugins/profile/joostdevalk">Joost de Valk</a>,the author of the <a href="http://wordpress.org/extend/plugins/sociable/">Sociable Plug-in</a>, in hopes that he adds them for everyone's use.

<a href="http://www.johntwang.com/sociable.zip">Download the complete code here</a>
