--- 
layout: post
title: How To Use Google APIs with iPhone SDK
tags: [apple, google, iphone, xcode]
categories: blog
---
Adding Google API support to your iPhone App could not be any easier. Google provides Objective-C APIs for:
<ul>
	<li><a href="http://code.google.com/apis/base/">Google Base</a></li>
	<li><a title="Analytics" href="http://code.google.com/apis/analytics/">Analytics</a> - (Only available via SVN Trunk or manual download)<a title="Analytics" href="http://code.google.com/apis/analytics/">
</a></li>
	<li><a href="http://code.google.com/apis/blogger/">Blogger</a></li>
	<li><a href="http://code.google.com/apis/books/">Book Search</a></li>
	<li><a href="http://code.google.com/apis/calendar/">Calendar</a></li>
	<li><a href="http://code.google.com/apis/codesearch/">Code Search</a></li>
	<li><a href="http://code.google.com/apis/contacts/">Contacts</a></li>
	<li><a href="http://code.google.com/apis/documents/overview.html">Documents List</a></li>
	<li><a href="http://code.google.com/apis/finance/">Finance</a></li>
	<li><a href="http://code.google.com/apis/health/">Health</a></li>
	<li><a href="http://code.google.com/apis/picasaweb/">Picasa Web Albums</a></li>
	<li><a href="http://code.google.com/apis/spreadsheets/">Spreadsheets</a></li>
	<li><a href="http://code.google.com/apis/webmastertools/">Webmaster Tools</a></li>
	<li><a href="http://code.google.com/apis/youtube/">YouTube</a></li>
</ul>

<h3>Step 1</h3>
The first step, is to head on over to the <a title="Google Code Objective-C" href="http://code.google.com/p/gdata-objectivec-client/">Google Code website for the Objective-C Client</a>, <a title="Download link" href="http://code.google.com/p/gdata-objectivec-client/downloads/list">download</a> and extract the zip file source code. Alternatively, you can get the latest and greatest version via Subversion using:
<pre class="box-inner"> <tt id="checkoutcmd">svn checkout http://gdata-objectivec-client.googlecode.com/svn/trunk/gdata-objectivec-client-read-only</tt></pre>
If you downloaded the zip file from the website, you'll have version 1.7.0, and if you used the svn code you'll have a -read-only folder.

<h3>Step 2</h3>
Open up the GData XCode Project from your downloaded folder as well as your iPhone App XCode project.

<img class="alignnone size-full wp-image-655" title="extracted" src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/06/extracted1.jpg" alt="extracted" width="286" height="490" />

<h3>Step 3</h3>
Drag over the GData Sources Folder from the GData project to your iPhone App project and add it as reference. Don't check the box for <em>Copy items into destination group's folder (if needed)</em>. You do not need to copy over all the files into your project. You can, but it's not required.

<img class="alignnone size-full wp-image-656" title="to_drag" src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/06/to_drag1.png" alt="to_drag" width="186" height="67" />

<img class="alignnone size-full wp-image-657" title="copy_reference" src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/06/copy_reference1.jpg" alt="copy_reference" width="403" height="375" />

This will add a ton of files to your project. You may delete the APIs you are not planning on using, but make sure that the files <em>GDataXMLNode.m</em> and <em>GDataXMLNode.h</em> in the <em>Common/Optional/XMLSupport</em> group are <strong>not</strong> removed from your project as they are required for iPhone builds.

<img class="alignnone size-full wp-image-658" title="files" src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/06/files1.jpg" alt="files" width="258" height="345" />

<h3>Step 4</h3>
Open up the build settings for <strong>your</strong> iPhone App project. Located and set the following settings.
<ul>
	<li>Header Search Paths: /usr/include/libxml2</li>
	<li>Other Linker Flags: -lxml2</li>
</ul>
For the <em>Debug</em> build configuration only, add the Other C Flags setting so that the library's debug-only code is included:
<ul>
	<li>Other C Flags: -DDEBUG=1</li>
</ul>
<img class="alignnone size-full wp-image-660" title="build" src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/06/build1.gif" alt="build" width="603" height="743" />

<h3>Step 5 (Optional for iPhone 3.0 Beta SDK)</h3>
If you downloaded the zip file version (1.7.0) of the API, you will also run into <a title="NSTask SDK Issue 24" href="http://code.google.com/p/gdata-objectivec-client/issues/detail?id=24">this error message</a> when you attempt to first build your iPhone App project:
<blockquote>
<pre><span style="color: #ff0000;">...Source/Networking/GDataHTTPFetcherLogging.m:224: error: 'NSTask' undeclared (first use in
...Source/Networking/GDataHTTPFetcherLogging.m:224: error: 'task' undeclared (first use in this
function)
</span></pre>
</blockquote>
<p style="text-align: center;"><img class="size-full wp-image-651 aligncenter" title="Error Message" src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/06/error11.png" alt="Error Message" width="342" height="72" /></p>

Apple has removed the NSTask from the Foundations Framework in the iPhone 3.0 SDK. In order to fix this, simply open up the <em>GDataDefines.h</em> file, find the developer section and add:
<pre>#define GDATA_SKIP_LOG_XMLFORMAT 1</pre>

<img class="alignnone size-full wp-image-659" title="gdefine" src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/06/gdefine1.png" alt="gdefine" width="801" height="256" />

<strong>Note:</strong> This fix is only needed if you downloaded version 1.7.0 of the GData Objective-C API and are using iPhone 3.0 Beta SDK. If you downloaded the latest Subversion read-only trunk of the code or are using iPhone 2.2.1 SDK, you do not need to do this.
<h3>Step 6</h3>
At this point, your iPhone XCode project should build successfully and you can begin using the Google APIs by simply importing the appropriate header files. i.e.
<pre>#import "GDataAnalytics.h"</pre>

<h3>Step 7 (Optional if downloaded GData version 1.7.0 Zip file)</h3>
If you downloaded the zip file version (1.7.0) of the API, you will be missing the Analytics API. That has not yet been zipped up for the download. You may want to download those separately.
<h3>Additional Resources:</h3>
<ul>
	<li><a title="Using Google APIs in an iPhone App" href="http://googlemac.blogspot.com/2009/03/using-google-apis-in-iphone-app.html">Official Google Mac Blog: Using Google APIs in an iPhone App</a></li>
	<li><a title="gdata-objectivec-client" href="http://code.google.com/p/gdata-objectivec-client/">gdata-objectivec-client - Google Code</a></li>
	<li><a title="NSTask SDK Issue 24" href="http://code.google.com/p/gdata-objectivec-client/issues/detail?id=24">Issue 24 - gdata-objectivec-client - Build failure due to removal of NSTask from IPhone SDK 3.0 - Google Code</a></li>
</ul>
