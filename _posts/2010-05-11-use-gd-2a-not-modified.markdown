---
author: John Hoover
comments: true
date: 2010-05-11 20:31:09+00:00
layout: post
slug: use-gd-2a-not-modified
title: 'Use GD - 2a: Not Modified'
category: Code
tags:
- GD
- PHP
- Tutorial
---

When creating an image with PHP per call, why should the server have to create the image every time. In part two of using GD I'll show how to use the 304 Not Modified header, and storing the image as cache.

To start we need to pass more info about the image from the [previous post](/2010/02/25/use-gd-part-1/). Need to pass the last modified date and time, tell the browser that it can cache it and for how long, when the image would expire and an ID for the file.

{% highlight php %}
header("Last-Modified: ".gmdate("D, d M Y H:i:s", filemtime($sFile))." GMT");
header("Pragma: public");
header("Cache-Control: maxage=".(60*60*24*2));
header("Expires: ".gmdate("D, d M Y H:i:s", strtotime("+2 days"))." GMT");
header("ETag: ".md5($sFile));
{% endhighlight %}

<!-- /excerpt -->

Now that the browser knows this info, it will pass it back in the next call. We use this info to check to make sure the browser has the right file, and if so, tell the browser that the file has not changed and use what it already has. We pass all the headers from before again so the browser has them for the next time it calls for the image. Once all the headers are passed we must exit so we don't do process that isn't needed.

{% highlight php %}
if ($_SERVER["HTTP_IF_MODIFIED_SINCE"] == gmdate("D, d M Y H:i:s", filemtime($sFile))." GMT"
  && $_SERVER["HTTP_IF_NONE_MATCH"] == md5($sFile)) {
	header('HTTP/1.1 304 Not Modified');
	header("Last-Modified: ".gmdate("D, d M Y H:i:s", filemtime($sFile))." GMT");
	header("Pragma: public");
	header("Cache-Control: maxage=".(60*60*24*2));
	header("Expires: ".gmdate("D, d M Y H:i:s", strtotime("+2 days"))." GMT");
	header("ETag: ".md5($sFile));
	exit;
}
{% endhighlight %}

Now lets put it all together.

{% highlight php %}
// Set image info
$sFile = "image.jpg";
$aInfo = getimagesize($sFile);
list($sWidth, $sHeight, $sType) = $aInfo;

// Check if file is cached in the browser
if ($_SERVER["HTTP_IF_MODIFIED_SINCE"] == gmdate("D, d M Y H:i:s", filemtime($sFile))." GMT"
  && $_SERVER["HTTP_IF_NONE_MATCH"] == md5($sFile)) {
	header('HTTP/1.1 304 Not Modified');
	header("Last-Modified: ".gmdate("D, d M Y H:i:s", filemtime($sFile))." GMT");
	header("Pragma: public");
	header("Cache-Control: maxage=".(60*60*24*2));
	header("Expires: ".gmdate("D, d M Y H:i:s", strtotime("+2 days"))." GMT");
	header("ETag: ".md5($sFile));
	exit;
}

// Load image
switch($sType)
{
	case IMAGETYPE_GIF: $oImage = imagecreatefromgif($sFile); break;
	case IMAGETYPE_JPEG: $oImage = imagecreatefromjpeg($sFile); break;
	case IMAGETYPE_PNG: $oImage = imagecreatefrompng($sFile); break;
	default: die("Image type not found");
}

$sCropOffsetX = 148;
$sCropOffsetY = 119;
$sCropWidth = 350;
$sCropHeight = 275;

$oCrop = imagecreatetruecolor($sCropWidth, $sCropHeight);
imagefill($oCrop, 0, 0, imagecolorallocate($oCrop, 255, 255, 255));

// Crop image onto canvas
imagecopyresized($oCrop, $oImage, 0, 0, $sCropOffsetX, $sCropOffsetY, $sWidth, $sHeight, $sWidth, $sHeight);
imagedestroy($oImage);

$sMime = image_type_to_mime_type($aInfo);
header("Content-type: ".$sMime);
header("Last-Modified: ".gmdate("D, d M Y H:i:s", filemtime($sFile))." GMT");
header("Pragma: public");
header("Cache-Control: maxage=".(60*60*24*2));
header("Expires: ".gmdate("D, d M Y H:i:s", strtotime("+2 days"))." GMT");
header("ETag: ".md5($sFile));

switch($sType)
{
	case IMAGETYPE_JPEG: imagejpeg($oCrop); break;
	case IMAGETYPE_PNG: imagepng($oCrop); break;
	case IMAGETYPE_GIF: imagegif($oCrop); break;
	default: die("Image type not found");
}
imagedestroy($oCrop);
{% endhighlight %}
