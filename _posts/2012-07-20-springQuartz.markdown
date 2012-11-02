---
title: Spring中使用Quartz集群实现crontab job
author: 钱满
layout: post
category: code
---

我想了想，还是现在就记下来比较好，不然明天一早上起来又忘记掉了。用到quartz主要是因为有些乱七八糟的需求……随便了……想睡觉诶。所以捡主要的说好了。

>1.实现动态的quartz job/trigger。

这个相对来说不是很复杂，quartz实现crontab job就是用scheduler,trigger,job组成的，这里只要inject scheduler然后调用scheduleJob就可以。出了一个错误，把动态的jobdetail绑到非动态的trigger上了，不过手动设置了一下后还是有warning,但是没有在意了……

>2.jdbc jobstore：quartz的jobstore有两种方式，一种是存在内存里，一种就是JDBC JOBSTORE。

根据官方的文档来说数据源的设置之类的需要改写quartz.properties文件。也可以使用spring提供的schedulerFactoryBean来配置，这样的好处就是可以和项目其他部分无差别的使用同样的数据源之类的。quartz.properties的内容同样可以通过配置quartzProperties属性来覆盖。还是很方便的。

然后有个 很坑爹的地方我觉得。就是数据库里的表要自己建的，12张啊,我最讨厌数据库了啊！虽然quartz在分发包里提供了sql，但是如果是通过maven之类的去获取依赖，是只能拿到jar包的，因为用错了sql走了不少弯路最后还是到官网下了分发包才正确。

最后遇到的问题是序列化。这个也很坑的我觉得。因为使用了spring提供的methodInvokingJobDetailFactoryBean(可以指定job使用的方法很方便），但是这个东西它不能序列化……幸好有很高级的人重写了一份可以序列化的补丁，直接用了，十分高级。最后就是jobdetail里用到的类要实现serializable接口。