---
author: John Hoover
comments: true
date: 2010-02-18 16:51:32+00:00
layout: post
slug: tct-crawl-a-website
title: 'TCT: Crawl a Website'
category: Code
tags:
- PHP
- Thursday Code Tip
- Tip
---

DISCLAIMER: I would like to say I do not condone doing this. Better ways, more legal, ways to get content from someone. But sometimes this is asked of you by your boss. DO NOT STEAL CONTENT.

For this weeks Thursday Code Tip I will show how to use PHP to crawl a website to gather content. First we start by selecting the URL to crawl:

{% highlight php %}
$sURL = "http://www.defvayne23.com/";
{% endhighlight %}

Next we get the content of the page:

{% highlight php %}
$sContent = file_get_contents($sURL);
{% endhighlight %}

<!-- /excerpt -->

Now to use REGEX to get what we want. You can learn patterns here. Below we search for the text within a H1 tag.

{% highlight php %}
$sPattern = '/<h1>([a-z0-9\s]+)<\/h1>/i';
preg_match($sPattern, $sContent, $aMatches);
{% endhighlight %}

The above won’t return anything because I link all my H1′s. So lets modify it so it will account for the links, but not gather them.

{% highlight php %}
$sPattern = '/<h1><a [^>]+>([a-z0-9\s]+)<\/a><\/h1>/i';
{% endhighlight %}

Now that we account for the anchor the above should return:

	Array
	(
	    [0] => Defvayne23
	    [1] => Defvayne23
	)

The first part of the array is the HTML it found including the h1 and anchor. The second is just the text that we where looking for.

Here it is all together:

{% highlight php linenos %}
$sURL = "http://www.defvayne23.com/";
$sContent = file_get_contents($sURL);
$sPattern = '/<h1><a [^>]+>([a-z0-9\s]+)<\/a><\/h1>/i';
preg_match($sPattern, $sContent, $aMatches);
$sHeader = $aMatches[1];
{% endhighlight %}
