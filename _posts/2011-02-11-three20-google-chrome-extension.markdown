--- 
layout: post
title: Three20 Google Chrome Extension
tags:
- three20
- ios
categories: blog
---
Earlier this week, Roman Nurik released <a href="https://chrome.google.com/webstore/detail/hgcbffeicehlpmgmnhnkjbjoldkfhoin">the Android SDK Reference Search</a> Google Chrome Extension to search the Android SDK Docs. I wasn't aware that it was possible to add super powers to the Chrome Omnibox. Having wanting to create a Chrome Extension for quite some time now, but having no idea what do, I was inspired by Roman's idea and went through <a href="http://code.google.com/p/romannurik-code/source/browse/chrome-extensions/android-sdk-reference-search">his source code</a> and also <a href="http://code.google.com/chrome/extensions/samples.html">the Google Chrome Extensions sample</a>:<a href="http://src.chromium.org/viewvc/chrome/trunk/src/chrome/common/extensions/docs/examples/api/omnibox/extension-docs/">Extension Docs Search</a>.

<img class="alignnone" title="Three20 Chrome Extension" src="https://assets1.github.com/img/2fa131320a162f6bdd3b0519a49b7f13848137f3?repo=&amp;url=http%3A%2F%2Ffarm6.static.flickr.com%2F5179%2F5437698841_9af26a8255_d.jpg&amp;path=" alt="" width="400" height="275" />

The result, is a nifty little Omnibox for Three20's API library. This very simple extension adds an <em>tt</em> command to the Chrome Omnibox. For example, typing <em>tt TTView</em> will bring up a list of all class names in the Three20 Library API matching <em>TTView</em> - selecting a list item navigates to the relevant Three20 API Reference URL on<a href="http://api.three20.info/">api.three20.info</a>.

<a href="https://chrome.google.com/extensions/detail/nbkcgmaaghokgoidfhfdddlgolegpknf?hl=en">Install through the Google Chrome Extension Gallery</a>

You can find <a href="https://github.com/jwang/three20-chrome-extension">the source code for the Three20 Chrome Extension on GitHub</a>.
