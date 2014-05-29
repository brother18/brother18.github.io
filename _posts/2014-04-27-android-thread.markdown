---
layout: post
title:  "Android Thread"
date:   2014-04-27 15:17:43
categories: Android
---

#Android的线程调度(Thread Scheduling)
线程调度器(thread scheduler)是操作系统的一个概念，决定线程是否该运行、什么时候运行和运行多久  

Android线程调度器使用两个主要的因素   

* Nice values
* Cgroups

#Nice values
Linux的线程调度策略有一种是CFS(completely fair scheduling policy)，Android使用这种策略来表示线程的优先级
nice值得范围是–20 (high priority) to +19 (low priority)

![Thread+]({{site.url}}/assets/images/thread_2.png)  

由于java的平台无关性，对应到java里  

![Thread+]({{site.url}}/assets/images/thread_1.png)

Android中的两个
但是如果只有Nice value来表示线程的调度优先级是不够的，比如如果有20个后台线程和一个前台UI线程，尽管后台线程的优先级很低，20个优先级低的后台线程也会严重影响这个前台UI线程
所有Android同样使用了Linux的cgroups(control groups)来处理前台线程和后台线程,后台组线程在其他组busy时只能使用5%的CPU资源，从源码init.rc(https://android.googlesource.com/platform/system/core/+/android-sdk-4.4.2_r1/rootdir/init.rc)可以看出
{% highlight ruby %}
# Create cgroup mount points for process groups
mkdir /dev/cpuctl
mount cgroup none /dev/cpuctl cpu
chown system system /dev/cpuctl
chown system system /dev/cpuctl/tasks
chmod 0660 /dev/cpuctl/tasks
write /dev/cpuctl/cpu.shares 1024
write /dev/cpuctl/cpu.rt_runtime_us 950000
write /dev/cpuctl/cpu.rt_period_us 1000000
mkdir /dev/cpuctl/apps
chown system system /dev/cpuctl/apps/tasks
chmod 0666 /dev/cpuctl/apps/tasks
write /dev/cpuctl/apps/cpu.shares 1024
write /dev/cpuctl/apps/cpu.rt_runtime_us 800000
write /dev/cpuctl/apps/cpu.rt_period_us 1000000
mkdir /dev/cpuctl/apps/bg_non_interactive
chown system system /dev/cpuctl/apps/bg_non_interactive/tasks
chmod 0666 /dev/cpuctl/apps/bg_non_interactive/tasks
# 5.0 %
write /dev/cpuctl/apps/bg_non_interactive/cpu.shares 52
write /dev/cpuctl/apps/bg_non_interactive/cpu.rt_runtime_us 700000
write /dev/cpuctl/apps/bg_non_interactive/cpu.rt_period_us 1000000
{% endhighlight %}   
