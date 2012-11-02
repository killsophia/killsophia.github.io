---
author: 钱满
title: 关于angularjs
category: code
layout: post
---

这个我前面几天就想记一下了。用angularjs实现了一下某个内部聊天的前端demo。实际上我并没有使用过前端框架，最多也就是lib级别的东西。感觉还是蛮好用的，记一下有印象的东西。

a. directive，相当令人印象深刻的特性。利用directive可以很好地开发可重用的组件。根据开发者文档的说明，浏览器将html解析为dom，编译器接受dom，遍历之后得到所有的directive得到link函数集合。directive的compile方法被执行，返回值为link函数。compile函数主要用于dom结构的变形，而link函数实现了model和view的绑定，在link函数中可以注册监听dom元素并对dom做出更改。这样也达到了数据绑定的效果，很方便。

b. 依赖注入。对DI并不陌生，这让代码变得相当结构化，对于一个功能集合，我指的的是service，并不需要重复零散的写方法，而是使用一个module来做归并。

根据开发者文档，angularjs的另一个特性是可以达到点对点的UT，这个没有试。有空可以看看。

具体的可以见官方文档 。我认为已经写的相当详细。