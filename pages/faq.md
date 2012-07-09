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
As POC is built on stable and tested tools we can keep the code maintainable, testable  and flexible. Doctrine is used for the cache tagging feature only on cache writes and invalidation and it works on a relatively small and efficient table structure. As Doctrine2 also uses the PSR0 standard only the required classes are loaded into the memory so it doesn't slow down your application much, but feel free to write a native SQL implemetation of this feature if you need it.

##How it can add extra protetion to my application?
For instance POC contains a CIA module that makes your application cached part really efficient on resources. For example if you have a slow part in your application and many client use it frequently the cache is not generated muliple times, all client waits for the already generating page. Sounds good right? To enable this feature is enough to add one line to the Poc part of your code:).
This can help you to protect your page against DDOS services, of coures it is not enouhh alone, but can help.

##How POC can be good for the SEO of my site?
The fastest is your site the better your SEO gets! And Poc is really good at speed.

##It is not enough to cache the output if you want it good efficiently.
True in some situations, but if your application produces output it is wise to take into consideration if it was worthy to cache the output, because it is really efficient, and fast.

##For what kind of pages would you suggest to use poc?
Everywhere where you need perfoeance. For example Poc is optimal for Blog engines public websites, but of course intranet corporate software can use it as well.


