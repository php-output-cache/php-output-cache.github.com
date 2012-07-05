---
layout: page
title: "FAQ"
description: "Frequently Asked Questions"
group: navigation
order: 3
---
{% include JB/setup %}
##It seemes it is a reverse proxy 
Poc is not a revese proxy altought it does something similar to it. This tool is embedded within your application it can see more things that a reverse proxy. Poc can work happily together with other reverse proxyes, helping them out if it is really needed.

##Well is it better than the other proxies written in C?
Caching HTTP Reverse proxyes like Varnish are really nice tools for proxying pages, if your application is written properly They can use the request headers of the client and the server in order to be able to cache or not the content is being produced on the server side. This amoutn of infomation in most cases is not enough to make proxying really efficient. Or you have to develop custom (proxy) server and (http server side) client applications to be able to handle this situations. It takes a lot of time and effper, also you have modify your application.

As POC can see the internal state of your application it is easier for this to be abble to cache or nat cache the contents of a page if you can define for instance blacklisting for certains states within your application. For instance if the admin is logged in don't apply any caching for her session.

##POC uses huge libraries like Docrine2, how can it be effective and fast?
As Poc builts on stable and tested tools we can keep the code maintainable, testable  and flexible. Altough these tools only used when it is neede Doctine is used for the cache Tagging feature olyon cache writes and invalidation on a really easy table structure. As Doctrne2 also uses the PSR0 standard only the required classes are loaded into the memory this part is fast enough, but be free to wrtie some native sql implemetation for this feature if you need it.

