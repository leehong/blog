---
layout: post
title: "linux常用的8个命令"
date: 2012-10-06 23:36
comments: true
categories: linux
---
###来源:[外刊IT评论网](http://www.aqee.net/8-linux-commands-every-developer-should-know/)
###英文来源:[8 Linux Commands Every Developer Should Know](http://blog.jayfields.com/2012/08/8-linux-commands-every-developer-should.html)
- - - - - - - - - - - - - - 

每个程序员，在职业生涯的某个时刻，总会发现自己需要知道一些Linux方面的知识。我并不是说你应该成为一个Linux专家，我的意思是，当面对linux命令行任务时，你应该能很熟练的完成。事实上，学会了下面8个命令，我基本上能完成任何需要完成的任务。

注意：下面的每个命令都有十分丰富的文档说明。这篇文章并不是来详尽的展示每个命令的各种功用的。我在这里要讲的是这几个最常用的命令的最常见用法。如果你对linux命令并不是很了解，你想找一些这方面的资料学习，那这篇文章将会给你一个基本的指导。
让我们从处理一些数据开始。假设我们有两个文件，分别记录的订单清单和订单处理结果。

      order.out.log
      8:22:19 111, 1, Patterns of Enterprise Architecture, Kindle edition, 39.99
      8:23:45 112, 1, Joy of Clojure, Hardcover, 29.99
      8:24:19 113, -1, Patterns of Enterprise Architecture, Kindle edition, 39.99
      order.in.log
      8:22:20 111, Order Complete
      8:23:50 112, Order sent to fulfillment
      8:24:20 113, Refund sent to processing 

* ### cat
  
cat – 连接文件，并输出结果

cat 命令非常的简单，你从下面的例子可以看到。

          jfields$ cat order.out.log 
          8:22:19 111, 1, Patterns of Enterprise Architecture, Kindle edition, 39.99
          8:23:45 112, 1, Joy of Clojure, Hardcover, 29.99
          8:24:19 113, -1, Patterns of Enterprise Architecture, Kindle edition, 39.99  

就像它的说明描述的，你可以用它来连接多个文件。

      fields$ cat order.* 
      8:22:20 111, Order Complete
      8:23:50 112, Order sent to fulfillment
      8:24:20 113, Refund sent to processing
      8:22:19 111, 1, Patterns of Enterprise Architecture, Kindle edition, 39.99
      8:23:45 112, 1, Joy of Clojure, Hardcover, 29.99
      8:24:19 113, -1, Patterns of Enterprise Architecture, Kindle edition, 39.99 

如果你想看这些log文件的内容，你可以把它们连接起来并输出到标准输出上，就是上面的例子展示的。这很有用，但输出的内容可以更有逻辑些。

* ### sort

sort – 文件里的文字按行排序

此时sort命令显然是你最佳的选择。

      jfields$ cat order.* | sort
      8:22:19 111, 1, Patterns of Enterprise Architecture, Kindle edition, 39.99
      8:22:20 111, Order Complete
      8:23:45 112, 1, Joy of Clojure, Hardcover, 29.99
      8:23:50 112, Order sent to fulfillment
      8:24:19 113, -1, Patterns of Enterprise Architecture, Kindle edition, 39.99
      8:24:20 113, Refund sent to processing`

就像上面例子显示的，文件里的数据已经经过排序。对于一些小文件，你可以读取整个文件来处理它们，然而，真正的log文件通常有大量的内容，你不能不考虑这个情况。此时你应该考虑过滤出某些内容，把cat、sort后的内容通过管道传递给过滤工具。

* ### grep
 
grep, egrep, fgrep – 打印出匹配条件的文字行

假设我们只对Patterns of Enterprise Architecture这本书的订单感兴趣。使用grep，我们能限制只输出含有Patterns字符的订单。

        jfields$ cat  order.* | sort |grep Patterns
        8:22:19 111, 1, Patterns of Enterprise Architecture, Kindle edition, 39.99
        8:24:19 113, -1, Patterns of Enterprise Architecture, Kindle edition, 39.99

假设退款订单113出了一些问题，你希望查看所有相关订单——你又需要使用grep了。
      jfields$ cat order.*| sort | grep| ":\d\d 113, "
      8:24:19 113, -1, Patterns of Enterprise Architecture, Kindle edition, 39.99
      8:24:20 113, Refund sent to processing

  你会发现在grep上的匹配模式除了“113”外还有一些其它的东西。这是因为11 以匹配上书目或价格，加上额外的字符后，我们可以精确的搜索到我们想要的 
  现在我们已经知道了退货的详细信息，我们还想知道日销售和退款总额。但我心Patterns of Enterprise Architecture这本书的信息，而且只关心数量和价现在要做到是切除我们不关心的任何信息。

* ### cut

cut – 删除文件中字符行上的某些区域

又要使用grep，我们用grep过滤出我们想要的行。有了我们想要的行信息，我们就可以把它们切成小段，删除不需要的部分数据。

      jfields$ cat order.* | sort | grep Patterns
      8:22:19 111, 1, Patterns of Enterprise Architecture, Kindle edition, 39.99
      8:24:19 113, -1, Patterns of Enterprise Architecture, Kindle edition, 39.99
       jfields$ cat order.* | sort | grep Patterns | cut -d"," -f2,51, 39.99
       -1, 39.99

  现在，我们把数据缩减为我们计算想要的形式，把这些数据粘贴到Excel里立刻就能得到结果了。
  cut是用来消减信息、简化任务的，但对于输出内容，我们通常会有更复杂的形式。假设我们还需要知道订单的ID，这样可以用来关联相关的其他信息。我们用cut可以获得ID信息，但我们希望把ID放到行的最后，用单引号包上。

* ### sed

  sed – 一个流编辑器。它是用来在输入流上执行基本的文本变换。

  下面的例子展示了如何用sed命令变换我们的文件行，之后我们在再用cut移除无用的信息
      
      jfields$ cat order.* | sort | grep Patterns \
      | sed s/"[0-9\:]* \([0-9]*\)\, \(.*\)"/"\2, '\1'"/
      1, Patterns of Enterprise Architecture, Kindle edition, 39.99, '111'
      1, Patterns of Enterprise Architecture, Kindle edition, 39.99, '113'
      l mp-jfields01:~ jfields$ cat order.* | sort | grep Patterns \
      | sed s/"[0-9\:]* \([0-9]*\)\, \(.*\)"/"\2, '\1'"/ | cut -d"," -f1,4,5
      1, 39.99, '111'
      1, 39.99, '113'
  
  我们对例子中使用的正则表达式多说几句，不过也没有什么复杂的。正则表达式做了下面几种事情
  
      1、 删除时间戳
      2、 捕捉订单号
      3、 删除订单号后的逗号和空格
      4、 捕捉余下的行信息

里面的引号和反斜杠有点乱，但使用命令行时必须要用到这些。
一旦捕捉到了我们想要的数据，我们可以使用 \1 & \2 来存储它们，并把它们输出们想要的格式。我们还在其中加入了要求的单引号，为了保持格式统一，我们还加逗号。最后，用cut命令把不必要的数据删
现在我们有麻烦了。我们上面已经演示了如何把log文件消减成更简洁的订单形式我们的财务部门需要知道订单里一共有哪些书。

* ### uniq
uniq – 删除重复的行
下面的例子展示了如何过滤出跟书相关的交易，删除不需要的信复的信息。
      jfields$ cat order.out.log | grep "\(Kindle\|Hardcover\)" | cut -d"," -f3 | sort |      uniq -c     
         1  Joy      of Clojure     
         2  Patterns of Enterprise Architecture     
      
看起来这是一个很简单的任务。
这都是很好用的命令，但前提是你要能找到你想要的文件。有时候你会发现一些文件藏在很深的文件夹里，你根本不知道它们在哪。但如果你是知道你要寻找的文件的名字的话，这对你就不是个问题了。

* ### find

在上面的例子中我们处理了order.in.log和order.out.log这两个文件。这两个文件放在我的home目录里的。下面了例子将向大家展示如何在一个很深的目录结构里找到这样的文件。

      jfields$ find /Users -name "order*"
      Users/jfields/order.in.log
      Users/jfields/order.out.log

find命令有很多其它的参数，但99%的时间里我只需要这一个就够了。
简单的一行，你就能找到你想要的文件，然后你可以用cat查看它，用c但文件很小时，你用管道把它们输出到屏幕上是可以的，但当文件大，你也许应该用管道把它们输出给less命令。

* ### less

  less – 在文件里向前或向后移动

  让我们再回到简单的 cat | sort 例子中来，下面的命令就是将经过合并、排序后的内容输出到less命令里。在 less 命令，使用“/”来执行向前搜索，使用“？”命令执行向后搜索。搜索条件是一个正则表达式

      jfields$ cat order* | sort | less
      
 如果你在 less 命令里使用 /113.*，所有113订单的信息都会高亮。你也可*112，所有跟订单112相关的时间戳都会高亮。最后你可以用来退出le
 linux里有很丰富的各种命令，有些是很难用的。然而，学会了前面说的，你已经能处理大量的log分析任务了，完全不需要用脚本语言写程序来处理它们。

  
<p> {{ page.date | date_to_string }} </p>