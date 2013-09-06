---
author: admin
comments: true
date: 2010-05-14 16:08:21+00:00
layout: post
slug: using-jtube-video-and-thumbnail
title: Using jTube - Video and Thumbnail
category: Code
tags:
- Javascript
- jQuery
- jTube
- Plugin
- Tutorial
- YouTube
---

This is the final post of the series that has so far covered the [very basics of showing a list of video titles](/2010/05/07/using-jtube-basics/), [to paging of the list](/2010/05/12/using-jtube-paging/) using jTube. In this post I will discuss how to embed a video using jTubeEmbed which comes with the jTube jQuery plugin, also how to show the video thumbnail provided by YouTube. To show these features off I will modify where we left off in the [paging tutorial](/2010/05/12/using-jtube-paging/) and show the video for the first entry of the page then show the thumbnail for the other videos on the page.

To show the video only on the first video we need to modify the [each loop](http://api.jquery.com/each/) to tell us where we are in the loop. Simply add `index` as the first parameter received.

{% highlight javascript %}
$(videos).each(function(index) {
{% endhighlight %}

<!-- /excerpt -->

Now that we know what position we are in the loop, we can check if we are on the first video. Simply add an if statement that checks if the index is 0 (first). If we are on the first, embed the video, otherwise show the link with the title and length. To embed the video we will use the jTubeEmbed function. Only thing it requires is the video url. That is given with the key `video`. In this tutorial we can access that with `this.video`. If you choose not to pass the width and height option, the embed defaults to 290x250px. In our demo we will pass in the dimensions  200x172px so they fit snug in our page. These options are passed in similar to jTube but they go in as a second parameter. We should also show the video title below the video, but not link it since we already have the video.

{% highlight javascript %}
videoHTML = '<li>';

if(index == 0) {
    videoHTML += $.jTubeEmbed(this.video, {width: 200, height: 172});
    videoHTML += this.title;
} else {
    videoHTML += '<a href="'+this.link+'" target="_blank">';
    videoHTML += this.title;
    videoHTML += '</a> - '+this.length;
}

videoHTML += '</li>';
{% endhighlight %}

Now that we [have the video showing up](/demo/using-jtube-video-and-thumbnail/beta.html) lets add the thumbnail. Each video that jTube returns we have a url for the thumbnail, accessible with the key `thumbnail`. This will return the image url which you can put as the source in your img tag. To not overflow the page with large thumbnails, also set the width to 200px to match the video and height to 150px to keep the aspect ratio.

{% highlight javascript %}
videoHTML += '<a href="'+this.link+'" target="_blank">';
videoHTML += '<img src="'+this.thumbnail+'" height="150" width="200"><br>';
videoHTML += this.title;
{% endhighlight %}

That should sum it up for this series of jTube tutorials. I have now covered [getting users uploaded videos](/2010/05/07/using-jtube-basics/), [success function](/2010/05/07/using-jtube-basics/), [error function](/2010/05/07/using-jtube-basics/), [paging](/2010/05/12/using-jtube-paging/), [limit number of videos per page](/2010/05/12/using-jtube-paging/), [videos from YouTube feeds](/2010/05/12/using-jtube-paging/), embed video using jTubeEmbed, and thumbnails.

Next series will cover user feeds including subscriptions and playlists. Including getting the videos from those playlists.

If you have any request for help or tutorials either leave a comment or [submit a comment](/contact/).

### Complete Code:

{% highlight javascript %}
<script type="text/javascript">
var currentPage = 1;
var options = {
    feed: 'top_rated',
    limit: 12,
    page: currentPage,
    success: function(videos, numberPages) {
        $('#myVideos').html('');
        $('#currentPage').html(currentPage);
        $('#numberPages').html(numberPages);
        
        $(videos).each(function(index) {
            videoHTML = '<li>';

            if(index == 0) {
                videoHTML += $.jTubeEmbed(this.video, {width: 200, height: 172});
                videoHTML += this.title;
            } else {
                videoHTML += '<a href="'+this.link+'" target="_blank">';
                videoHTML += '<img src="'+this.thumbnail+'" width="200" height="150"><br>';
                videoHTML += this.title;
                videoHTML += '</a> - '+this.length;
            }
            
            videoHTML += '</li>';
            
            $('#myVideos').append(videoHTML);
        });
        
        //Back
        if(currentPage != 1) {
            $('#pageBack').removeClass('disabled').unbind('click').click(function(event) {
                currentPage = currentPage - 1;
                options.page = currentPage;
                $.jTube(options);
                event.preventDefault();
            });
        } else {
            $('#pageBack').addClass('disabled').unbind('click').click(empty);
        }
        
        //Next
        if(currentPage < numberPages) {
            $('#pageNext').removeClass('disabled').unbind('click').click(function(event) {
                currentPage = currentPage + 1;
                options.page = currentPage;
                $.jTube(options);
                event.preventDefault();
            });
        } else {
            $('#pageNext').addClass('disabled').unbind('click').click(empty);
        }
    },
    error: function(error) {
        $('#myVideos').append('<li class="error">'+error+'</li>');
    }
};
var empty = function(event) {event.preventDefault();};

$("#pageBack, #pageNext").click(empty);
$.jTube(options);
</script>
<ul id="myVideos"></ul>
<a href="" id="pageBack" class="disabled">Back</a> - Page <span id="currentPage">1</span> of <span id="numberPages">1</span> - <a href="" id="pageNext" class="disabled">Next</a>
{% endhighlight %}

#### [View Demo](/demo/using-jtube-video-and-thumbnail/final.html)

### Keep up with jTube

  * [GitHub](http://github.com/monkeecreate/jTube)
  * [jQuery Plugins](http://plugins.jquery.com/project/jTube)
  * [My Twitter](http://twitter.com/defvayne23)
  * [monkeeCreate Twitter](http://twitter.com/monkeecreate)