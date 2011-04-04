--- 
layout: post
title: PostgreSQL Preference Pane
tags: [postgres, postgresql]
categories: blog
---
I recently found myself having introducing interns to Ruby on Rails development with both mySQL and PostgreSQL.  The downloadable version of mySQL found on [mysql.com](http://dev.mysql.com/downloads/mysql/) comes with a Preference Pane for Mac OS X.  The prefpane allows users to do a few small things.  Primarily, view the status of the database server, start and stop the server, and toggle the auto-start option.

For [PostgreSQL](http://www.postgresql.org), we install using [HomeBrew](http://mxcl.github.com/homebrew/).  There's no accompanying Preference Pane when installed via:

`brew install postgresql`

I also looked around on the PostgreSQL download section and couldn't find anything.  A couple of Google searches yielded some outdated information, but a very interesting framework for Cocoa called [postgres-kit](http://code.google.com/p/postgres-kit/).  I want to look a bit more into it at a later point, but I also needed something that was bit more quick and easy for the interns.

The Terminal could be a bit more user friendly for Designers. Wise Heart Design has a [intro guide to the Terminal for Designers.](http://wiseheartdesign.com/articles/2010/11/12/the-designers-guide-to-the-osx-command-prompt/).  It's worth a read if you're not familiar with the terminal and shell commands.

It's also been a pain point for learning as, there's long commands to learn, or we need to teach them how to create _aliases_ in the bash/zsh shell.  Something we want to avoid and get through quickly, so we can get the designers access to the CSS and view layers of the app for them to style.

This gave me an interesting option of creating a Pref Pane for Postgres that mirrors the mySQL pref pane. 

![PGPane Screenshot](https://github.com/jwang/pgpane/raw/master/Postgres.png)

It's available for [download](https://github.com/downloads/jwang/pgpane/Postgres.prefPane) and if installed with HomeBrew, does not need admin access or permissions to run.  The code is open source and available on [GitHub](https://github.com/jwang/pgpane).

There's still a few things here and there that I may eventually try to add, but for the most part, it's a decent functional alpha.  Perhaps, I'll go through the exercise of rewriting it in [MacRuby](http://macruby.com).