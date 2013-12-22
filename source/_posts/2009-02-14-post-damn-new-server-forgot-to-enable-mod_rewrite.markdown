---
layout: post
title: "damn, new server: forgot to enable mod_rewrite"
date: 2009-02-14 17:24
comments: true
---
dont know if someone besides thilo noticed, but since i reinstalled my server (2 weeks ago) all the links in this blog (running wordpress) where broken.wordpress uses an apache module called [mod_rewrite](http://httpd.apache.org/docs/1.3/mod/mod_rewrite.html) to rewrite the url. so what u see in the adressbar in your browser is not like<pre>``www.batterslave.com/?p=9``</pre>but<pre>``www.batteryslave.com/2009/02/lixhot-boysmeisenfrei-log/``</pre>a bit more readable. but i forgot to enable the module. that is an easy step if you running a standard debian 4.0 linux on your server like i do. apache 2 comes with some little helper scripts for managing modules and sites. in my case the script a2enmod will do the job (# will symbolise my prompt):<pre>``# a2enmod rewrite``</pre>after that you call<pre>``# /etc/init.d/apache2 reload``</pre>so apache relaods the module and config. and we are ready to run. 