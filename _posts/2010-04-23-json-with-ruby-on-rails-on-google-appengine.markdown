--- 
layout: post
title: JSON with Ruby on Rails on Google AppEngine
tags: 
- appengine
- json
- ruby on rails
categories: blog
---
<h3>Getting Started</h3>
<p>Here's a list of what you'll need to get started with the demo:</p>
<ul class="styled">
<li>a Google AppEngine account</li>
<li>Ruby 1.8.6</li>
<li>Rails 2.3.5</li>
<li>Ruby on Rails AppEngine Gem</li>
</ul><!--more-->
<h4>Instructions</h4>
<p>The first steps that you'll want to follow are from John Woodell's Gist on <a href="http://gist.github.com/268192">Rails 2.3.5 on App Engine with DataMapper</a>.</p>
<p>As you'll notice, the last couple of steps generate a RESTful resource called Contact and also it's associated DataMapper Model.</p>

<h4>Adding REST</h4>
<p>Now in order to add JSON to the mix, we'll need a couple more things:</p>
<ul class="styled">
<li>JSON gem</li>
<li><a href="http://github.com/datamapper/dm-serializer/tree/">dm-serializer</a> gem</li>
</ul>
<p>We only have to make a few very small changes to the demo app that you got from the <a href="http://gist.github.com/268192">Gist</a> to get JSON integrated. They're listed below, and also at this <a href="http://gist.github.com/377353">Gist</a>.</p>

<h5>Gemfile changes: adding the <em>json-jruby</em> and <em>dm-serializer</em> to the gems for bundler</h5>
[sourcecode language="ruby"]
# Critical default settings:
disable_system_gems
disable_rubygems
bundle_path '.gems/bundler_gems'

# List gems to bundle here:
gem 'rails_dm_datastore'
gem 'rails', "2.3.5"
gem "json-jruby"
gem "dm-serializer"
[/sourcecode]
<h5>config.ru changes: adding <em>require 'dm-serializer'</em> and<em> 'json'</em></h5>
[sourcecode language="ruby"]
require 'appengine-rack'
require 'cgi'
require 'json'
require 'dm-core'
require 'dm-serializer'

AppEngine::Rack.configure_app(
    :application => 'iphone-json',
    :precompilation_enabled => true,
    :sessions_enabled => true,
    :version => "1")

AppEngine::Rack.app.resource_files.exclude :rails_excludes
ENV['RAILS_ENV'] = AppEngine::Rack.environment

deferred_dispatcher = AppEngine::Rack::DeferredDispatcher.new(
     :require => File.expand_path('../config/environment', __FILE__),
     :dispatch => 'ActionController::Dispatcher', :isolate => true)

map '/admin' do
  use AppEngine::Rack::AdminRequired
  run deferred_dispatcher
end

map '/user' do
  use AppEngine::Rack::LoginRequired
  run deferred_dispatcher
end

map '/' do
  run deferred_dispatcher
end
[/sourcecode]

<h5>and finally, the contacts_controller.rb needs the corresponding <em>format.json</em> render statements.</h5>
[sourcecode language="ruby"]
class ContactsController < ApplicationController
  # GET /contacts
  # GET /contacts.xml
  # GET /contacts.json
  def index
    @contacts = Contact.all

    respond_to do |format|
      format.html # index.html.erb
      format.xml  { render :xml => @contacts }
      format.json { render :json => @contacts }
    end
  end

  # GET /contacts/1
  # GET /contacts/1.xml
  # GET /contacts/1.json
  def show
    @contact = Contact.find(params[:id])

    respond_to do |format|
      format.html # show.html.erb
      format.xml  { render :xml => @contact }
      format.json { render :json => @contact }
    end
  end

  # GET /contacts/new
  # GET /contacts/new.xml
  # GET /contacts/new.json
  def new
    @contact = Contact.new

    respond_to do |format|
      format.html # new.html.erb
      format.xml  { render :xml => @contact }
      format.json { render :json => @contact }
    end
  end

  # GET /contacts/1/edit
  def edit
    @contact = Contact.find(params[:id])
  end

  # POST /contacts
  # POST /contacts.xml
  # POST /contacts.json
  def create
    @contact = Contact.new(params[:contact])

    respond_to do |format|
      if @contact.save
        flash[:notice] = 'Contact was successfully created.'
        format.html { redirect_to(@contact) }
        format.xml  { render :xml => @contact, :status => :created, :location => @contact }
        format.json { render :json => @contact, :status => :created, :location => @contact }
      else
        format.html { render :action => "new" }
        format.xml  { render :xml => @contact.errors, :status => :unprocessable_entity }
        format.json { render :json => @contact.errors, :status => :unprocessable_entity }
      end
    end
  end

  # PUT /contacts/1
  # PUT /contacts/1.xml
  # PUT /contacts/1.json
  def update
    @contact = Contact.find(params[:id])

    respond_to do |format|
      if @contact.update_attributes(params[:contact])
        flash[:notice] = 'Contact was successfully updated.'
        format.html { redirect_to(@contact) }
        format.xml  { head :ok }
        format.json { head :ok }
      else
        format.html { render :action => "edit" }
        format.xml  { render :xml => @contact.errors, :status => :unprocessable_entity }
        format.json { render :json => @contact.errors, :status => :unprocessable_entity }
      end
    end
  end

  # DELETE /contacts/1
  # DELETE /contacts/1.xml
  def destroy
    @contact = Contact.find(params[:id])
    @contact.destroy

    respond_to do |format|
      format.html { redirect_to(contacts_url) }
      format.xml  { head :ok }
      format.json { head :ok }
    end
  end
end
[/sourcecode]

<h3>Final Notes</h3>
<ol class="styled">
<li>You may have noticed that by default you get RESTful XML when you generated the resource. It doesn't work without the needed dm-serializer. Remember, we're using DataMapper here and not ActiveResource. If you try, you'll get this beautiful error.<br />
<code>SEVERE: [1272067889120000]  RuntimeError (Not all elements respond to to_xml)</code><br />
</li><br />
<li>If you inspect your DataMapper model, you'll notice that every field is a required field. This is key for when you're testing your POST method.<br /></li><br />
<li>The last thing that is currently being looked into, is that the POST requires you to pass in the Class in lowercase for it to properly work. I'm not sure if this is a case-sensitive issue, or just purely needing to change the param to be ":Contact" instead of ":contact" in the controller.<br /></li>
</ol>
<h3>Source Code</h3>
<ul class="styled">
<li>The fully working demo can be found running on Google AppEngine at: <a href="http://iphone-json.appspot.com">iphone-json.appspot.com</a></li>
<li>source is available on <a href="http://github.com/freshblocks/iPhone-JSON">Github</a>.</li>
<li>And there is an accompanying iPhone REST client also available on <a href="http://github.com/freshblocks/iPhone-JSON-Client">Github</a>. (Android client is currently in development.)</li>
</ul>
<p>Alternatively, if you just want to quickly test your newly created JSON RESTful resource, you can use the <a href="https://addons.mozilla.org/en-US/firefox/addon/9780">REST Client for Firefox</a> or <a href="https://chrome.google.com/extensions/detail/fhjcajmcbmldlhcimfajhfbgofnpcjmb">REST Client for Google Chrome.</a></p>
