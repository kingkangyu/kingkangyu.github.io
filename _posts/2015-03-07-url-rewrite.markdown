---
layout: post
title:  "URL重写总结"
date:   2015-03-07 16:55:00
categories: 运维
tags: apache url rewirte
---
URL伪静态：

浏览器输入静态URL，通过重写模块转换为服务器可识别动态URL。浏览器显示的URL是不会改变的也就是静态的URL。

过去我一直认为URL伪静态或者重写是浏览器中输入动态URL的

    index.php?id=1
    
被重写为

    index.php/1
    
来实现友好的SEO，浏览器的URL显示也会变成

    index.php/1
    
这从最初的理解就完全偏差了。

事实上伪静态应该是输入静态的网址然后通过重写模块转化为可识别的动态URL，无论你输入什么，浏览器中的URL是不会改变的（除非进行重定向）。

所以应用场景应该是这样的：

浏览器输入URL为

    index.php/1
    
然后重写模块转换为

    index.php?id=1
    
然后交由服务器脚本处理，而此时浏览器中的URL就是

    index.php/1
  
也就是所谓的静态网址。

再比如[CodeIgniter](http://codeigniter.org.cn/user_guide/general/urls.html)帮助文档中URL去掉`index.php`的重写规则

    RewriteRule ^(.*)$ /index.php/$1 [L]
    
当在浏览器输入

    eample.com/para
    
会被apache重写为

    eample.com/index.php/para
    
来实现去掉URL中的`index.php`。
