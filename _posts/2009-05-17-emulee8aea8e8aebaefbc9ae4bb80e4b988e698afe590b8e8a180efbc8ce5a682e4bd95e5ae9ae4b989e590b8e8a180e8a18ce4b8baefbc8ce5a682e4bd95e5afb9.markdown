---
author: Sophia
comments: true
date: 2009-05-17 03:19:59+00:00
layout: post
slug: '639573'
title: EMULE讨论：什么是吸血，如何定义吸血行为，如何对抗吸血行为
wordpress_id: 639573
categories:
- 电脑/数码/网事
tags:
- dlp
- emule
- leecher
- 电骡
---

**前言**
吸血行为对ed2k网络的危害是很严重的，这一点得到了广泛的认同。但是，我们总是在这个概念上迷惑：什么是吸血，哪些行为属于吸血行为？如何对抗吸血行为？
SO，我们来讨论一下


### 1wikipedia对“吸血(leecher)”的定义




> In most P2P-networks, leeching can be defined as behavior consisting of downloading more data, over time, than the individual is uploading to other clients, thus draining speed from the network. The term is used in a similar way for shared FTP directories. Mainly, leeching is taking without giving.


摘自[http://en.wikipedia.org/wiki/Leecher](http://en.wikipedia.org/wiki/Leecher)


> 通常，吸血驴（吸血骡）的行为被定义成“上传全是为了下载”而特别订制的特点都可以叫做吸血行为，它是违反共享精神的。用通俗的话来说，就是“私驴，自私的驴”。


[http://zh.wikipedia.org/wiki/吸血驴](http://zh.wikipedia.org/wiki/吸血驴)


> eMule/eDonkey 采用的是基于P2P网络的共享原理，拥有排队机制。这种机制可以保证“分享给我最多的人获得最优先的下载权”，同时保持资源的长期有效性。但是吸血驴(吸血骡)破坏了这种机制，下载完毕后并不提供给其他人，这种潜在的对P2P共享精神的摒弃，可能将最终导致P2P网络效率下降，直至崩溃


（[http://zh.wikipedia.org/wiki/吸血驴](http://zh.wikipedia.org/wiki/吸血驴)）

个人认为，Taking without giving，和中文定义中的“上传全是为了下载”，这两点是一致的。
首先确立一点，吸血行为为什么是不好的？因为违反共享精神。
在P2P的世界里，任何一个客户端能够下载到数据，都是因为有其他的客户端上传了等量的数据。而作为对p2p网络的回馈，接收数据的客户端应当在其他客户端需要这些数据的时候将这些数据上传。这样才能保证该p2p网络的存续性。换句话说，共享精神是ed2k这个p2p网络生存的根源，一旦被颠覆，那么ed2k将不复存在。
也正是因此，ed2k有积分系统和回馈模式，依据某客户端的上传量来分配其获得下载的权利。


### 2什么是吸血行为


既然确定了吸血行为具有怎样的特性，那么可以归纳出一些典型的吸血行为。
wikipedia中文认为：[http://zh.wikipedia.org/wiki/吸血驴](http://zh.wikipedia.org/wiki/吸血驴)


> 吸血驴（吸血骡）通常具备以下行为：
每次启动时变换自己的UserHash(用户切细值)和安全认证。（让其他用户看起来此用户是新手）
只上传自己可以交换到对方下载的数据。（这就是著名的Credit Shaping，相当于用软件实现“下了就跑”）
没有自己的Mod String的称为Ghost Mod，是一种纯粹的欺骗行为。（因为它冒充官方版本）
正常工作时不断变换自己的UserHash(用户识别码)、以及假冒不同的IP+Port下同一个文件。
另外，过分的(滥用)社区加速和好友加速也属于“集体吸血”行为。（因为，社区之外的客户将受到歧视，无法体现公平性）


另外，个人认为，频繁向服务器发送请求（增加ed2k服务器负担，影响其他用户使用），盗用MOD String（这显然是不正常的），虚假的MOD String（如果是好人，为什么不亮明身份），盗用用户昵称和UserHash（会被emule加入到放行队列），传播坏块（用坏的换好的，骗取上传），伪造队列排名（严重影响公平）也都属于吸血行为。
留待补充。
总而言之，个人认为，影响ed2k网络公平性的行为，都属于吸血行为。


### 3吸血行为与吸血骡


被认定为吸血骡的原因不只是因为吸血行为的存在，还有其他原因也会使得某mod/客户端程序被认定为吸血骡。
包括但不限于下面几项：
1对GPL协议的违反。（GPL协议参见[http://zh.wikipedia.org/wiki/GNU通用公共许可证](http://zh.wikipedia.org/wiki/GNU通用公共许可证)）
mod和客户端程序（的ed2k部分）必须开源，否则是对GPL协议的违反。（此属于违法行为，这不是本帖讨论的重点）


> GPL条款规定演绎作品也必须是GPL的。


（出处同上）
另外，不开源的mod和客户端程序是否存在一些不正常的代码无法得到判定，因此被视为吸血骡。
2不能主动生成ed2k链接
只能被动地接收ED2K链接，不能生成一个不在ED2K网络中的文件的ED2K链接从而将其分享至ED2K网络之中，称为消极共享。这虽然并非罪大恶极，但是因为不鼓励分享，这样的mod不会得到骡友的认可。而大部分有此特点的mod和客户端程序往往具备其他吸血特点，因此该条是一个表现（不积极共享），而不是一个判定原则（暂时性看法，留待讨论查证）。
3包含恶意代码
这与第一条通常是伴生的。
留待补充


### 4如何对抗吸血


当前得到普遍认可的是DLP。本类下面的引用全部出自

[http://zh.wikipedia.org/wiki/动态反吸血骡保护](http://zh.wikipedia.org/wiki/动态反吸血骡保护)


> DLP全称Dynamic Leecher Protection（动态反吸血骡保护）。是一个开源的用于eMule的类似插件的东西，主要用来辅助eMule检测并屏蔽吸血骡（Leecher）。




> DLP全称Dynamic Leecher Protection（动态反吸血骡保护）。DLP最早是由eMule的Mod之一的Xtreme的开发者Xman开发，是一个开源的用于eMule的类似插件的东西，主要用来辅助eMule检测并屏蔽吸血骡（Leecher）。官方DLP原来由Xman开发并更新，现在主要由Stulle负责更新维护，自V34版本中国人zz_fly亦参与其中。


在官方版本的DLP之外，还有风之痕改写的DLP+可供选择。
其他如mod自带的反吸血（如Argos），本人不甚了解，留待讨论补充。


### 5官方对屏蔽吸血的态度


事实上Emule-Project对于屏蔽吸血骡的态度是严守中立，不支持也不反对。
然而事实上，正如wikipedia指出的那样：


> 吸血驴(吸血骡)会给eMule网络造成巨大危害，影响广大eMule使用者的使用体验，尤其在中国大陆地区，吸血驴(吸血骡)被广泛使用，严重破坏eMule网络的公平性原则。鉴于此，很多中国eMule爱好者通过加载DLP，达到屏蔽部分不良eMule客户端的目的。


[http://zh.wikipedia.org/wiki/动态反吸血骡保护](http://zh.wikipedia.org/wiki/动态反吸血骡保护)
_请注意：“尤其在中国大陆地区，吸血驴(吸血骡)被广泛使用，严重破坏eMule网络的公平性原则。”正在被请求来源。_
吸血骡对ed2k网络的危害是得到广大骡友承认的。而著名的mod大部分都有自己对抗吸血的方式（DLP，Argos==），从侧面说明了对抗吸血骡的重要性。
个人认为，如果不屏蔽吸血骡，那么正常的客户端就很难下载到数据了，更不用提ED2K的存续。

这是一篇长期讨论文章。

**关于版权的声明：因引用了Wikipedia的内容，本篇文章的版权遵循[GNU自由文档许可证](http://zh.wikipedia.org/wiki/Wikipedia:GNU自由文档许可证文本)**
