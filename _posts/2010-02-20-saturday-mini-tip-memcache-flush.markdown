---
author: admin
comments: true
date: 2010-02-20 19:26:39+00:00
layout: post
slug: saturday-mini-tip-memcache-flush
title: 'Saturday Mini Tip: Memcache flush'
category: Code
tags:
- PHP
- Saturday Mini Tip
---

[Memcache](http://php.net/manual/en/ref.memcache.php) can be great to store variables, but beware of [Memcache::flush](http://www.php.net/manual/en/function.memcache-flush.php). The function actually requires about 1 second to finish. Below is the best way to handle it:

{% highlight php %}
$oMemcache->flush();
 
// Wait for memcache to finish flushing
$time = time()+1; //one second future
while(time() < $time) {
//sleep
}
{% endhighlight %}

You can find more info [here](http://www.php.net/manual/en/function.memcache-flush.php#81420).