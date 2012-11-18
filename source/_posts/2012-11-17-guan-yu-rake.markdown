---
layout: post
title: "rake介绍"
date: 2012-11-17 00:33
comments: true
categories: ruby
---

rake是一个用ruby开发的代码构建工具,以任务的方式创建和运行脚本.非常非常的有用,在我们项目中需要不同环境切换等等,用rake来管理大大提高效率.
[rake用户手册](http://docs.rubyrake.org/user_guide/index.html) 
  
就用一些小demo来学习吧,动手是最好最有用的方式,再简单的demo也要动手去做,let's go.首先在项目目录下新建一个Rakefiles文件.然后添加如下代码:
        desc "describe you rake task"
        task :one do    
            p "first task"
        end            
  然后用rake  one 命令,就看到你想要看到的结果了,这里task是Rake最重要的方法。它的方法定义是：task(args, &block)。任务体是一个block.  

  依赖关系：在一个task执行之前，需要其它的task执行

        task ：one do
           p "first task"
        end

        task :two => :one do
          p "second task"
        end

运行 rake two，结果自己试试。。

rake文件里有很多的任务时，你需要关注它们的命名冲突问题，因为task的命名需要是唯一的。这时候命名空间（namespace）就是一个自然的解决方案。你可以为上面的2个任务定义一个叫做install的命名空间
 
        namesapce :install do
            task :one do
            p  "sdsds"
        end
        ...
        end
运行rake --tasks就可以看到rake的结构，运行方式是rake install:one的方式。

在一个task中调用其它的task，使用Rake::Task["task_name"].invoke 方法。
        task :four do
           Rake::Task["one"].invoke
          p "four rask"
        end
其实这和依赖是一样的，写法不一样而已，它也等于
       task :four => :one do
         p "four rask"
      end

可以为Rake增加一个默认任务，这样可以简单地用Rake命令来触发这个默认任务,
      task :deault =>[:one]
 
import("loadpath")
这样的语句导入各个子任务即可，不同的任务写到不同的文件里面就不会一团糟了。而且，import 同 Ruby 自己的 require 不一样，import 并不是立即进行导入的，而是在整个 Rakefile 执行结束之后才全部导入，因此，可以在任意的地方写 import ，而不用担心依赖关系，需要共享的变量之类的只要在主 Rakefile 中定义了即可  


