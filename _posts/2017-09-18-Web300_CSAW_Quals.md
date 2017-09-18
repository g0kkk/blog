---
layout: post
title:  "Java Serialization - CSAW Web 300"
image: ''
date:   2017-09-18 00:06:31
tags:
- Java Serialization, CSAW-Qualifiers, CSAW-2017
description: 'Web 300 - Java Serialization'
categories:
- Web, Java Serialization
---

So after a break from last year writing blogs, I thought of writing blog for this particular challenge for the reason that the time it took to get solved and also the method involved to solve this particular challenge.

The challenge description was `Not My Cup of Coffee, I heard you liked food based problems, so here's a liquid one.`
<a href="http://web.chal.csaw.io:8616"> Challenge Link</a>

Clicking on the link directs us to home where they have some beans already present in a table in which scrolling down shows `Flag` which we are interested in. Below that there was a link to the `admin` panel which led to a password protected page. When I tried entering random values, it showed incorrect. For a moment, I thought it would be injection but did not seem to work out.

Later, going back to the main page had an option of `Start breeding` wherein there is a coloumn where there were two fields to enter our input and another two boxes containing parent1 and parent2. Randomly tried giving in some input and gave the default parent which was `Covfefe`.

Redirecting to the home page and scrolling down showed us that our value got input with a description as well of `Covfefe`. Since there was `Flag` below that, my essential aim was to see which parent's description was inherited and bingo, it was parent1. Also we were able to see that the parent list in the `Start Breeding` page had new options being added with the Name that we gave before.

Later on going through the source code revealed that there was something next to the option, which was more of like a alphanumeric string seperated by a hash. Was able to get that the first was a base64 string and the second, some hex string. Decoding the base64 string of one of the options, here I took `Covfefe`, revealed a string.

{% highlight python %}
In [2]: base64.b64decode("rO0ABXNyABJjb2ZmZWUuQ292ZmVmZUJlYW4AAAAAAAAAAQIAAHhyAAtjb2ZmZWUuQmVhbgAAAAAAAAABAgAETAAHaW5oZXJpdHQADUxjb
   ...: 2ZmZWUvQmVhbjtMAARuYW1ldAASTGphdmEvbGFuZy9TdHJpbmc7TAAHcGFyZW50MXEAfgACTAAHcGFyZW50MnEAfgACeHBwdAAHQ292ZmVmZXBw")
Out[2]: '\xac\xed\x00\x05sr\x00\x12coffee.CovfefeBean\x00\x00\x00\x00\x00\x00\x00\x01\x02\x00\x00xr\x00\x0bcoffee.Bean\x00\x00\x00\x00\x00\x00\x00\x01\x02\x00\x04L\x00\x07inheritt\x00\rLcoffee/Bean;L\x00\x04namet\x00\x12Ljava/lang/String;L\x00\x07parent1q\x00~\x00\x02L\x00\x07parent2q\x00~\x00\x02xppt\x00\x07Covfefepp'
{% endhighlight %}
