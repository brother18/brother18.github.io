---
layout: post
title:  "JavaScript Hoisting"
date:   2014-08-07 15:17:43
categories: javascript
---

###含义   
Hoisting 指的是把所有声明移到当前script的顶部或者当前函数的顶部
Hoisting is JavaScript's default behavior of moving all declarations to the top of the current scope (to the top of the current script or the current function).

***

###JavaScript Declarations are Hoisted

{% highlight javascript %}
x = 5; // Assign 5 to x

elem = document.getElementById("demo"); // Find an element 
elem.innerHTML = x;                     // Display x in the element

var x; // Declare x
{% endhighlight %}

***

###JavaScript Initializations are Not Hoisted

javascript只移动声明不移动初始化
JavaScript only hoists declarations, not initializations.

{% highlight javascript %}

var x = 5; // Initialize x

elem = document.getElementById("demo"); // Find an element 
elem.innerHTML = x + " " + y;           // Display x and y

var y = 7; // Initialize y
{% endhighlight %}
将输出5 undefined

***

所以遵循   
### Declare Your Variables At the Top !
