--- 
layout: post
title: Remove the Heroku Toolbelt
tags: heroku
categories: blog
---
If for some reason you installed the [Heroku Toolbelt](http://toolbelt.herokuapp.com/osx/readme) and want to remove it. You can do so by running these 2 commands:
`rm -rf /usr/local/heroku`
`rm -rf /usr/bin/heroku`

Unfortunately, these aren't documented on the site yet and it gave me some issues with my heroku accounts plugin and heroku ruby gem.