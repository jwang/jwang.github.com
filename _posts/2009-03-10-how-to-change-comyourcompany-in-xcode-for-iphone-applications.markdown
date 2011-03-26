--- 
layout: post
title: How to change com.yourcompany in Xcode for iPhone Applications
tags: 
- iphone
- xcode
categories: blog
---
Having been doing some iPhone App development lately, I've run into one of the little issues abound. In order to test your application on an iPhone and also deploy it to the Apple iTunes App Store, you need to properly configure your application's Bundle Identifier in your Info.plist file. All the books tell you this. And Apple makes it a point to tell you how to do this before you try to deploy on the App Store.

After creating App after App though, it becomes a very tedious task. I've searched far and wide online and couldn't find anything that pointed me to how to change it. I did manage to find information on how to change it for developing Mac Desktop Applications. Didn't really help my case too much.

In any case, what you're looking for is in the directory path:
<em>/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Project Templates/Application</em>

In there, you'll find directories for each type of iPhone Application template that you can create from Xcode.
<ul>
	<li>Navigation-Based Application</li>
	<li>OpenGL ES Application</li>
	<li>Tab Bar Application</li>
	<li>Utility Application</li>
	<li>View-Based Application</li>
	<li>Window-Based Application</li>
</ul>
In each directory, there is a file called: <em>Info.plist</em>
Here you can change your default Bundle Identifier by finding the Key String pair.
[sourcecode language='xml']	<key>CFBundleIdentifier</key>
	<string>com.yourcompany.${PRODUCT_NAME:identifier}</string>
[/sourcecode]
Simply change yourcompany to your new default save. Once you've done that. Any new iPhone App you create from Xcode's iPhone Application templates will be pre-filled with your new identifier.

Much easier than fixing it for each new iPhone App you make. Hope this helps!
