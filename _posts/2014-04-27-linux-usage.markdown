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

Skype Chinese

{% highlight ruby %}
sudo apt-get install ttf-wqy-microhei
sudo vim /etc/fonts/conf.d/69-language-selector-zh-cn.conf
{% endhighlight %}

{% highlight xml %}
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
   <match target="pattern">
      <test qual="any" name="family">
         <string>serif</string>
      </test>
      <edit name="family" mode="prepend" binding="strong">
         <string>WenQuanYi Micro Hei</string>
         <string>AR PL UMing CN</string>
         <string>AR PL ShanHeiSun Uni</string>
         <string>WenQuanYi Bitmap Song</string>
         <string>Bitstream Vera Serif</string>
         <string>DejaVu Serif</string>
         <string>AR PL UKai CN</string>
         <string>AR PL ZenKai Uni</string>
      </edit>
   </match> 
   <match target="pattern">
      <test qual="any" name="family">
         <string>sans-serif</string>
      </test>
      <edit name="family" mode="prepend" binding="strong">
         <string>WenQuanYi Micro Hei</string>
         <string>Bitstream Vera Sans</string>
         <string>DejaVu Sans</string>
         <string>WenQuanYi Zen Hei</string>
         <string>AR PL UMing CN</string>
         <string>AR PL ShanHeiSun Uni</string>
         <string>WenQuanYi Bitmap Song</string>
         <string>AR PL UKai CN</string>
         <string>AR PL ZenKai Uni</string>
      </edit>
   </match> 
   <match target="pattern">
      <test qual="any" name="family">
         <string>monospace</string>
      </test>
      <edit name="family" mode="prepend" binding="strong">
         <string>WenQuanYi Micro Hei Mono</string>
         <string>Bitstream Vera Sans Mono</string>
         <string>DejaVu Sans Mono</string>
         <string>WenQuanYi Zen Hei</string>
         <string>AR PL UMing CN</string>
         <string>AR PL ShanHeiSun Uni</string>
         <string>WenQuanYi Bitmap Song</string>
         <string>AR PL UKai CN</string>
         <string>AR PL ZenKai Uni</string>
      </edit>
   </match> 
</fontconfig>
{% endhighlight %}

查看自己的外部ip
{% highlight ruby %}
curl ifconfig.me
{% endhighlight %}
