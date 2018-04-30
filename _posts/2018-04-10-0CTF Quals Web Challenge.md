---
layout: post
title:  "0CTF - EZdoor Web Challenge Writeup"
comments: true
image: ''
date:   2018-04-10 00:06:31
tags:
- Web Security, CTF, 0CTF 2018, 0CTF Quals, PHP OPcache, PHP, Binary-PHP
description: 'Sunshine CTF 2018 web challenges writeup'
categories:
- Web, CTF, 0CTF Quals, Web Security
---

Challenge name: `EZdoor`

Link: `http://202.120.7.217:9527`

In this challenge, we are greeted with the <a href="https://github.com/gokulkrishna01/gokulkrishna01.github.io/tree/master/scripts/0CTF/source.php">source code</a>:

Breaking down the code,

{% highlight php %}

if(!file_exists($dir . "index.php")){
  touch($dir . "index.php");
}

{% endhighlight %}

does create a new `index.php` file if not created already.

This question was kind of similar to the one we had for ASIS CTF finals couple of years back. This made me realise that it was related to OPCache and since there was a <a href="http://gosecure.net/2016/04/27/binary-webshell-through-opcache-in-php-7/">writeup</a>, probably a research, on the same.

So going line by line, we were provided with 5 different functionalities - `pwd` for the `path`, `phpinfo` which provides the info page, `reset` to clear the directory, `time` to show the timestamp (this will prove to be important later) & `upload` option to upload our file.

Understanding the problem deeper, `OPCache` is the built-in caching engine with PHP 7.0.* versions. It not only compiles PHP scripts and sets the resulting bytecode in memory but also, offers caching in the filesystem when specifying a destination folder in `PHP.ini` file.

You can find the line `opcache.file_cache=/tmp/opcache` in `PHP.ini` file. Inside this folder, `OPCache` stores its compiled PHP scripts in the same folder where the PHP code is written. So essentially, a compiled version of `/var/www/index.php` will be stored in `/tmp/opcache/<system_id>/var/www/index.php.bin`.

Now to calculate the `system_id`, we have got a tool over <a href="https://github.com/GoSecure/php7-opcache-override">here</a> and with that, we can identify what we want to calculate.

To calculate the above, the parameters were `php_version = 7.0.28` , `zend_extension_id: API320151012,NTS` and `zend_bin_id: BIN_SIZEOF_CHAR48888`.

Now about solving the challenge, firstly, to control the cache file, we mainly need to get hold of two parameters, `timestamp` and `opchache_ID`.


After generating the system ID, grab the `timestamp`. Set this to the OPcached web-shell (index.php.bin) to bypass opchache.validation_timestamp and upload it.

The final script was:

```
requests.get(URL+"?action=reset")
time = int(requests.get(URL+"?action=time").text)
f = open("index.php.bin").read()
pld = f[:8]+sid+f[40:64]+struct.pack("I", ts)+f[68:]

#and finally
requests.post(URL+"?action=upload&name=../../../../../tmp/cache/"+SID+"/var/www/html/sandbox/"+UID+"/index.php.bin", files=dict(file=pld))
```
