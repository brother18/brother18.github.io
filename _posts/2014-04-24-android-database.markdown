---
layout: post
title:  "Android Database"
date:   2014-04-24 15:17:43
categories:  Android
---

#Android Sql Transaction
{% highlight java %}
db.beginTransaction();
try {
	db.execSQL(xxx);
	...
	db.setTransactionSuccessful();
} finally {
	db.endTransaction();
}
{% endhighlight %}

