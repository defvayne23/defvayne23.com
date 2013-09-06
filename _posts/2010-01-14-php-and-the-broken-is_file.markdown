---
author: admin
comments: true
date: 2010-01-14 19:15:57+00:00
layout: post
slug: php-and-the-broken-is_file
title: PHP and the broken is_file()
category: Code
tags:
- Bug
- PHP
---

Working on Hospice of Wichita Falls I used my image resize function that's built into the my CMS. Was having trouble with the gallery and the images not showing up. After some short debugging, realized the code was dying when I tried to make sure the file existed using is_file before I passed it onto GD. I spent hours looking at the path for the file and making sure everything was spelled correctly and that the file in fact did exist.

After spending most of a day on the issue I decided to look at the php.net documentation of the file. What I found in the comments was the root of my problem. Apparently the function is_file can not read files larger than ~2.1 billion bytes. My images where only on average 1MB. I wound up removing that code. Need to look into if file_exists() has the same problem.