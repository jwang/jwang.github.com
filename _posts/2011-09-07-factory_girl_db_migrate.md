---
layout: post
title: Factory Girl Blocking Rake Database Migrations
tags: [ruby, rails, factory_girl]
categories: blog
---
I use a couple of items for doing Behavior Driven Development. Primarily, [Cucumber](http://cukes.info) and [RSpec](http://relishapp.com/rspec). In conjunction with those, I use [Factory Girl](https://github.com/thoughtbot/factory_girl) and [Shoulda](https://github.com/thoughtbot/shoulda-matchers).

Sometimes, it seems that Factory Girl can get in the way of running `rake db:migrate` when you push your code up to your source control system, in my case [GitHub](http://github.com), for others download and run it. Such was the case with the [Jenkins CI](http://jenkins-ci.org) server.

#### The Issue
The problem was that Factory Girl attempts to create factories for models that did not have database tables created yet. It was trying to run before the database migrations ran when the server tried running the `rake db:migrate`. (Note, yes, we always prepend `bundle exec`, but shortened it here for context.)

#### The Solution
It seems that you're not to load the `factory\_girl_rails` gem in your Gemfile in the `:development` group at all. The configuration on [the GitHub repo](https://github.com/thoughtbot/factory_girl_rails) just says to add it to your Gemfile, but instead you should add it to your `:test` group only. For example:
`gem 'factory_girl_rails', :group => :test`

More details can be found on [the mailing list topic](https://groups.google.com/d/topic/factory_girl/0RtdwiUIm5I/discussion).