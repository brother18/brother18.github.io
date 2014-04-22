---
layout: post
title:  "Android 自定义控件"
date:   2014-04-22 15:17:43
categories:  Android
---


#读取Android隐藏的id的方法,比如Actionbar上的homeAsUp的ImageView
{% highlight java %}
int id = getResources().getIdentifier("up", "id", "android");
ImageView mUpIndicatorView = (ImageView) findViewById(id);
{% endhighlight %}

#得到actionbar的高度
{% highlight xml %}
?android:attr/actionBarSize
{% endhighlight %}
