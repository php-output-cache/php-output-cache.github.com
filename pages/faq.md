---
layout: page
title: "FAQ"
description: "Frequently Asked Questions"
group: navigation
order: 3
---
{% include JB/setup %}
##It seemes it is a reverse proxy 
POC is not a reverse proxy although it does something similar to it. Because this tool is embedded in your application, it can see more things than a reverse proxy. But POC is not a lone soldier but a real teamplayer, it can work easily together with reverse proxies, helping them if needed.

##Well, is it better than other proxies written in C?
Caching HTTP reverse proxies like Varnish are really nice tools for proxying pages if your application is written properly. They can use the request headers of the client and the server in order to be able to decide whether or not to cache the content produced on the server side. But in most cases, this amount of infomation is not enough to make proxying really efficient. Only if you develop custom (proxy) server and (http server side) client applications to handle these situations. Not only does it take a lot of time and effort but you also have to modify your application.

As POC can see the internal state of your application it can decide more easily whether or not to cache the contents of a page. You can simply define the exact conditions of caching your application with the help of black- and whitelists. For instance if the admin is logged in do not apply any caching for her session.

##POC uses huge libraries like Doctrine2, so how can it be effective and fast?
As POC is built on stable and tested tools we can keep the code maintainable, testable  and flexible. Doctrine is used for the cache tagging feature only on cache writes and invalidation and it works on a relatively small and efficient table structure. As Doctrine2 also uses the PSR-0 standard only the required classes are loaded into the memory so it doesn't slow down caching too much, but feel free to write a native SQL implemetation of this feature if you need it.

##How can it add extra protection to my application?
POC contains a CIA module that makes your application's cached part really efficient on resources. For example if you have a slow part in your application and many clients use it frequently the cache is not generated muliple times, all clients wait for the already generated page. Sounds good, right? To enable this feature it is enough to add one line to the POC part of your code.
This can help you protect your page against DDOS services, of course it won't give you all around protection in itself, but it can help to make your application more secure against such attacks.

##How can POC be good for the SEO of my site?
The faster your site the better your SEO gets! And POC is really good at boosting the speed of your site.

##It is not enough to cache the output if you want a good efficiency.
True in some situations, but if your application produces output it is wise to take into consideration if it is worth to cache the output, because it is really efficient, and fast.

##For what kind of pages would you suggest using POC?
Wherever you need performance. POC is optimal for blog engines, public websites and of course intranet corporate software can use it as well.


