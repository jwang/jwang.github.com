--- 
layout: post
title: Environmentally Friendly Programming
tags: 
- coding
- practices
categories: blog
---
[caption id="attachment_546" align="alignright" width="289" caption="Flickr photo from kimberlyfaye"]<a href="http://www.flickr.com/photos/kimberlyfaye/2628819393/"><img class="size-medium wp-image-546" title="Reduce, Reuse, Recycle" src="http://www.johntwang.com/blogturbo.paulstamatiou.com/uploads/2009/02/2628819393_6d7eb82ec4-289x300.jpg" alt="Flickr photo from kimberlyfaye" width="289" height="300" /></a>[/caption]

Environmentally Friendly thinking can apply to Programming. We're not talking about your programming environments here. What we are talking about is using the 3 R's, Reduce, Reuse, and Recycle as part of your Coding Practices. These principles are taught at most schools. At least it was at mine. However, when you start off on your own with no formal education, you can run into some problems. Let's take a look at these basic principles.
<h2>Reduce</h2>
Reducing code is fairly simple. Quite frequently used as part of defining "Beautiful Code," using the least amount of code to accomplish the task at hand. Since that's the case, it's also usually the hardest to accomplish. Minimizing processing time and file loading size. You can do even some small things to reduce your coding efforts. Such as using CMS plugins already written instead of writing your own, or even modifying existing plugins to do your bidding. Here's a very simple C example in reducing lines of code via initialization.
[sourcecode language='c']
void foo() {
    Complex c;
    c = (Complex)5;
}

void foo_optimized() {
    Complex c = 5;
}
[/sourcecode]
<h2>Reuse</h2>
How do we reuse code? Very simply, we use functions. Instead of copying the same code in various places, functions allow us to reuse the same code again. Similarly, using a global Debug boolean to turn on/off debugging is another way to save some time. Here is a small piece of code not performing reuse.
[sourcecode language='c']
unsigned int random_number1;
unsigned int random_number2;
rand_seed = rand_seed * 1103515245 +12345;
random_number1 = (unsigned int)(rand_seed / 65536) % 32768;
[/sourcecode]
And the same performing reuse.
[sourcecode language='c']
int rand()
{
rand_seed = rand_seed * 1103515245 +12345;
return (unsigned int)(rand_seed / 65536) % 32768;
}
int main() {
random_number1 = rand();
random_number2 = rand();
return 0;
}
[/sourcecode]

Simple and elegant. This also allows us to make changes to the function and having it take effect in all the places the function is called. Whereas, the opposite, would require us to go and make the changes in all of the places we used the same code.
<h2>Recycle</h2>
Recycling code can be fairly easy. Creating code clips/snippets that you frequently use and having them readily accessible is a great way to recycle code. Panic's Coda helps you do this via their "Clips" tool. Another way is to create general use codes such as reset.css, typography.css, etc. A sandbox starter theme for your CMS of choice is yet another way to recycle code. Recycling can cut down on the development time you need to do things significantly.
<h3>How do you use the 3R's in your programming practices?</h3>
