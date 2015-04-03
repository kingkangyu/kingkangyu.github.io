---
layout: post
title:  "CI框架源代码学习01"
date:   2015-04-03 13:06:00
categories: php
tags: php CI
---

###index.php###

`defined` — 检查某个名称的常量是否存在

- 说明
- 
        bool defined ( string $name )
    
	检查该名称的常量是否已定义。

- 参数

    `name`

    常量的名称。

- Note:

	如果你要检查一个变量是否存在，请使用 `isset()`。 `defined()` 函数仅对 `constants` 有效。如果你要检测一个函数是否存在，使用 `function_exists()`。

`exit` — 输出一个消息并且退出当前脚本

- 说明

		void exit ([ string $status ] )
		void exit ( int $status )

	中止脚本的执行。 尽管调用了 `exit()`， `Shutdown`函数 以及 `object destructors` 总是会被执行。

- 参数

    `status`

    如果 status 是一个字符串，在退出之前该函数会打印 status 。

    如果 status 是一个 integer，该值会作为退出状态码，并且不会被打印输出。 退出状态码应该在范围0至254，不应使用被PHP保留的退出状态码255。 状态码0用于成功中止程序。

`dirname` — 返回路径中的目录部分

- 说明

        string dirname ( string $path )

    给出一个包含有指向一个文件的全路径的字符串，本函数返回去掉文件名后的目录名。

- 参数

    `path`

    一个路径。

    在 Windows 中，斜线（/）和反斜线（\）都可以用作目录分隔符。在其它环境下是斜线（/）。

- 返回值

    返回 path 的父目录。 如果在 path 中没有斜线，则返回一个点（'.'），表示当前目录。否则返回的是把 path 中结尾的 /component（最后一个斜线以及后面部分）去掉之后的字符串。

chdir — 改变目录

- 说明

        bool chdir ( string $directory )

    将 PHP 的当前目录改为 directory。

- 参数

    `directory`

    新的当前目录

`realpath` — 返回规范化的绝对路径名

- 说明

        string realpath ( string $path )

    `realpath()` 扩展所有的符号连接并且处理输入的 path 中的 '/./', '/../' 以及多余的 '/' 并返回规范化后的绝对路径名。返回的路径中没有符号连接，'/./' 或 '/../' 成分。

- 参数

    `path`

    要检查的路径。

`pathinfo` — 返回文件路径的信息

- 说明

        mixed pathinfo ( string $path [, int $options = PATHINFO_DIRNAME | PATHINFO_BASENAME | PATHINFO_EXTENSION | PATHINFO_FILENAME ] )

    `pathinfo()` 返回一个关联数组包含有 path 的信息。返回关联数组还是字符串取决于 options。

- 参数

    `path`

    要解析的路径。

   `options` 

    如果指定了，将会返回指定元素；它们包括：`PATHINFO_DIRNAME`，`PATHINFO_BASENAME` 和 `PATHINFO_EXTENSION` 或 `PATHINFO_FILENAME`。

    如果没有指定 `options` 默认是返回全部的单元。

- 返回值

    如果没有传入 `options` ，将会返回包括以下单元的数组 array：dirname，basename 和 extension（如果有），以及filename。

`str_replace` — 子字符串替换

- 说明

        mixed str_replace ( mixed $search , mixed $replace , mixed $subject [, int &$count ] )

    该函数返回一个字符串或者数组。该字符串或数组是将 subject 中全部的 search 都被 replace 替换之后的结果。

    如果没有一些特殊的替换需求（比如正则表达式），你应该使用该函数替换 ereg_replace() 和 preg_replace()。

- 参数

    如果 `search` 和 `replace` 为数组，那么 `str_replace()` 将对 `subject` 做二者的映射替换。如果 `replace` 的值的个数少于 `search` 的个数，多余的替换将使用空字符串来进行。如果 `search` 是一个数组而 `replace`是一个字符串，那么`search` 中每个元素的替换将始终使用这个字符串。该转换不会改变大小写。

    如果 `search` 和 `replace` 都是数组，它们的值将会被依次处理。

    `search`

    查找的目标值，也就是 needle。一个数组可以指定多个目标。

    `replace`
    
    search 的替换值。一个数组可以被用来指定多重替换。

    `subject`
    
    执行替换的数组或者字符串。也就是 haystack。

    如果 subject 是一个数组，替换操作将遍历整个 subject，返回值也将是一个数组。

    `count`
    
    如果被指定，它的值将被设置为替换发生的次数。

- 返回值

    该函数返回替换后的数组或者字符串。

[`__FILE__`](http://php.net/manual/zh/language.constants.predefined.php)

文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。自 PHP 4.0.2 起，`__FILE__ `总是包含一个绝对路径（如果是符号连接，则是解析后的绝对路径），而在此之前的版本有时会包含一个相对路径。

[`strrchr`](http://php.net/manual/zh/function.strrchr.php) — 查找指定字符在字符串中的最后一次出现

- 说明

        string strrchr ( string $haystack , mixed $needle )
    
    该函数返回 `haystack` 字符串中的一部分，这部分以 needle 的最后出现位置开始，直到 `haystack` 末尾。

[`is_dir`](http://php.net/manual/zh/function.is-dir.php) — 判断给定文件名是否是一个目录

- 说明

        bool is_dir ( string $filename )
    
    判断给定文件名是否是一个目录。

[`trim`](http://php.net/manual/zh/function.trim.php) — 去除字符串首尾处的空白字符（或者其他字符）

- 说明

        string trim ( string $str [, string $charlist = " \t\n\r\0\x0B" ] )
    
    此函数返回字符串 str 去除首尾空白字符后的结果。如果不指定第二个参数，trim() 将去除这些字符：

    " " (ASCII 32 (0x20))，普通空格符。

    "\t" (ASCII 9 (0x09))，制表符。

    "\n" (ASCII 10 (0x0A))，换行符。

    "\r" (ASCII 13 (0x0D))，回车符。

    "\0" (ASCII 0 (0x00))，空字节符。

    "\x0B" (ASCII 11 (0x0B))，垂直制表符。

[`require_once`](http://php.net/manual/zh/function.require-once.php)

- 说明
    
    `require_once` 语句和 `require` 语句完全相同，唯一区别是 PHP 会检查该文件是否已经被包含过，如果是则不会再次包含。

    `require` 和 `include` 几乎完全一样，除了处理失败的方式不同之外。`require` 在出错时产生 `E_COMPILE_ERROR` 级别的错误。换句话说将**导致脚本中止**而 `include` 只产生警告`（E_WARNING）`，脚本会继续运行。

### core/Common.php ###

[`phpversion`](http://php.net/manual/zh/function.phpversion.php) — 获取当前的PHP版本

- 说明

        string phpversion ([ string $extension ] )
    
    返回了包含当前运行 PHP 解释器或扩展版本信息的 string。

[预定义常量](http://php.net/manual/zh/reserved.constants.php)

- PHP_VERSION 

    PHP版本

- DIRECTORY_SEPARATOR 

    系统分隔符
    
[`version_compare`](http://php.net/manual/zh/function.version-compare.php) — 对比两个「PHP 规范化」的版本数字字符串

- 说明

        mixed version_compare ( string $version1 , string $version2 [, string $operator ] )

    `version_compare()` 用于对比两个「PHP 规范化」的版本数字字符串。 这对于编写仅能兼容某些版本 PHP 的程序很有帮助。


