--- 
layout: post
title: Disqus Custom Author CSS WordPress Hack
tags: 
- css
- disqus
- php
- wordpress
categories: blog
---
<img src="http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/01/wordpress-disqus1.jpg" alt="wordpress-disqus" title="wordpress-disqus" width="499" height="113" class="aligncenter size-full wp-image-481" />
Custom CSS Styling for Post Authors are really nice. It's a great way to differentiate the author's comments from other readers' comments, in addition to threaded comments. The problem with the threaded comments, is that other readers may also write response comments which should be threaded to maintain conversational aspect. So styling helps much more.

<a href="http://www.disqus.com">Disqus</a> is a wonderful commenting system. I was introduced to it by reading <a href="http://www.louisgray.com/live/labels/disqus.html">Louis Gray's blog</a> from <a href="http://friendfeed.com/louisgray">FriendFeed</a>. Since using Disqus, I have found it to be a very great network. Disqus provides many features including:
<ul>
	<li>Threaded comments and comment ratings</li>
	<li>moderation and admin tools</li>
	<li>spam filters</li>
</ul><!--more-->
There are tons of <a href="http://www.wordpress.org">WordPress</a> comment hacks to do custom CSS styling for Author's comments. I have listed some at the bottom of the post. There's even plug-ins that will style author comments. Unfortunately, <a href="http://www.disqus.com">Disqus</a> does not offer up any solution to this. And after posting on the <a href="http://disqus.disqus.com/">Disqus forums</a>, I got no response for this feature. But, Disqus does offer some <a href="http://disqus.com/docs/css/">Custom CSS</a> for comments.
<h3>Out of the Box</h3>
Disqus actually does give us quite a bit of customization options. We are able to customize:
<ul>
	<li>The box where a post is typed into</li>
	<li>The form elements (Name, Email, Website).</li>
	<li>The submit button "Post".</li>
	<li>The main wrapper for the comment system.</li>
	<li>"Add New Comment" and "# Comments" are enclosed in &lt;h3&gt; tags.</li>
	<li>The toggle button for the thread options.</li>
	<li>The links within the thread Options</li>
	<li>The entire comment thread list.</li>
	<li>A single comment in the thread.</li>
	<li>The comment rating arrows for posts.</li>
	<li>The header at the top of comment posts.</li>
	<li>The avatar image for the registered user.</li>
	<li>This is the meta information about the post (time stamp and points).</li>
	<li>The message body of a single comment post.</li>
	<li>The footer contains the link to "reply."</li>
	<li>This contains and determines the style for the pagination links.</li>
</ul>
<h4>The Problem</h4>
As you can see, this is a lot of customizing power. Unfortunately, none of them is related to the <em>blog post's author</em>.

<h3>The Solution: Hack the Planet!</h3>
We are going to modify the <a href="http://wordpress.org/extend/plugins/disqus-comment-system/">official Disqus WordPress Plug-in</a>. The files being modified are: <em>comments.php</em> and <em>disqus.php</em> <strong>Note:</strong> We are also making the assumption that authors will use the same e-mail address to write posts and comments. This is generally true for most users.
<h4>Identifying the Post Author</h4>
First we need to find the post's author. We could just hardcode the email address, but this does not help multi-author websites. To find the author's email address we need to go into <em>diqus.php</em>

Inside <em>disqus.php</em> there is a function called: <em>dsq_comments_template</em>
Here we are going to add a global variable called <em>$author_email</em> and set it to <em>$author_email = get_the_author_email();
</em>

Here's the full function code:
[sourcecode language='php']function dsq_comments_template($value) {
global $dsq_response;
global $dsq_sort;
global $dsq_api;
global $post;
global $author_email; // Added by John Wang

if ( ! (is_single() || is_page() || $withcomments) ) {
return;
}

if ( !dsq_can_replace() ) {
return $value;
}

if ( dsq_legacy_mode() ) {
return dirname(__FILE__) . '/comments-legacy.php';
}

$author_email = get_the_author_email(); // Added by John Wang

$permalink = get_permalink();
$title = get_the_title();
$excerpt = get_the_excerpt();

$dsq_sort = get_option('disqus_sort');
if ( is_numeric($_COOKIE['disqus_sort']) ) {
$dsq_sort = $_COOKIE['disqus_sort'];
}
if ( is_numeric($_GET['dsq_sort']) ) {
setcookie('disqus_sort', $_GET['dsq_sort']);
$dsq_sort = $_GET['dsq_sort'];
}

// Call "get_thread" API method.
$dsq_response = $dsq_api->get_thread($post, $permalink, $title, $excerpt);
if( $dsq_response < 0 ) {
return false;
}
// Sync comments with database.
dsq_sync_comments($post, $dsq_response['posts']);

// TODO: If a disqus-comments.php is found in the current template's
// path, use that instead of the default bundled comments.php
//return TEMPLATEPATH . '/disqus-comments.php';

return dirname(__FILE__) . '/comments.php';

}[/sourcecode]

<h4>Identifying the Author's Comments</h4>
Now we know the blog post's author. Off to <em>comments.php</em>
Inside of <em>comments.php</em>, the first thing we need to do is add <em>$author_email</em> to the list of globals.

[sourcecode language='php']<?php echo("<?php" ); ?>
global $dsq_response, $dsq_sort,$author_email; // Added author_email variable!
$site_url = get_option('siteurl');
?>
[/sourcecode]

Once that's done, we need to find the comments and figure out if any of the comments were written by the <em>post author</em>. To do this, we'll be using the Disqus array <em>$comment['user']['email']</em>. This gives us the current comment's author's email address. We'll be comparing that to our global variable <em>$author_email</em>. On line 131 of <em>comments.php</em> we add the PHP if condition: <em>if ($comment['user']['email'] == $author_email ).</em>The complete line now looks like this:

[sourcecode language='php']<div class="dsq-comment-body <?php echo("<?php" ); ?> if ($comment['user']['email'] == $author_email ) echo ("-author"")?> ">[/sourcecode]

The same change can be made for the class <em>dsq-comment-header</em> on line 82, <em>dsq-header-avatar</em> on line 83, and <em>dsq-comment-footer</em> on line 154.

<h3>It's Styling Time!</h3>
<h4>Styling author's comments</h4>
Here's the easy part. All you need to do now is add the CSS code to <em>your</em> WordPress Theme for the appropriate classes. For example. This would change the background color for the author's comment.

[sourcecode language='css']
/*    Disqus CSS for Author Comments. Margin and Padding is set to be the same as other comments    */
#dsq-content #dsq-comments .dsq-comment-body-author {
	margin-left: 20px;
	padding-top: 5px;
	background: #e2fddc;
[/sourcecode]

<h4>The Results</h4>
Here's what an author's comment would look like now with a reader's comment.
<img src='http://www.johntwang.com/blog/wp-content/gallery/disqus/untitled.png' alt='Disqus' class='ngg-singlepic ngg-none' />

<a href='http://johntwang.local:8888turbo.paulstamatiou.com/uploads/2009/01/disqus-comment-system1.zip'>Download the complete modified plug-in.</a>.

<h3>Further Reading:</h3>
<ul>
	<li><a href="http://nettuts.com/working-with-cmss/weekend-tip-author-comment-styling-in-wordpress/">Weekend Tip: Author Comment Styling In WordPress - NETTUTS</a></li>
	<li><a href="http://5thirtyone.com/archives/774">How-to style WordPress author comments - Derek Punsalan - 5ThirtyOne</a></li>
	<li><a href="http://www.spoonfeddesign.com/wordpress-how-to-styling-author-comments">WordPress How-To: Styling Author Comments | Spoonfed Design</a></li>
	<li><a href="http://www.disqus.com/docs/css/">DISQUS | Custom CSS</a></li>
</ul>
<h3>Do you style your author's comments? Do you use something other than Disqus? Any feedback?</h3>

<h3>Update from Disqus.com</h3>
<blockquote>
<?php get_quotes("1138088841,1138091278") ?>
</blockquote>
