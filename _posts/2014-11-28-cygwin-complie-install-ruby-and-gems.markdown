---
layout: post
title:  "Cygwin编译安装最新版Ruby和gems"
date:   2014-11-28 12:52:00
categories: 折腾
tags: cygwin ruby gems
---

因为想在公司电脑上写博客，但是Jekyll的环境需要Ruby，就去[Ruby官网](https://www.ruby-lang.org/zh_cn/downloads/)下载，公司电脑是windows，也不能装linux，没办法只能下载windows版。
windows版的Ruby比linux版本低很多（windows版1.8.6，最新版2.1.5，也真是够了!)，安装完成后gems更新[淘宝源](http://ruby.taobao.org/)的时候显示：
		
>ERROR:  While executing gem ... (Gem::RemoteSourceException)
>
>HTTP Response 302

如何更新淘宝源我文章[后边](#taobaoyuan)会说到。

一开始以为是被屏蔽了，挂上VPN还是一样。

后来查了一下好像是因为版本低的缘故，就开始琢磨其他的方法了。

然后就发现了[Cygwin](https://cygwin.com/install.html),它是用来在windows模拟linux的程序，到官网根据自己的系统（32位setup-x86 ,64位setup-x86_64）下载相应的版本。

安装的时候一路下一步，当到达`Choose A Download Site`这一步时选择:

	ftp://mirros.neusoft.edu.cn

(也不排除有163源，公司联通没看到，家里电信倒是看到了）

下一步

![安装选项](https://raw.githubusercontent.com/kingkangyu/kingkangyu.github.io/master/assets/images/20141128/20141128ruby.jpg)

这里看到两处蓝色线的地方，点一下`Devel`，会从`Default`变成`Install`，这里是包含gcc的部分，用来编译安装从官网下载的Ruby源码。
有人可能会问，下边的蓝线标注不是有`Ruby`选项，把它点一下变成`Install`不就可以了。这种是可以安装`Ruby`和`gems`的
，但是安装的不是最新版本，更新源的时候也有可能会像前边的老版本一样出错。（不过网速快的话也可以尝试一下，可以直接[跳到下边](#taobaoyuan)看如何更新淘宝源）

然后一直下一步就好了。

最后一步记得在桌面新建一个快捷方式，以后好找。

打开Cygwin，输入：

	$ cd /
	$ cd cygdrive(这里是磁盘的目录）
	$ cd d(这里是你放置下载的源码的盘符)
	$ cd 相应的源码目录

我为了方便把下载Ruby和gems源码直接放在D盘根目录了
	
解压源码包：

	$ tar -zxv -f ruby-2.15.tar.gz

`-f`后跟着下载的源码包文件名，这里是`ruby-2.15.tar.gz`

到解压的目录：

	$ cd ruby-2.1.5

一般是下载的Ruby加版本号，`2.1.5`就是我当前下载的版本号。

然后就是大同小异的安装了：

	$ ./configure
	$ make
	$ make install

`make`时间会有点长，一般安装是不会出现问题的。（要是./configure不能运行，你就要看看是不是Cygwin安装的时候gcc没有安装上，就是上一步那个Devel点成Install你没做，或者做的不对，尝试在Cygwin中输入gcc看看说的是不是命令不存在）

查看Ruby版本

	$ ruby -v

>ruby 2.1.5p273 (2014-11-13 revision 48405) [x86_64-cygwin]

你的输出不一定和我一样，但应该显示你安装的Ruby版本，如果没有更新，关闭Cygwin，重新打开再看看。

好了，这里Ruby就装完了，然后是gems。

切换到RubyGems所在目录，我的只需

	$ cd ..

解压源码包：

	$ tar -zxv -f rubygems-2.4.4

切换到解压目录：

	$ cd rubygems-2.4.4

然后gems的安装，这个是用ruby命令安装的：

	$ ruby setup.rb

然后查看gem版本：
	
	$ gem -v

>2.4.4

至此Ruby和gems已经在Cygwin环境下全部配置完成。

<h3 id="taobaoyuan">
更换淘宝gems源
</h3>
引用淘宝gems源的话

>由于国内网络原因（你懂的），导致 rubygems.org 存放在 Amazon S3 上面的资源文件间歇性连接失败。所以你会与遇到 gem install rack 或 bundle install 的时候半天没有响应，具体可以用 gem install rails -V 来查看执行过程。

淘宝gems源上的安装教程是这样的：

<pre>
<del>
$ gem sources --remove https://rubygems.org/
$ gem sources -a https://ruby.taobao.org/
</del>
</pre>

为什么打删除线呢，因为T妹的在我这里根本不好使，一输入就报：
这种

>source https://rubygems.org/ not present in cache

或这种

>source https://ruby.taobao.org/ is not a URI

错误。(后来我测试发现也正确了，坑爹。)

所以应该是这样的：

	$ gem sources -r 'https://rubygems.org/'
	$ gem sources -a 'https://ruby.taobao.org/'
	$ gem sources -l

>*** CURRENT SOURCES ***
>	
>https://ruby.taobao.org

用`'`把URL括起来就正确了，

至此就完美的结束了。

吐槽一下，发布这篇的时候，GitHub不支持markdown链接的参考式。


