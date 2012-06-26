---
layout: post
title: "POC: Flexible Output Caching "
description: ""
category: Php Output Caching
tags: [php, proxy, cache, POC]
---
{% include JB/setup %}

I started my pet-project almost a year ago, developed it in my free time and it is time to write an article about it. I also released this project under Apache 2.0 license.(http://github.com/tothimre/POC)

Last year at the Symfony conference in Paris I have heard a really good quote:

“There are only two hard things in Computer Science: cache invalidation and naming things” — Phil Karlton. I agree with it and it gave me a boost to keep evolving the concept.

#Addressed Audience

This article is for software developers and people who can tweak code. It is also for people who had or will have performance issues. This software will help you to solve these issues with minimal effort.

I don’t want you to get bored, so I will lead you to the examples, if you want to know more technical details you can find it after the following topic.

#Examples

Unfortunately I cannot cover all the features framework provides in detail here but I can give you a brief introduction to their usage through a few examples. Let’s take the first functional tests in the framework as the first example. Please be aware though that this is a quickly evolving project and the examples seen/given here might not work with these parameters in the near future as the parameters are also subject to change. So please always refer to the documentation and examples shipped with the framework.

The functional tests do not cover all the implemented features but can be used to prove that the main functionality (caching) is working on the webserver. The tests can be found in the root of the project at the “/functionaltests” folder. The following example code does not contain inline comments because I am going to explain the code in detail.

Let’s have a look at cache_filecache.php


    use POC\Poc;
    use POC\cache\cacheimplementation\FileCache;
 
    include("../framework/autoload.php");
 
    $poc  = new Poc(array(Poc::PARAM_CACHE => new FileCache(), Poc::PARAM_DEBUG => true));
 
    $poc->start();
 
    include('lib/text_generator.php');

The first thing you notice looking at the code snippet above is the way parameters are given to the constructor of the Poc class. Because many parameters can be changed in the classes shipped with the project, I decided to build a more flexible way of handling parameters than the one used in PHP. The user has to pass an array to these objects where the index of the array is the name of the parameter and the value of course is the value of the parameter. If a parameter is not defined in the array the framework will use the builtin default value for that parameter. As the default cache engine is FileCache we could have omitted the PARAM_CACHE parameter in the previous example and define the $poc variable like this:

$poc  = new Poc(array(Poc::PARAM_DEBUG => true));
This is a really easy scenario where your application is mocked by the “/lib/text_generator.php” file. This is at the moment a Lorem Ipsum generator. We cache its contents – simply by creating the Poc object – as we can see in the example. We store the generated caches for 5 seconds – it is the default value – and we also want to see some performance information at the end of the cached text so we turned on debugging. We achieve this by adding the last parameter with a “true” value. The Hasher class is an important part of the concept. Let me describe it in the following example.

In this example we used the FileCache engine for caching, but by changing only a few characters we can use  “MemcachedCache”, “RediskaCache”, “MongoCache”, etc. So, it is really easy to implement new caching engines to the project.

#More complex Example

Let’s get a closer look at an everyday example. That’s why I took the Symfony Jobeet tutorial for describing the usage more closely. I have copied my framework to the lib/vendors folder and also created the poc.php file in the config folder. This file needs to be included at the very beginning of the application. For now we put it at the first line of the file:  config/ProjectConfiguration.class.php

Let me reveal to you the contents of the file poc.php.

    require(dirname(__FILE__).'/../lib/vendor/poc/framework/autoload.php');
    use POC\cache\filtering\Hasher;
    use POC\cache\filtering\Filter;
    use POC\Poc;
    use POC\cache\cacheimplementation\FileCache;
 
    $hasher = new Hasher();
    $hasher->addDistinguishVariable($_GET);
    $hasher->addDistinguishVariable($_SERVER["REQUEST_URI"]);
 
    $conditions = new Filter();
    $conditions->addBlackListCondition($_POST);
    $conditions->addBlackListCondition(strpos($_SERVER['REQUEST_URI'],'backend'));
    $conditions->addBlackListCondition(strpos($_SERVER['REQUEST_URI'],'job'));
    $conditions->addBlackListCondition(strpos($_SERVER['REQUEST_URI'],'affiliate'));
 
    $cache = new FileCache(array(FileCache::PARAM_TTL=>300,
    FileCache::PARAM_FILTER=>$conditions,
    FileCache::PARAM_HASHER=>$hasher));
 
    $poc  = new Poc(array(Poc::PARAM_CACHE=>$cache, Poc::PARAM_DEBUG=>true));
    $poc->start();

As you can see, the basics of this case are really similar to the previous example. But here we utilize some other features as well.

By calling the addDistinguishVariable function we will make our cache unique. Those variables get stored into an array that you add to it as a parameter, then at one point the array will be serialized and a hashkey will be generated from it. This hash will identify your cache. You have to find the variables that can identify the state of your application and add those to this function, it is that simple! As I examined the Jobeet application the $_GET, and the Request URI values define the state of the application on the level we want to use the cache. This means that authenticated pages are not cached in this case.

now we have reached the next piece of code that calls the addBlacklistCondition function. This stores logical values regarding the current state of your application. If any of those are true the page will not be involved in the caching process. Here we have defined if there is a POST action or if the URL contains any of the “backend”,” job”, “affiliate” words the caching is not applied.

Easy, right? There is no need for more explanation; it just works out of box. Maybe I have not covered all possible blacklist states in this example, but you know the basic software.

#Performance

The Engine is really small; it does not contain more than 2400 lines of code (excluding unittests and vendors), and it does not do any “black magic”. The total overhead should be less than one millisecond. If there is a cache hit, it is likely to get the output to your machine within a few milliseconds. If you measure the process before the moment the output is pushed to the client, you can see that the cache engine in most cases gets you the page under 1 millisecond. (Note that the actual performance might depend on the cache engine you employ and the environment you run the software on.)The performance is really promising, way much faster as I thought it will be when I started the project, but for now what I can say is that in a next article you will see proper benchmarks as well, so stay tuned!

Constraints and concepts

I wanted to use the latest programming methods so I decided to support only PHP 5.3 and above. Some of the several concepts and methodologies the project uses are the following:

namespaces
Continuous integration (Jenkins)
Dependency injection
Of course it had to be fast so I didn’t want to rely on external frameworks but used the built in PHP functionality where it was possible.

#Features

With this framework you can customize the caching settings to a great extent. Let me list some of these options:

Output caching based on user defined criteria
Cache invalidation by TTL
Blacklisting / cache invalidation by application state
Blacklisting by output content
For caching it utilizes many interfaces, such as:
Memcached
Redis
MongoDb
Its own filesystem based engine.
APC (experimental, performs and works well on a webserver, but unfortunately the CLI interface does not behave like it should and it cannot be unit tested properly so I don’t include it in the master branch)
For cache tagging it utilizes MySQL but more database engines will be added
Cache Invalidation by tags
Minimal overhead on the performance
Easy to turn on/off
Controls the headers
Planned features

As the framework is still in an early state, many new features will be implemented in the future. These include the following:

Edge side includes
Cache templating with Twig
statistics stored in database
And many more
If you have any questions and or suggestions, feel free to leave a comment!
