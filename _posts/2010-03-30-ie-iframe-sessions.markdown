---
author: John Hoover
comments: true
date: 2010-03-30 16:41:39+00:00
layout: post
slug: ie-iframe-sessions
title: IE & IFRAME Sessions
category: Code
tags:
- IE
- Tip
---

Had an issue come up today with a site that offers the service to pull in data to your site using an iframe. Apparently IE has a security feature where it does not accept sessions from external sites using iframe. Sounds great, but the way to make it work is to simply send a header with the request.

{% highlight php %}
header('P3P: CP="CAO PSA OUR"');
{% endhighlight %}

So how does this add security? [Find more info here.](http://support.microsoft.com/kb/323752)
