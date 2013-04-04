---
title: gradle+guice-struts之类的
author: 钱满
layout: post
---
对不起我是标题党。

其实主要是尝试使用了一下gradle的感受而已。guice和struts只是来打酱油的。算了我真的很不会写技术博客，不如把用到的资料罗列出来还比较有帮助。

首先是gradle，是一个基于groovy的项目构建工具。创建一个由gradle管理的项目很简单。就是建一个文件夹，当中放一个build.gradle文件就可以了。这个文件类似maven的pom文件，gradle项目配置都放在里面。

gradle没有固定的项目结构，默认遵循src/main/java,resouce,src/test/java,resource这样的结构。当然你可以通过在build.gradle里面设定sourceSet来改变目录结构。不想手动设置的话，gradle的文档中提供了一个create-dir的task来做这件事情。
{% highlight python linenos %}
task "create-dirs" << {
	sourceSets*.java.srcDirs*.each { it.mkdirs() }``
	sourceSets*.resources.srcDirs*.each { it.mkdirs() }``   
}
{% endhighlight %}

执行task只需要命令行输入 `gradle create-dirs` 就可以了。

这样就有了第一个gradle项目。

其实已经可以看出gradle就是以一个一个的task来执行对项目的管理的。理解上就相当于是maven的phase。不过gradle更加有灵活性。而且配置文件看起来也轻很多，因为不是xml而是一段groovy程序。

先贴一下测试项目的build.gradle：
{% highlight python linenos %}
defaultTasks 'deploy2tomcat'
apply plugin: 'java'
apply plugin: 'war'


task "create-dirs" << {
   sourceSets*.java.srcDirs*.each { it.mkdirs() }
   sourceSets*.resources.srcDirs*.each { it.mkdirs() }
}
repositories {
    mavenLocal()
    mavenCentral()
}

dependencies {
    compile('com.google.inject.extensions:guice-struts2:3.0')
    compile('org.apache.struts:struts2-core:2.3.12')
    compile('javax.servlet:servlet-api:2.5')
}

task deploy2tomcat(type:Copy, dependsOn: war) {
    from 'build/libs/test.war'
    into '/path/to/tomcat/webapps/'
}

task deleteTest(type:Delete,dependsOn: deploy2tomcat) {
    delete '/path/to/tomcat//webapps/test'
}

task restarttomcate(type:Exec,dependsOn: deleteTest){
    workingDir '/Users/fuluchii/Documents/code.dianping.com/personal-scripts'
    commandLine './rt.sh'
}
{% endhighlight %}

这是一个简单的web application。首先可以看到`apply plugin: 'plugin-name'`这样的配置。这些插件给我们提供了一些默认的task，比如java提供了compile,resource,jar等。而war提供了war。这些tasks之间互有依赖，构成了项目的生命周期。

有一个方法可以让我们直观看到执行某个task时，哪些task被调用了。在命令行`gradle -m build` ，其中-m参数表示跳过所有task，不执行只是打印出来（dry run）。那我们可以看到，执行build task时，经过了哪些task。输出如下：
{% highlight bash %}
fuluchiitekiMacBook-Air:service-test fuluchii$ gradle -m build
folder deleted.
:compileJava SKIPPED
:processResources SKIPPED
:classes SKIPPED
:war SKIPPED
:assemble SKIPPED
:compileTestJava SKIPPED
:processTestResources SKIPPED
:testClasses SKIPPED
:test SKIPPED
:check SKIPPED
:build SKIPPED
{% endhighlight %}


这就是java,war plugin默认的项目生命周期。当然如果需要修改这些task之间的依赖关系，也是可以轻松做到的。

下面说一下`repositories` 和 `dependencies`。作为maven使用者应该很熟悉这些。是的，gradle可以通过这样的格式来表明项目依赖，起码行数上短了很多。注意到dependencies中的compile，说明这些依赖是在compile task的时候执行的。可以想一下maven依赖的scope参数，很容易理解。

最后要说的是我们自己的task。这里有很多东西，大家可以看一下gradle的文档（但是这个文档太长了。。）。我就只写了最简单的应用，因为自己也只学了这么多。

可以看到我的build文件里面有3个自定义的task `deploy2tomcat`  `deleteTest` `restarttomcat`。分别做了3件事情：把war包放到tomcat的webapp里，删除之前的文件夹，重启tomcat。这些其实就是我完成项目修改之后的deploy工作。

可以看到一个task的一个很重要的参数dependsOn，这就是task相互依赖的实现。比如说重启tomcat之前需要删除旧的项目文件夹，所以restarttomcat依赖了deleteTest。之前也提到java plugin里面的task依赖也是可以改的，因为task提供了一个方法dependsOn来修改自己的依赖。比如：

`build.dependsOn deploy2tomcat`

这样就让build转而依赖deploy2tomcat

构建task的另一个参数：type。这个参数是用来指定task的类型。比如说restarttomcat这个task的type是Exec，因为这个task实际上是执行了我自己的一个脚本`rt.sh`，来做一些关闭进程，清除日志，重启服务器之类的工作。

现在执行一下 `gradle -m deploy2tomcat`看看：
{% highlight perl %}
fuluchiitekiMacBook-Air:service-test fuluchii$ gradle -m restarttomcat
:compileJava SKIPPED
:processResources SKIPPED
:classes SKIPPED
:war SKIPPED
:deploy2tomcat SKIPPED
:deleteTest SKIPPED
:restarttomcate SKIPPED

BUILD SUCCESSFUL{% endhighlight %}

可以看到当我执行deploy2tomcat时，gradle重新编译了项目，拷贝资源，生成war包，把新的war包拷贝进服务器目录，最后重启服务器。这些事情其实就是我们自定义的项目生命周期。回想一下在maven中，我们可能需要不少插件来完成这些事情。

另外可以看到build.gradle第一行调用了`defaultTasks 'deploy2tomcat'`。这表示我在执行gradle脚本时不加参数时，默认执行deploy2tomcat。这里其实有点原因，intelliJ的gradle插件貌似不支持执行带参数的gradle命令。所以这样设置了，然后“我们不用很麻烦也可以在IDE里点一下完成整个项目的重新部署=W=”

啊好辛苦终于写完了，虽然配置只有短短的几行而已。。。其实gradle可以做的很多，动态task啊什么的很高端的样子。而且gradle提供了对maven的完美支持，所以可用性还是很强的。

其实觉得最关键的是build.gradle文件，不要把他当作一个配置文件去读，他是一段程序。

然后来说一下标题里面挂羊头卖狗肉的guice-struts。其实就是guice提供的一个和struts整合的插件，按照官方文档写就好。不过我现在还没有找到怎么改struts.xml路径的方法，所以先丢在默认路径了。

guice还是比较有意思的，觉得可以继续看下去。

我好困QAQ。
