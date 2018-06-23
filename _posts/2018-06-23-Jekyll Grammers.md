---
layout:     post
title:      Jekyll Grammer
author:     Kim SS
tags: 	    log
subtitle:   How to use things about Jekyll
category:   log
---



# Linking to posts

`2018-06-23` Add | 

https://jekyllrb.com/docs/templates/#linking-to-posts

```
{{ site.baseurl }}{% post_url 2010-07-21-name-of-post %}
{{ site.baseurl }}{% post_url /subdir/2010-07-21-name-of-post %}
[Name of Link]({{ site.baseurl }}{% post_url 2010-07-21-name-of-post %})
```