--- 
layout: post
title: Fuzzy and Document Searching with Solr
tags: [rails, websolr, sunspot]
categories: blog
---
Recently, I needed to get add Searching into a web app. I decided to use Sunspot and Solr. Luckily, Ryan Bates has already done a [Sunspot Railscasts](http://railscasts.com/episodes/278-search-with-sunspot), which got me started. However, I needed Fuzzy and Document Searching. The first part is actually fairly trivial thanks to the [Wildcard searching with ngrams](https://github.com/outoftime/sunspot/wiki/Wildcard-searching-with-ngrams) and many other results from doing a simple Google Search.

### Fuzzy Searching
For the fuzzy searching, it was a simple matter of changing the configuration for *text* fieldType to:
{% highlight xml %}
    <fieldType name="text" class="solr.TextField" omitNorms="false">
      <analyzer type="index">
          <tokenizer class="solr.StandardTokenizerFactory"/>
          <filter class="solr.StandardFilterFactory"/>
          <filter class="solr.LowerCaseFilterFactory"/>
          <filter class="solr.EdgeNGramFilterFactory" minGramSize="2" maxGramSize="15"/>
        </analyzer>
      <analyzer type="query">
        <tokenizer class="solr.StandardTokenizerFactory"/>
        <filter class="solr.StandardFilterFactory"/>
        <filter class="solr.LowerCaseFilterFactory"/>
        <filter class="solr.PorterStemFilterFactory"/>
        <filter class="solr.PhoneticFilterFactory" encoder="DoubleMetaphone" inject="true"/>
      </analyzer>
    </fieldType>
{% endhighlight %}

### Document Searching
To add searching of attachments, I needed to add the [Sunspot Cell add-on](https://github.com/chebyte/sunspot_cell). It's important to note, that this is a fork of the Sunspot Cell that is linked from the main Sunspot Wiki. [The reference for it, is by Mauro Torres](http://www.chebyte.com/2011/08/13/how-to-index-file-contents-like-pdf-doc-etc-with-solr-sunspot-paperclip-s3-and-rails-3/) who's fork is being used.

The first step was to download the [Solr Cell dependencies](https://gist.github.com/1196190) that are needed and install the plugin through *rails plugin install*. They all go into the solr/lib with the rest of Sunspot's built in Solr server. I have uploaded them into 2 separate zip files. [File 1](http://cl.ly/9tFw). [File 2](http://cl.ly/9tV4). Combined, they are 26MB large. that's why it's not in the main sunspot project.

After that, the schema needs to be altered to include the *_attachment* dynamic type.
{% highlight xml %}
    <dynamicField name="*_attachment" stored="true" type="text" multiValued="true" indexed="true"/>     
    <dynamicField name="ignored_*" type="ignored"/>
{% endhighlight %}

### Searching Amazon S3 Documents
Since I'm deploying to Heroku and Heroku doesn't support writing to the filesystem for persistent storage, we use the recommended Amazon S3. You can use other systems, such as Google Storage for Developers or RackSpace CloudFiles. I use Paperclip for handling file uploads.

Due to the S3 needs. We need to grab the URL and use that for the attachment search. This is a simple as changing the *searchable* to use a private method like so:

{% highlight ruby %}
class Asset
	has_attached_file :file,
		storage => :s3,
		:s3_credentials => "#{Rails.root}/config/amazon_s3.yml"

    searchable do
       text :title
       attachment :attached_file
     end

  private
    def attached_file
      URI.parse(file.expiring_url(60))
    end
  end
{% endhighlight %}

### Deployment with Heroku and WebSolr
I deployed to Heroku. The easiest way I found to deal with the Solr system was to use the [WebSolr Addon](http://addons.heroku.com/websolr) instead of trying to bring up my own Solr server. The starting point is $20/month. Not too terrible. You will need to check the *PDF/MS Word/etc Indexing* checkbox in the *Basic Configuration* section. Then in the *Advanced Configuration* copy and paste your *schema.xml* that you have configured for the document search. 

### Gotchas
There seems to still be a bug in the sunspot gem when it is used for fuzzy and document searching. It seems to be due to multiple matches in a document. The issue is with [line 230 of the abstract_search.rb file](https://github.com/outoftime/sunspot/blob/master/sunspot/lib/sunspot/search/abstract_search.rb#L230). I recall seeing a pull request being sent to the sunspot repository, but it doesn't look like it's been merged in yet. I forked the gem and [patched it](https://github.com/jwang/sunspot/commit/1154a62f261fd8679d2a29129deaafa67bde66b3). I'm using the git url to get this working until it is merged back into the upstream gem repo.

Note that you need to invoke the sunspot rake command to reindex all the items you have already uploaded. The system will only index new uploads and not existing without invoking the rake command.

### TL;DR
The [full schema.xml can be downloaded from the GitHub Gist](https://gist.github.com/1196146). Download the Solr Cell Dependencies [[Part 1](http://cl.ly/9tFw) [Part 2](http://cl.ly/9tV4)]. And put them in the solr/lib with the other files.