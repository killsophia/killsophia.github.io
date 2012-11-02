---
title: 关于ClassLoader
layout: post
author: 钱满
category: code
---


真沉痛，时隔已久的blog居然是这个标题。

前几天刚刚拜读了这篇文章，正好今天有时间，于是严格按照原PO的说法来写代码试了一下。

我觉得这篇文章和提供的代码已经很清楚了，在我自己尝试（试着写一个动态从lib目录下的jar中加载类的classloader）时，也遇到了一点问题：

#####a. NoClassDefFoundError xxxx(wrong name:xx/xx/xx).

这个异常是在

defineClass(name, buf, 0, buf.length);

这行代码中出现的。也就是说，已经成功的找到了.class文件并且获取字节码之后，加载某个类的时候找不到对应的.class文件。错误的原因是name变量没有带相应的package，除非我在这个类对应的package下运行程序，否则都会出错。

注意后面的wrongname，这是一个提示，告诉我们有这样的一个package存在。

#####b. loadClass和findClass

注意到原PO中在自定义classLoader的时候，重写的是findClass方法，而不是loadClass.

也是很巧，我在调用我自己的classLoader的时候，因为在两个类中都有用到加载类的方法，不小心一个写了loadClass一个写了findClass。于是一直在报cast错误。

#####我猜测的原因：

由于custom classLoader在调用loadClass时，采取的策略是双亲委托模式，也就是说，他会先向上委托自己的parent，也就是AppClassLoader,System ClassLoader来加载类，比较巧的是，lib文件夹已经在classpath当中了，所以parent加载器返回了这个类给custom classloader，加载成功。如果要加载的类目录不在classpath里面，那么parent加载无果的情况下，custom会调用自己的findClass方法加载。那么在我的尝试中，findClass并没有被调用，实际上加载类的是parent加载器。

而在另一个地方调用的是findClass这个方法，直接读路径做了类的加载并返回。

这里又涉及到classloader的命名空间问题。不同的classloader在jvm中被用来分隔不同的命名空间。可以肯定的是，不同的classloader，其加载的类也是独立的。这是很容易理解的。

但是这样我又有一个疑问，作为parent的appClassLoader和customClassLoader肯定不是一个classloader，但是，custom通过双亲委托模式获得的class，和通过findClass拿到的class也不是同一个class吗？当然，直接去调用classloader的findClass方法是略奇怪的，但是如果这样做了，在这个classLoader的命名空间之内是否就存在同名的class了？

P.S.关于findClass和loadClass方法的控制循环，这个问题中阐述的十分清晰。

于是又找了一些资料，发现涉及到了命名空间之间的类型共享。

首先有一个概念，定义类加载器和初始类加载器。打个比方，child要加载a.class，调用loadClass之后加载失败，于是parent的loadClass被调用,找到了，那么child和parent都是初始类加载器，parent是定义类加载器。

实际上jvm给每个classloader都维护了一张内部列表，列表里记着哪些类已经被请求加载过了，加载了什么东西，像个缓存，下次先查表。在上面的例子里面，a.class被记了下来，同时child和parent都被标记为了初始类加载器，而对于a.class的所有初始类加载器，a.class在他们的命名空间里都是共享的。也就是说，child返回给我们的class（无论是第一次加载还是之后），实际上是定义类加载器命名空间共享过来的，而并不是自己的命名空间里的。换句话说，custom加载器可以共享到parent加载器能加载到的所有类型，但是parent不会被共享custom的独有类型。

这样的话就很清楚了，我的尝试里，loadClass返回了systemClassLoader的类实例，而findClass则返回了专属的实例，这是两样东西。所以出现了cast错误。

当然关于classloader还涉及到了GC，内存泄漏问题。这个，明天再说……