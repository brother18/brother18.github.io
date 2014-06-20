---
layout: post
title:  "Android NDK记录"
date:   2014-06-13 15:17:43
categories: Android_Tools
---

NDK编译判断找不到pthread error: cannot find -lpthread
{% highlight ruby %}
去掉  
LOCAL_LDLIBS += -lpthread
加上  
LOCAL_CFLAGS += -DHAVE_PTHREADS
{% endhighlight %}


