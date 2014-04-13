---
layout: post
title:  "Android命令行截屏"
date:   2014-04-13 15:17:43
categories: Android_Tools
---

{% highlight ruby %}
adb shell screencap -p | sed 's/\r$//' > ${PWD}/screenshot_$(date +%Y_%m_%d_%H_%M_%S).png
{% endhighlight %}


