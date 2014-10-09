---
layout: post
title:  "Short guide to fast JavaScript"
date:   2014-03-04 17:00
categories: volafile
---

So you have optimized your logic but your program is still not running fast enough?
Don't worry, there are still a few things you can do:

#Use the Identity Operator instead of the Equality Operator
{% highlight javascript %}
if(a === b || a !== b)  //Do this
if(a == b || a != b)  //Don't do this
{% endhighlight %}
Using the identity operator can be up to three times faster in v8.
The reason is that when using the identity operator, the JavaScript VM will 
not do type conversions. This can be useful if you know what kind of type
you're expecting and will even give you a huge speedup if you're only
comparing a few strings. 

Example:<br>
*false == null* is true<br>
*false === null* is false.

#Don't use Array.prototype.join()
{% highlight javascript %}
//Do this:
var result = '';
for(var i=0, length=array.length; i < length; i++){
   result += array[i];
}
//Don't do this:
var result = array.join('');
{% endhighlight %}

Using .join() to concatenate strings is still [slower](http://jsperf.com/string-concat-vs-array-join-10000/15) than just using the
\+ Operator in modern VMs.

#Don't use for-in
{% highlight javascript %}
//Do this:
for(var i=0, length=array.length; i < length; i++){}
//Don't do this:
for(var i in array){}
{% endhighlight %}

Always prefer a regular for-loop iterating from *0* to *length* to using
a for-in loop.

#Prefer a.x to a\[\'x\'\]
The latter will make the VM perform a slow hash-table lookup instead of using
an optimized path.

#Avoid 'delete' where possible
Use a.x = undefined instead.
Using delete a.x forces the VM to create a new hidden class.

#Don't use eval & with
Both statements prevent the VM and JIT from applying optimizations.

#The End
Above are a few simple tricks that are all easy to implement. Some allowed me
to increase the speed of code used on [Volafile's](http://volafile.io) Node.js
servers by a factor of 2x and more. I hope it will be just as helpful to you as
learning about these was to me.
If you're eager to learn more,
[here](http://www.slideshare.net/olivvv/prez-4856640) are some slides by Florian Loitsch, who gave
a presentation on the very topic of JavaScript Performance.
