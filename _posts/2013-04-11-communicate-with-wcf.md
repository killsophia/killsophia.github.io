---
title: java与WCF接口交互
author: 钱满
layout: post
---

最近工作需要在做一个调用.NET的WCF的SOAP接口的东西。之前完全没有接触过.NET也不知道WCF是个什么东西，于是也去查了一下。

一开始的想法就是用工具根据接口的wsdl文件生成对应的客户端，然后使用客户端去调用。我觉得正常思路也就是这个样子的，因为对方提供的contract在wsdl文件中描述清楚，然后客户端代理去调用webservice。

使用的是jdk自带的工具wsimport，可以解析wsdl文件并且自动生成代理类文件。命令如下：

{% highlight python %}
wsimport -s dirName wsdlURL
{% endhighlight %}

会在对应的dirName中生成对应的接口文件。-s的话是java文件，-d的话就是class。这样就有了客户端代理，调用的时候直接调用客户端方法即可，相当方便。本地测试也很顺利的通过了。

但是！在上测试环境的时候出现了问题。

在发送soapmsg的时候一直报一个exception。稍微查一下泪流满面，这里就出现了上面使用wsimport的隐患。由于测试环境是JBOSS4.X，刚好在soapMsg这里和jdk1.6有兼容性问题，导致报错。而我本地用的容器是tomcat6/7，所以本地pass,测试环境挂了= =

我觉得这个就是因为我使用了jdk1.6的wsimport导致的。实际上我在看的时候，用的比较多的生成客户端的工具是axis，但是当时觉得很麻烦，所以直接使用了自带工具。于是掉了个坑。

后来由于怕axis还是有这样的问题以及时间问题，所以干脆使用了简单粗暴的方法：直接发送http请求，在请求里直接写soap请求的xml。

但是这样的话，需要知道对应的soap请求的格式。这里需要推荐一个工具叫做 SoupUI，这个工具对于测试soap接口非常好，输入wsdl地址的，会为你生成一个request sample，我这里直接抄就可以了。

不过昨天下载的时候速度实在是太慢了，好多次下到10M就卡住了。于是在这个时间里面我想不如我直接抓本地发出去的请求拉倒了。但是悲伤的是mac没有fiddler，然后charles不知道为什么失灵了（可能因为挂VPN），然后cocoaPackageAnalyzer似乎没有办法直接拿报文。。搞了好久最后soupui下载好了= =

然后的事情就很简单了。httpclient发出去，然后解析返回的xml就可以了。

完。

我要把本地换成JBOSS，被容器坑这种事情不能再发生了！