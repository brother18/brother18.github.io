---
layout: post
title:  "Activity launch mode"
date:   2014-06-04 15:17:43
categories: Android
---

* Standard
* SingleTop
* SingleTask
* SingleInstance   

命令
{% highlight ruby %}
adb shell dumpsys activity
{% endhighlight %}



* stardard和singletop可以被实例化多次
* singletask和singleinstance只能有有一个实例


SingleTop  
如果Activity A在Stask的顶部则A不会被启动,onNewIntent会被调用,

SingleTask   
在新栈的底部创建新的Activity
一次只有一个Activity实例
onNewIntent会被调用如果activity已经存在

singletask模式的activity不管时位于栈顶还是栈底,再次运行找个activity时,都会destroy掉它上面的activity来保证整个栈中只有一个自己

singleinstance
跟singletop类似,但是
