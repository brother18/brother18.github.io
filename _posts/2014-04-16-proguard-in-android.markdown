---
layout: post
title:  "Android代码混淆"
date:   2014-04-16 15:17:43
categories:  Android
---


#1.首先标配
{% highlight ruby %}
-optimizationpasses 5
-dontusemixedcaseclassnames
-dontskipnonpubliclibraryclasses
-dontpreverify
-verbose
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*
-keep public class * extends android.app.Activity
-keep public class * extends android.app.Application
-keep public class * extends android.app.Service
-keep public class * extends android.content.BroadcastReceiver
-keep public class * extends android.content.ContentProvider
-keep public class * extends android.app.backup.BackupAgentHelper
-keep public class * extends android.preference.Preference
-keep public class com.android.vending.licensing.ILicensingService
-keepclasseswithmembernames class * {
    native <methods>;
}
-keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet);
}
-keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet, int);
}
-keepclassmembers class * extends android.app.Activity {
   public void *(android.view.View);
}
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}
-keep class * implements android.os.Parcelable {
  public static final android.os.Parcelable$Creator *;
}
-keepattributes *Annotation*
{% endhighlight %}


#2.具体项目
比如需要忽略fragment
{% highlight ruby %}
-keep class com.cvte.irremote.*Fragment
{% endhighlight %}

忽略某个包
{% highlight ruby %}
-keep public class com.cvte.weather.models.**
{% endhighlight %}

#3.其他常用混淆方式
umeng统计
{% highlight ruby %}
-dontwarn com.umeng.**
-keep class com.umeng.**{*;}
-keep public class com.cvte.irremote.R$*{
    public static final int *;
}
{% endhighlight %}

EventBus
{% highlight ruby %}
-keepclassmembers class ** {
    public void onEvent*(**);
}
{% endhighlight %}

gson
{% highlight ruby %}
##---------------Begin: proguard configuration for Gson  ----------
# Gson uses generic type information stored in a class file when working with fields. Proguard
# removes such information by default, so configure it to keep all of it.
-keepattributes Signature

# For using GSON @Expose annotation
-keepattributes *Annotation*

# Gson specific classes
-keep class sun.misc.Unsafe { *; }
#-keep class com.google.gson.stream.** { *; }

# Application classes that will be serialized/deserialized over Gson
-keep class com.google.gson.examples.android.model.** { *; }

##---------------End: proguard configuration for Gson  ----------
{% endhighlight %}

GreenDao
{% highlight ruby %}
-keepclassmembers class * extends de.greenrobot.dao.AbstractDao {
    public static java.lang.String TABLENAME;
}
-keep class **$Properties
{% endhighlight %}
