---
layout: post
title:  "[Java异常]fillInStackTrace"
date:   2014-07-01 21:29:43
categories:  java
---



{% highlight java linenos=table %}
package exceptions;

public class MyDemo {
	public static void f() throws Exception {
		System.out.println("in f()....");
		throw new Exception("throw from f()");
	}

	public static void g() throws Exception {
		try {
			f();
		} catch (Exception e) {
			System.out.println("inside g(), printStackTrace()");
			e.printStackTrace(System.out);
			throw e;
		}
	}

	public static void h() throws Exception {
		try {
			f();
		} catch (Exception e) {
			System.out.println("inside h(), printStackTrace()");
			e.printStackTrace(System.out);
			throw (Exception) e.fillInStackTrace();
		}
	}

	public static void main(String[] args) {
		try {
			g();
		} catch (Exception e) {
			System.out.println("main: printStackTrace");
			e.printStackTrace(System.out);
		}
		System.out.println("####################################################################");
		try {
			h();
		} catch (Exception e) {
			System.out.println("main: printStackTrace");
			e.printStackTrace(System.out);
		}
	}
}
{% endhighlight %}

{% highlight java linenos=table %}
in f()....
inside g(), printStackTrace()
java.lang.Exception: throw from f()
	at exceptions.MyDemo.f(MyDemo.java:6)
	at exceptions.MyDemo.g(MyDemo.java:11)
	at exceptions.MyDemo.main(MyDemo.java:31)
main: printStackTrace
java.lang.Exception: throw from f()
	at exceptions.MyDemo.f(MyDemo.java:6)
	at exceptions.MyDemo.g(MyDemo.java:11)
	at exceptions.MyDemo.main(MyDemo.java:31)
####################################################################
in f()....
inside h(), printStackTrace()
java.lang.Exception: throw from f()
	at exceptions.MyDemo.f(MyDemo.java:6)
	at exceptions.MyDemo.h(MyDemo.java:21)
	at exceptions.MyDemo.main(MyDemo.java:38)
main: printStackTrace
java.lang.Exception: throw from f()
	at exceptions.MyDemo.h(MyDemo.java:25)
	at exceptions.MyDemo.main(MyDemo.java:38)
{% endhighlight %}
