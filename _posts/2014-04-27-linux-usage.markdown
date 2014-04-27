---
layout: post
title:  "Linux使用"
date:   2014-04-27 15:17:43
categories: Linux
---

JD-gui cannot run on Ubuntu 14.04(64bit) 
error while loading shared libraries: libgtk-x11-2.0.so.0
solution
{% highlight ruby %}
sudo dpkg --add-architecture i386
udo apt-get update
sudo apt-get install libgtk2.0-0:i386
{% endhighlight %}

Add desktop entry
{% highlight ruby %}
sudo vim /usr/share/applications/jd-gui.desktop
{% endhighlight %}
{% highlight ruby %}
[Desktop Entry]
Name=JD-GUI
Comment=JD-GUI Java Decompiler
Exec=/usr/bin/jd-gui %f
Icon=/home/arthur/dev_env/tools/jd-gui.png
Terminal=false
Type=Application
Categories=GNOME;Application;Development;
StartupNotify=true
{% endhighlight %}
