---
author: admin
comments: true
date: 2010-05-12 17:43:45+00:00
layout: post
slug: using-jtube-paging
title: Using jTube - Paging
category: Code
tags:
- Javascript
- jQuery
- jTube
- Plugin
- Tutorial
- YouTube
---

In my [previous post](/2010/05/07/using-jtube-basics/) I covered the basics of showing a users uploaded videos. In this tutorial we will go over how to implement paging. _To really show off paging, I have changed the request to top rated videos._

First we need to break our video options into a variable so we can re-use them when calling the other pages. This allows us to easily change the page number and leave all the other options intact.

{% highlight javascript %}
var options = {
    feed: 'top_rated',
    success: function(videos, numberPages) {
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
};

$.jTube(options);
{% endhighlight %}

<!-- /excerpt -->

Now that we have the options broke out, lets create some other variables that we will be using. We will need to keep track of the current page so create a variable currentPage with the default value of 1. Also a function that ignores the click of the paging link by preventing the default link behavior.

{% highlight javascript %}
var currentPage = 1;
var options = {
    // ...
};
var empty = function(event) {event.preventDefault();};
{% endhighlight %}

Now lets modify the HTML to add our paging links below our videos.

{% highlight javascript %}
<ul id="myVideos"></ul>
<a href="" id="pageBack" class="disabled">Back</a> - <a href="" id="pageNext" class="disabled">Next</a>
{% endhighlight %}

Pretty simple so far. Now lets start adding click events for the paging links. First we need to make sure that when they click the links at first nothing happens so we attach the empty function for onClick before we make the first call to jTube.

{% highlight javascript %}
$("#pageBack, #pageNext").click(empty);
$.jTube(options);
{% endhighlight %}

Next we need to add the option for paging to tell the plugin what page we are requesting. Lets also up the amount of videos from the default 5 to 12.

{% highlight javascript %}
feed: 'top_rated',
limit: 12,
page: currentPage,
success: function(videos, numberPages) {
{% endhighlight %}

Now we have a page that shows 12 videos with two dead links for paging. Lets add a click event to `next` if we are not on the last page, else we attach the empty function. When we attach the event for next page we also remove the class for disabled so we can show the user the link works. If we reach the last page we re-add the disabled class and remove any clicking events.

{% highlight javascript %}
        $('#myVideos').append(videoHTML);
    });
    
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
{% endhighlight %}

Since we use append to add the videos, clicking next adds the next page to the page we are already on. So lets clear the list before we add the new videos.

{% highlight javascript %}
success: function(videos, numberPages) {
    $('#myVideos').html('');
    
    $(videos).each(function() {
{% endhighlight %}

Now the user can go traverse forward through the pages, but what if they want to go back? Simple, do the same as next but subtract from the currentPage.

{% highlight javascript %}
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
{% endhighlight %}

[Here](/demo/use-jtube-paging/alpha.html) is what we have so far. Looks great, but its missing something. How about we tell the user what page they are on, and how many pages they can go through. First we need to add the HTML where that info will go.

{% highlight javascript %}
<ul id="myVideos"></ul>
<a href="" id="pageBack" class="disabled">Back</a> - Page <span id="currentPage">1</span> of <span id="numberPages">1</span> - <a href="" id="pageNext" class="disabled">Next</a>
{% endhighlight %}

All that's left is updating the span elements each time we change the page.

{% highlight javascript %}
success: function(videos, numberPages) {
    $('#myVideos').html('');
    $('#currentPage').html(currentPage);
    $('#numberPages').html(numberPages);
    
    $(videos).each(function() {
{% endhighlight %}

We now have paging so the user can see all our videos. In the next post I will discuss embedding the video and showing a thumbnail.

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
        
        $(videos).each(function() {
            videoHTML = '<li>';
            videoHTML += '<a href="'+this.link+'" target="_blank">';
            videoHTML += this.title;
            videoHTML += '</a> - '+this.length;
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

#### [View Demo](/demo/use-jtube-paging/final.html)

### Keep up with jTube

  * [GitHub](http://github.com/monkeecreate/jTube)
  * [jQuery Plugins](http://plugins.jquery.com/project/jTube)
  * [My Twitter](http://twitter.com/defvayne23)
  * [monkeeCreate Twitter](http://twitter.com/monkeecreate)