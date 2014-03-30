---
layout: post
title:  "github push without username and password"
date:   2014-03-30 15:17:43
categories: git
---


1. Modify .git/config
{% highlight ruby %}
[remote "origin"]
	fetch = +refs/heads/*:refs/remotes/origin/*
	url = https://github.com/brother18/brother18.github.io.git
{% endhighlight %}
to the following

{% highlight ruby %}
[remote "origin"]
	fetch = +refs/heads/*:refs/remotes/origin/*
	url = git@github.com:brother18/brother18.github.io.git
{% endhighlight %}

