---
author: archisophialee
comments: true
date: 2012-02-14 07:17:06+00:00
layout: post
slug: twenty-eleven-child-theme
title: Twenty Eleven Child Theme
wordpress_id: 640099
categories:
- 电脑/数码/网事
---

Child Theme 是 Wordpress 3.0 新加入的一个功能，主要用于在不影响原始主题的基础上对主题文件进行修改。
官方文档：[http://codex.wordpress.org/Child_Themes](http://codex.wordpress.org/Child_Themes)

其核心文件是 style.css ，用于标识 Child Theme 的父模板，并通过 @import 引入父模板的 css 样式表后对其进行修改，或者完全重写 css 样式表。通过 @import 引入的 css 样式表后，用 css 语句覆盖父模板的对应 css 语句，有点类似于过去通过 mycss 之类的插件来实现的功能。
在 functions.php 写入的内容和父模板的 functions.php 中的代码一同生效。
template 文件则会添加新的 template ，或者替代父模板中同名的 template 。
其它文件也可以和父模板中的文件一样作用。

折腾了几天之后，我在 Wordpress 的官方模板 Twenty Eleven 的基础上制作了一个 Child Theme。也就是目前正在使用的主题。
改变了下列内容：


> 

> 
> 
	
>   * Change the font size and colour of the header.
> 
	
>   * Change the font family.
> 
	
>   * Change the width of the singular content and comment.
> 
	
>   * Hide some categories in both home page and single page.
> 
	
>   * Add the license to the footer.
> 
	
>   * Add a links-page template.
> 
	
>   * Hide the link to the author page to avoid password-attcking.
> 





说明：	

* 为了避免暴力破解密码的攻击，隐藏了作者的用户名，只显示昵称。


* functions.php 中的代码来源于 [http://wp-snippets.com/exclude-category-from-homepage/](http://wp-snippets.com/exclude-category-from-homepage/)


* 别忘了根据需要修改 footer 中的 lisence 内容和 links template 的内容。





PS 常见的问题基本都是由权限引起的。




压缩包：[http://db.tt/QBAsAsYA](http://db.tt/QBAsAsYA)

之后应该会研究看看如何用自己的图实现 Twenty Eleven 的 header 图片随机选择的功能。
