---
author: John Hoover
comments: true
date: 2010-09-30 13:40:18+00:00
layout: post
slug: jtube-v2-0-officially-released
title: jTube v2.0 Officially Released
category: Code
tags:
- Javascript
- jQuery
- jTube
- Plugin
- YouTube
---

Proud to announce that jTube has reached the milestone of v2.0. This update brings:

  * Video info based on video ID
  * Related videos
  * Video comments
  * Video responses
  * Usage Examples
  * Bug fixes

<!-- /excerpt -->

Instead of having to deal with multiple options, they have been consolidated down to three:

{% highlight javascript %}
$.jTube({
  request: 'user',
	requestValue: 'defvayne23',
	requestOption: 'uploads',
	limit: 10,
	page: 1,
	success: function(videos){
		...
	}
});
{% endhighlight %}

### Get the latest jTube

  * [GitHub](https://github.com/defvayne23/jTube/tags)
  * [jQuery Plugins](http://plugins.jquery.com/project/jTube)

### Questions/Comments

  * Leave a comment below
  * [Contact me directly](/contact/)
  * [Find me on Twitter](http://twitter.com/defvayne23)
