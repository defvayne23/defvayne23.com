---
author: John Hoover
comments: true
date: 2010-05-07 14:30:46+00:00
layout: post
slug: using-jtube-basics
title: Using jTube - Basics
category: Code
tags:
- Javascript
- jQuery
- jTube
- Plugin
- Tutorial
- YouTube
---


This is the first post in a line of uses for jTube. In this post I will show the basics of getting a users upload list and using the info. Before we start make sure that you have included the latest jQuery and latest jTube.

For jTube to work you must tell it what feed to grab by passing the info to the correct option. The only one we need to worry about today is `user`. Using the users feed has an option that accompanies it. The option userType tells what info about the user to retrieve. By default jTube loads the users uploads. Lets take a look at that.

{% highlight javascript %}
<script>
$.jTube({
    user: 'defvayne23'
});
</script>
{% endhighlight %}

<!-- /excerpt -->

With the above code jTube gathers the users 'defvayne23' (me) uploads. But nothing is shown. We have to pass a success function so we can use the videos. The success function is passed two variables, videos and number of pages. The videos variable contains an array of videos, each containing info about that video.

{% highlight javascript %}
<script>
$.jTube({
    user: 'defvayne23',
    success: function(videos, pages) {
        $(videos).each(function() {
            alert(this.title);
        });
    }
});
</script>
{% endhighlight %}

Now that we have our success function, lets do something more useful than throw an alert box at the user. Lets create a list of the videos linked to the video page.

{% highlight javascript %}
<script>
$.jTube({
    user: 'defvayne23',
    success: function(videos, pages) {
        $(videos).each(function() {
            videoHTML = '<li>';
            videoHTML += '<a href="'+this.link+'" target="_blank">';
            videoHTML += this.title;
            videoHTML += '</a> - '+this.length;
            videoHTML += '</li>';

            $('#myVideos').append(videoHTML);
        });
    }
});
</script>
<ul id="myVideos"></ul>
{% endhighlight %}

Pretty simple. Loops the videos. With each video creates a list item, and within it a link on the title to the video and the video link next to the title. Then appends the HTML to the list. The above would produce: (I don't have many videos, so this list is for `twofivethreetwo)

{% highlight javascript %}
<ul id="myVideos">
<li><a href="http://www.youtube.com/watch?v=7_Iw2nEROBg&feature=youtube_gdata" target="_blank">Changing | 1 Minute Short</a> - 1:01</li>
<li><a href="http://www.youtube.com/watch?v=wrsI4GXOzdc&feature=youtube_gdata" target="_blank">Chase Spelling</a> - 2:42</li>
<li><a href="http://www.youtube.com/watch?v=VqroSjIlrJc&feature=youtube_gdata" target="_blank">City floods the street again</a> - 0:44</li>
<li><a href="http://www.youtube.com/watch?v=ZPwy27-lj70&feature=youtube_gdata" target="_blank">At DQ going to Kansas</a> - 0:55</li>
<li><a href="http://www.youtube.com/watch?v=7xqVnhYTbKc&feature=youtube_gdata" target="_blank">Spray Park</a> - 3:55</li>
</ul>
{% endhighlight %}

What happens when something goes wrong with getting the feed like youtube being down. Lets add the option error to handle when we get an error.

{% highlight javascript %}
<script>
$.jTube({
    user: 'defvayne23',
    success: function(videos, pages) {
        $(videos).each(function() {
            videoHTML = '<li>';
            videoHTML += '<a href="'+this.link+'" target="_blank">';
            videoHTML += this.title;
            videoHTML += '</a> - '+this.length;
            videoHTML += '</li>';

            $('#myVideos').append(videoHTML);
        });
    },
    error: function(error) {
        $('#myVideos').append('<li class="error">'+error+'</li>');
    }
});
</script>
<ul id="myVideos"></ul>
{% endhighlight %}

Pretty simple. We put the error in the same place we put our videos, but added a class of error so we could style it with maybe a red border to let the user know we had issues.

This is only a small portion of what you can do with jTube. Next post will cover how to handle paging and number of videos.

### Keep up with jTube

  * [GitHub](http://github.com/monkeecreate/jTube)
  * [jQuery Plugins](http://plugins.jquery.com/project/jTube)
  * [My Twitter](http://twitter.com/defvayne23)
  * [monkeeCreate Twitter](http://twitter.com/monkeecreate)
