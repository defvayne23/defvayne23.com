---
author: John Hoover
comments: true
date: 2010-06-12 14:53:51+00:00
layout: post
slug: jtube-not-just-for-javascript
title: jTube - Not Just for Javascript
category: Code
tags:
- jTube
- PHP
- YouTube
---

My jQuery YouTube API plugin, jTube, has now been ported to a PHP class. The two work similar in the fact that you set a query type (user uploads, feed, playlist, etc.) and set some options. Below is an example:

{% highlight php %}
setQuery("user", "defvayne23", "uploads");
$oVideos->setOption("limit", 5);

try {
	$aVideos = $oVideos->runQuery();
} catch(Exception $e) {
	die($e->getMessage());
}

// Use $aVideos
{% endhighlight %}

With the PHP class jTube can now make use of authentication and with that the entire YouTube API. The YouTube API allows for anything from uploading a video to supplying feedback on a video (rate, comment, flag). The PHP class already can make use of authentication tokens to retrieve private info for the user that belongs to the token. The first new functionality coming to the PHP class is video managing and upload.

[Checkout jTube for PHP on github now.](http://github.com/monkeecreate/jtube-php)
