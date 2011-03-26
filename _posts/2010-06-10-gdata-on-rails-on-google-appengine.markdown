--- 
layout: post
title: GData on Rails on Google AppEngine
tags: 
- appengine
- gdata
- ruby on rails
categories: blog
---
<p>There's a great article by Jeff Scudder on <a href="http://code.google.com/appengine/articles/gdata.html">Retrieving Authenticated Google Data Feeds with Google App Engine</a> on the Google App Engine Articles site. Jeff's tutorial utilizes the <a href="http://code.google.com/p/gdata-python-client">GDdata (Google Data) Python client library</a>. The aim of this article is to replicate Jeff's Tutorial using <a href="http://code.google.com/apis/gdata/articles/gdata_on_rails.html">GData on Rails</a> on Google App Engine running on JRuby on Rails.</p>
<!--more-->

<h4>Requirements</h4>
<ul>
<li>Ruby 1.8.6 patch level 114+</li>
<li>RubyGems 1.3.1+</li>
<li>Rails 2.3.5</li>
<li>GData Gem</li>
</ul>

<h3>Introduction</h3>
<p>I'm sure your mind is positively buzzing with ideas for how to use Google App Engine, and a few of you might be <a href="http://code.google.com/apis/gdata/overview.html">Google Data</a> AtomPub APIs. Quite of few of Google's products expose a Google Data API, (a few interesting examples are YouTube, Google Calendar, and Blogger--you can find a complete <a href="http://code.google.com/apis/gdata/">list here</a>) and these APIs can be used to read and edit the user-specific data they expose.</p>

<p>In this article we'll use the Google Documents List Data API to walk through the process of requesting access from and retrieving data for a particular user. We'll use Google App Engine's <a href="http://code.google.com/appengine/docs/python/tools/webapp/">webapp framework</a> to generate the application pages, and the <a href="http://code.google.com/appengine/docs/python/users/">Users API</a> to authenticate users with Google Accounts.</p>

<h3>Google's AuthSub APIs</h3>
<p>Some Google Data services require authorization from your users to read data, and all Google Data services require their authorization before your app can write to these services on the user's behalf. Google uses <a href="http://code.google.com/apis/accounts/docs/AuthForInstalledApps.html">AuthSub</a> to enable users to authorize your app to access specific services.</p>

<p>Using AuthSub, users type their password into a secure page at google.com, then are redirected back to your app. Your app receives a token allowing it to access the requested service until the user revokes the token through the <a href="https://www.google.com/accounts/ManageAccount">Account Management</a> page.</p>

<p>In this article, we'll walk through the process of setting up the login link for the user, obtaining a session token to use for multiple requests to Google Data services, and storing the token in the datastore so that it can be reused for returning users.</p>

<h3>Using the GData on Rails client library</h3>
<p>Google offers a Data on Rails client library that simplifies token management and requesting data from specific Google Data APIs. In this article we'll use this library, but of course you're welcome to use whatever works best for your application. <a href="http://code.google.com/p/gdata-ruby-util/">Download the Gdata on Rails client library</a>.</p>

<p>To use this library with your Google App Engine application, simply install the library gem and require it as you usually would. There's nothing more to it than that!</p>
<h4>Installing the Google Data Ruby Utility Library</h4>
<p>To obtain the library, you can either download the library source directly from project hosting or install the gem:</p>
<code>sudo gem install gdata</code>

<h5>config/enviroment.rb</h5>
<code>config.gem 'gdata', :lib => 'gdata'</code>

<h5>config.ru</h5>
[sourcecode language="ruby"]
require 'appengine-rack'
require 'image'
require 'image-upload'
require 'user'
require 'gdata'
AppEngine::Rack.configure_app(
    :application => 'jruby-gdata',
    :precompilation_enabled => true,
    :sessions_enabled => true,
    :version => "1")

AppEngine::Rack.app.resource_files.exclude :rails_excludes
ENV['RAILS_ENV'] = AppEngine::Rack.environment

deferred_dispatcher = AppEngine::Rack::DeferredDispatcher.new(
    :require => File.expand_path('../config/environment', __FILE__),
    :dispatch => 'ActionController::Dispatcher')

map '/contacts' do
  use AppEngine::Rack::LoginRequired
  run deferred_dispatcher
end

map '/' do
  run deferred_dispatcher
end
[/sourcecode]

<h5>Gemfile</h5>
[sourcecode language="ruby"]
# Critical default settings:
disable_system_gems
disable_rubygems
bundle_path '.gems/bundler_gems'

# List gems to bundle here:
gem 'rails_dm_datastore'
gem 'rails', "2.3.5"
gem 'appengine-apis'
gem 'gdata'
[/sourcecode]

<h3>Step 1: Generating the Token Request Link (AuthSub)</h3>
<p>Applications use an API called <a href="http://code.google.com/apis/accounts/docs/AuthForWebApps.html">AuthSub</a> to obtain a user's permission for accessing protected Google Data feeds. The process is fairly simple. To request access from a user to a protected feed, your app will redirect the user to a secure page on google.com where the user can sign in to grant or deny access. Once doing so, the user is then redirected back to your app with the newly-granted token stored in the URL.</p>

<p>Your application needs to specify two things when using AuthSub: the common base URL for the feeds you want to access, and the redirect URL for your app, where the user will be sent after authorizing your application.</p>

<p>To generate the token request URL, we'll use the <code>GData::Auth::AuthSub</code> module included in the Google Data client library. This module contains a method, get_url, which automatically generates the correct URL given the base feed URL(scope) and your website's return address (next_url). In the code snippet below, we use this method to generate a URL requesting access to a user's Google Document List feed.</p>

<p>In our routes.rb file, we will define a URL mapping to create a separate URL for each step. Here's an example:</p>
[sourcecode language="ruby"]
    map.connect 'step1', :controller => 'step1', :action => 'get'
    map.connect 'step2', :controller => 'step2', :action => 'get'
    map.connect 'step3', :controller => 'step3', :action => 'get'
[/sourcecode]

<p>To illustrate this first step of using AuthSub in the app, we will create a step1_countroller.rb that looks something like this:</p>
[sourcecode language="ruby"]
class Step1Controller < ApplicationController

  def get
    scope = 'http://docs.google.com/feeds/'
    next_url = url_for :controller => self.controller_name, :action => self.action_name
    secure = false  # set secure = true for signed AuthSub requests
    session = true
    @authsub_link = GData::Auth::AuthSub.get_url(next_url, scope, secure, session)
  end
end
[/sourcecode]

<h3>Step 2: Retrieving and Updating a Token</h3>
<p>Once we've generated an authorization request URL for a particular Google Data service, we'll need a way to use the token returned to our app to access the feed in question. Now, we need to retrieve the initial token returned to us for the Google Documents List API, and upgrade that token to a permanent session token. Remember that we told the service to redirect the user to the URL 'http://jruby-gdata.appspot.com/step1'. Let's extend our simple example above to do a few things. We'll call this new version step2.</p>

<p>Let's write the functionality that will handle the return request from the Google Data service the user signed in to. The Google Data service will request a URL that will look something like this:</p>
<code>http://jruby-gdata.appspot.com/?auth_sub_scopes=http%3A%2F%2Fdocs.google.com%2Ffeeds%2F&token=CKC5y...Mg</code>
<p>Which is just our return URL appended with the initial authorization token for the service which grants our app access for our user. The code below first takes this URL and extracts the service and the token. Then, it requests an upgrade for the token for the document list service.</p>

<p>We use two new methods to achieve this. First, we try to obtain the single use AuthSub token by examining the current page's URL. The params[:token] function handles token extraction for us. To upgrade this initial token to a session token, we use <code>client.auth_handler.upgrade()</code>.</p>

[sourcecode language="ruby"]
require 'appengine-apis/users'

class Step2Controller < ApplicationController
  def get
    @client = GData::Client::DocList.new
    next_url = url_for :controller => self.controller_name, :action => self.action_name
    secure = false  # set secure = true for signed AuthSub requests
    sess = true
    @authsub_link = @client.authsub_url(next_url, secure, sess)

    if params[:token].nil? and session[:token].nil?
      @url = "nothing"
    elsif params[:token] and session[:token].nil?
      @client.authsub_token = params[:token] # extract the single-use token from the URL query params
      session[:token] = @client.auth_handler.upgrade()
    end

    @url1, @url_linktext = if AppEngine::Users.current_user
        [AppEngine::Users.create_logout_url(request.url), 'Sign Out']
      else
        [AppEngine::Users.create_login_url(request.url), 'Sign In']
      end

    @client.authsub_token = session[:token] if session[:token]

  end
end

[/sourcecode]

<p>After we upgrade the initial token using the <code>client.auth_handler.upgrade()</code> method. Below, we will take you through the steps to use this token and fetch your user's feed in your application.</p>

<h3>Step 3: Using a session token and fetching a data feed.</h3>
<p>Now that we have obtained and stored the session token, we can use the AuthSub session token to retrieve the user's document list feed with our application. The final step is to get the user feed from Google Docs and display it on our site!</p>

<p>Lets add a new method to our app to request the feed and handle a token required message. We will call this method fetch_feed and add it to the Fetcher request handler class. In this example, the app uses client.Get to try to read data from the feed.</p>

<p>Some Google Data feeds require authorization before they can be read. If our app had previously saved an AuthSub session token for the current user and the desired feed URL, then the token will be found automatically by the client object and used in the request. If we did not have a stored token for the combination of the current user and the desired feed, then we will attempt to fetch the feed anyway. If we receive a "token required" message from the server, then we will ask the user to authorize this app which will give our app a new AuthSub token.</p>

<p>Here is the code for the fetch_feed method:</p>

[sourcecode language="ruby"]
def fetch_feed (client, feed_url)

  if feed_url.nil?
    'No feed_url was specified for the app to fetch.<br/>'
  else
      begin
        @feed = @client.get(feed_url).to_xml
        # If fetching fails
        rescue GData::Client::AuthorizationError
          @error = 'authorization error'
      end
  end
end
[/sourcecode]


<p>Now that we have a method to fetch the target feed, we can modify the Fetcher class' get method to call this method after we upgrade and store the AuthSub token.</p>

<p>Our app also needs to know the URL which should be fetched, so we add a URL parameter to the incoming request to indicate which feed should be fetched. The below code for the get method adds the ability to find out which URL the app should fetch and fetches the desired feed.</p>

[sourcecode language="ruby"]
require 'appengine-apis/users'
require 'appengine-apis/urlfetch'

class Step3Controller < ApplicationController

  def get
    @client = GData::Client::DocList.new
    next_url = url_for :controller => self.controller_name, :action => self.action_name
    secure = false  # set secure = true for signed AuthSub requests
    sess = true
    @authsub_link = @client.authsub_url(next_url, secure, sess)

    if params[:token].nil? and session[:token].nil?
      @url = "nothing"
    elsif params[:token] and session[:token].nil?
      @client.authsub_token = params[:token] # extract the single-use token from the URL query params
      session[:token] = @client.auth_handler.upgrade()
    end


    @url1, @url_linktext = if AppEngine::Users.current_user
        [AppEngine::Users.create_logout_url(request.url), 'Sign Out']
      else
        [AppEngine::Users.create_login_url(request.url), 'Sign In']
      end

    @client.authsub_token = session[:token] if session[:token]

    feed_url = params[:feed_url]

    if feed_url.nil?
      @sample_url = next_url + '?feed_url=http%3A%2F%2Fdocs.google.com%2Ffeeds%2Fdocuments%2Fprivate%2Ffull'

    end

    fetch_feed(@client,feed_url)

  end
end
[/sourcecode]

<p>In the above, we request the feed by calling <code>fetch_feed(@client, feed_url)</code>.</p>

<p>You can see the final program at work by visiting: <a href="http://jruby-gdata.appspot.com/">http://jruby-gdata.appspot.com/</a>. Also, view the complete source code, where we put all of this together at the <a href="http://code.google.com/p/jruby-google-app-engine-samples/">Google App Engine sample code project</a> on Google Code Hosting.</p>

<p>The AuthSub session tokens are long lived, but they can be revoked by the user or by your application. At some point, a session token stored in your data store may become revoked so your application should handle cleanup of tokens which can no longer be used. The status of a token can be tested by <a href="http://code.google.com/apis/accounts/docs/AuthForWebApps.html#AuthSubTokenInfo">querying the token info URL.</a> You can read more about AuthSub token management in the <a href="http://code.google.com/apis/accounts/docs/AuthForWebApps.html">AuthSub documentation.</a> This feature is left as an exercise to the reader, have fun :)</p>

<h3>Conclusion</h3>
<p>Using the Google Data on Rails client library, you can easily manage your user's Google Data feeds in your own Google App Engine application.</p>

<p>The Google Data on Rails client library includes support for almost all of the Google Data services. For further information, you can read the <a href="http://code.google.com/apis/gdata/articles/gdata_on_rails.html">getting started guide</a> for the library, visit the <a href="http://code.google.com/p/gdata-ruby-util/">project</a> to browse the source, and even ask questions on the 's Google group.</p>

<p>As always, for questions about Google App Engine, read Google's <a href="http://code.google.com/appengine/docs">online documentation</a> and visit our <a href="http://groups.google.com/group/google-appengine">google group.</a></p>

<h3>Appendix: ClientLogin</h3>
<p>This example uses AuthSub to authorize the app to act on the user's behalf, but the Google Data APIs support other authorization mechanisms. In some cases, you might want to temporarily use <a href="http://code.google.com/apis/accounts/docs/AuthForInstalledApps.html">ClientLogin</a> while developing your application. If you are going to use ClientLogin, the Ruby library has a client class for each of the APIs and a Base class:</p>

[sourcecode language="ruby"]
client = GData::Client::DocList.new
client.clientlogin('user@gmail.com', 'pa$$word')

client_login_handler = GData::Auth::ClientLogin.new('writely', :account_type => 'HOSTED')
token = client_login_handler.get_token('user@example.com', 'pa$$word', 'google-RailsArticleSample-v1')
client = GData::Client::Base.new(:auth_handler => client_login_handler)
[/sourcecode]

<p>You may receive a CAPTCHA challenge when requesting a ClientLogin token which you will need to handle in your app before you can receive a ClientLogin token. For this and a few other reasons, I don't recommend using ClientLogin in Google App Engine, but the above is how you could use it while developing your app.</p>

<h4>Resources</h4>
<ul>
	<li><a href="http://code.google.com/appengine/docs/python/howto/usinggdataservices.html">Using Google Data Services - Google App Engine</a></li>
	<li><a href="http://code.google.com/appengine/articles/gdata.html">Retrieving Authenticated Google Data Feeds with Google App Engine</a> with Python</li>
	<li><a href="http://code.google.com/apis/gdata/articles/gdata_on_rails.html">GData on Rails</a></li>
	<li><a href="http://code.google.com/apis/gdata/articles/using_ruby.html">Using Ruby with the Google Data APIs - Google Data Protocol - Google Code</a></li>
</ul>
