---
layout: post
title:  "PHP文件上传基础"
date:   2015-04-02 23:04:00
categories: php
tags: php
---

超全局变量`$_FILES` — HTTP 文件上传变量

说明：

- 通过 HTTP POST 方式上传到当前脚本的项目的数组。

- 4.1.0版本引入`$_FILES`，弃用 `$HTTP_POST_FILES`。

要确保文件上传表单的属性是 enctype="multipart/form-data"，否则文件上传不了。 

	<form enctype="multipart/form-data" action="__URL__" method="POST">

我们假设文件上传字段的名称为 userfile。(名称可随意命名)

	<input name="userfile" type="file" />


- `$_FILES['userfile']['name']`

	客户端机器文件的原名称。

- `$_FILES['userfile']['type']`

	文件的 MIME 类型，如果浏览器提供此信息的话。一个例子是“image/gif”。不过此 MIME 类型在 PHP 端并不检查，因此不要想当然认为有这个值。

- `$_FILES['userfile']['size']`

	已上传文件的大小，单位为字节。

- `$_FILES['userfile']['tmp_name']`

	文件被上传后在服务端储存的临时文件名。

- `$_FILES['userfile']['error']`

	和该文件上传相关的错误代码。此项目是在 PHP 4.2.0 版本中增加的。

**Example #1** 文件上传表单

可以如下建立一个特殊的表单来支持文件上传：

	<!-- 数据编码方式enctype, 必须向下方这样设置 -->
	<form enctype="multipart/form-data" action="__URL__" method="POST">
	    <!-- MAX_FILE_SIZE 必须放在文件输入字段之前 -->
	    <input type="hidden" name="MAX_FILE_SIZE" value="30000" />
	    <!-- input元素的name属性决定$_FILES数组中的$key键值 -->
	    Send this file: <input name="userfile" type="file" />
	    <input type="submit" value="Send File" />
	</form>

以上范例中的 `__URL__` 应该被换掉，指向一个真实的 PHP 文件。

`MAX_FILE_SIZE` 隐藏字段（单位为字节）必须放在文件输入字段之前，其值为接收文件的最大尺寸。这是对浏览器的一个建议，PHP 也会检查此项。在浏览器端可以简单绕过此设置，因此不要指望用此特性来阻挡大文件。实际上，PHP 设置中的上传文件最大值是不会失效的。但是最好还是在表单中加上此项目，因为它可以避免用户在花时间等待上传大文件之后才发现文件过大上传失败的麻烦。 

**Example #2** 使文件上传生效

- `is_uploaded_file` — 判断文件是否是通过 HTTP POST 上传的

		bool is_uploaded_file ( string $filename )

	如果 filename 所给出的文件是通过 HTTP POST 上传的则返回 TRUE。这可以用来确保恶意的用户无法欺骗脚本去访问本不能访问的文件.
	
	这种检查显得格外重要，如果上传的文件有可能会造成对用户或本系统的其他用户显示其内容的话。
	
	为了能使 `is_uploaded_file()` 函数正常工作，必段指定类似于 $_FILES['userfile']['tmp_name'] 的变量，而在从客户端上传的文件名 $_FILES['userfile']['name'] 不能正常运作。 
	
- `move_uploaded_file` — 将上传的文件移动到新位置

		bool move_uploaded_file ( string $filename , string $destination )

	本函数检查并确保由 filename 指定的文件是合法的上传文件（即通过 PHP 的 HTTP POST 上传机制所上传的）。如果文件合法，则将其移动为由 destination 指定的文件。
	
	这种检查显得格外重要，如果上传的文件有可能会造成对用户或本系统的其他用户显示其内容的话。 

		<?php
		// PHP版本应高于4.1.0
		
		$uploaddir = '/var/www/uploads/';
		$uploadfile = $uploaddir . basename($_FILES['userfile']['name']);
		
		echo '<pre>';
		if (move_uploaded_file($_FILES['userfile']['tmp_name'], $uploadfile)) {
		    echo "File is valid, and was successfully uploaded.\n";
		} else {
		    echo "Possible file upload attack!\n";
		}
		
		echo 'Here is some more debugging info:';
		print_r($_FILES);
		
		print "</pre>";
		
		?>

参考：

- [POST 方法上传](http://php.net/manual/zh/features.file-upload.post-method.php)

- [预定义变量$_FILES](http://php.net/manual/zh/reserved.variables.files.php)