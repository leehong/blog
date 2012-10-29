---
layout: post
title: "ubuntu更新"
date: 2012-10-29 08:55
comments: true
categories: linux 
---

由于最近某党召开大会,导致于网速大降,在更新过程中不知道怎么就死掉了,然后出现了声音没了,再次更新提示dpkg 被中断，您必须手工sudo dpkg --configure -a 解决此问题,然后我就只好听话的sudo dpkg --configure -a 一下,结果在设置的过程中再次卡死,无赖,只有google了,然后大神就告诉我sudo  apt-get dist-upgrade,果然就搞定了.然后我郁闷了,为什么就会出现遮掩的情况呢?平时就都是upgrade就ok了么?这次怎么就挂掉了呢?原来apt-get upgrade源升级,会产生一些依赖,必须dis-upgrade来升级一下.

###顺便附上来源于ubuntu社区[Apt和dpkg快速参考](http://wiki.ubuntu.org.cn/Apt%E5%92%8Cdpkg%E5%BF%AB%E9%80%9F%E5%8F%82%E8%80%83)   
      apt-cache search # ------(package 搜索包)
      apt-cache show #------(package 获取包的相关信息，如说明、大小、版本等)
      sudo apt-get install # ------(package 安装包)
      sudo apt-get install # -----(package - - reinstall 重新安装包)
      sudo apt-get -f install # -----(强制安装?#"-f =
      --fix-missing"当是修复安装吧...)
      sudo apt-get remove #-----(package 删除包)
      sudo apt-get remove - - purge # ------(package
      删除包，包括删除配置文件等)
      sudo apt-get autoremove --purge # ----(package
      删除包及其依赖的软件包+配置文件等（只对6.10有效，强烈推荐）)
      sudo apt-get update #------更新源
      sudo apt-get upgrade #------更新已安装的包
      sudo apt-get dist-upgrade # ---------升级系统
      sudo apt-get dselect-upgrade #------使用 dselect 升级
      apt-cache depends #-------(package 了解使用依赖)
      apt-cache rdepends # ------(package
      了解某个具体的依赖?#当是查看该包被哪些包依赖吧...)
      sudo apt-get build-dep # ------(package 安装相关的编译环境)
      apt-get source #------(package 下载该包的源代码)
      sudo apt-get clean && sudo apt-get autoclean #
      --------清理下载文件的存档 && 只清理过时的包

suapt-get install
下载 以及所有倚赖的包裹，同时进行包裹的安装或升级。如果某个包裹被设置了 hold (停止标志，就会被搁在一边(即不会被升级)。更多 hold 细节请看下面。

apt-get remove [--purge]
移除 以及任何倚赖这个包裹的其它包裹。
--purge 指明这个包裹应该被完全清除 (purged) ，更多信息请看 dpkg -P。

apt-get update
升级来自 Debian 镜像的包裹列表，如果你想安装当天的任何软件，至少每天运行一次，而且每次修改了
/etc/apt/sources.list 后，必须执行。

apt-get upgrade [-u]
升级所以已经安装的包裹为最新可用版本。不会安装新的或移除老的包裹。如果一个包改变了倚赖关系而需要安装一个新的包裹，那么它将不会被升级，而是标志为 hold。apt-get update 不会升级被标志为 hold 的包裹 (这个也就是 hold 的意思)。请看下文如何手动设置包裹为 hold。我建议同时使用 '-u' 选项，因为这样你就能看到哪些包裹将会被升级。

apt-get dist-upgrade [-u]
和 apt-get upgrade 类似，除了 dist-upgrade 会安装和移除包裹来满足倚赖关系。因此具有一定的危险性。

apt-cache search
搜索满足 的包裹和描述。

apt-cache show
显示 的完整的描述。

apt-cache showpkg
显示 许多细节，以及和其它包裹的关系。

dselect
console-apt
aptitude
gnome-apt
APT 的几个图形前端(其中一些在使用前得先安装)。这里 dselect 无疑是最强大的，也是最古老，最难驾驭。

普通 Dpkg 用法
dpkg -i
安装一个 Debian 包裹文件，如你手动下载的文件。

dpkg -c
列出 的内容。

dpkg -I
从 中提取包裹信息。

dpkg -r
移除一个已安装的包裹。

dpkg -P
完全清除一个已安装的包裹。和 remove 不同的是，remove 只是删掉数据和可执行文件，purge 另外还删除所有的配制文件。

dpkg -L
列出 安装的所有文件清单。同时请看 dpkg -c 来检查一个 .deb 文件的内容。

dpkg -s
显示已安装包裹的信息。同时请看 apt-cache 显示 Debian 存档中的包裹信息，以及 dpkg -I 来显示从一个 .deb 文件中提取的包裹信息。

dpkg-reconfigure
重新配制一个已经安装的包裹，如果它使用的是 debconf (debconf 为包裹安装提供了一个统一的配制界面)。你能够重新配制 debconf 它本身，如你想改变它的前端或提问的优先权。例如，重新配制 debconf，使用一个 dialog 前端，简单运行：

dpkg-reconfigure --frontend=dialog debconf (如果你安装时选错了，这里可以改回来哟：)

echo " hold" | dpkg --set-selections
设置 的状态为 hlod (命令行方式)

dpkg --get-selections ""
取的 的当前状态 (命令行方式)

支持通配符，如：
Debian:~# dpkg --get-selections *wine*
libwine                                         hold
libwine-alsa                                    hold
libwine-arts                                    hold
libwine-dev                                     hold
libwine-nas                                     hold
libwine-print                                   hold
libwine-twain                                   hold
wine                                            hold
wine+                                           hold
wine-doc                                        hold
wine-utils                                      hold

例如：
大家现在用的都是 gaim-0.58 + QQ-plugin，为了防止 gaim 被升级，我们可以采用如下方法：

方法一：
Debian:~# echo "gaim hold" | dpkg --set-selections
然后用下面命令检查一下：
Debian:~# dpkg --get-selections "gaim"
gaim                                            hold
现在的状态标志是 hold，就不能被升级了。

如果想恢复怎么办呢?
Debian:~# echo "gaim install" | dpkg --set-selections
Debian:~# dpkg --get-selections "gaim"
gaim                                            install
这时状态标志又被重置为 install，可以继续升级了。

同志们会问，哪个这些状态标志都写在哪个文件中呢?
在 /var/lib/dpkg/status 里，你也可以通过修改这个文件实现 hold。

有时你会发现有的软件状态标志是 purge，不要奇怪。
如：事先已经安装了 amsn，然后把它卸了。
apt-get remove --purge amsn
那么状态标志就从 install 变成 purge。

方法二：
在/etc/apt 下手动建一个 preferences 文件
内容：
Package: gaim
Pin: version 0.58*
保存

dpkg -S
在包裹数据库中查找 ，并告诉你哪个包裹包含了这个文件。(注：查找的是事先已经安装的包裹)do apt-get check #-------检查是否有损坏的依赖
  apt-get install
下载 以及所有倚赖的包裹，同时进行包裹的安装或升级。如果某个包裹被设置了 hold (停止标志，就会被搁在一边(即不会被升级)。更多 hold 细节请看下面。

apt-get remove [--purge]
移除 以及任何倚赖这个包裹的其它包裹。
--purge 指明这个包裹应该被完全清除 (purged) ，更多信息请看 dpkg -P。

apt-get update
升级来自 Debian 镜像的包裹列表，如果你想安装当天的任何软件，至少每天运行一次，而且每次修改了
/etc/apt/sources.list 后，必须执行。

apt-get upgrade [-u]
升级所以已经安装的包裹为最新可用版本。不会安装新的或移除老的包裹。如果一个包改变了倚赖关系而需要安装一个新的包裹，那么它将不会被升级，而是标志为 hold。apt-get update 不会升级被标志为 hold 的包裹 (这个也就是 hold 的意思)。请看下文如何手动设置包裹为 hold。我建议同时使用 '-u' 选项，因为这样你就能看到哪些包裹将会被升级。

apt-get dist-upgrade [-u]
和 apt-get upgrade 类似，除了 dist-upgrade 会安装和移除包裹来满足倚赖关系。因此具有一定的危险性。

apt-cache search
搜索满足 的包裹和描述。

apt-cache show
显示 的完整的描述。

apt-cache showpkg
显示 许多细节，以及和其它包裹的关系。

dselect
console-apt
aptitude
gnome-apt
APT 的几个图形前端(其中一些在使用前得先安装)。这里 dselect 无疑是最强大的，也是最古老，最难驾驭。

普通 Dpkg 用法
dpkg -i
安装一个 Debian 包裹文件，如你手动下载的文件。

dpkg -c
列出 的内容。

dpkg -I
从 中提取包裹信息。

dpkg -r
移除一个已安装的包裹。

dpkg -P
完全清除一个已安装的包裹。和 remove 不同的是，remove 只是删掉数据和可执行文件，purge 另外还删除所有的配制文件。

dpkg -L
列出 安装的所有文件清单。同时请看 dpkg -c 来检查一个 .deb 文件的内容。

dpkg -s
显示已安装包裹的信息。同时请看 apt-cache 显示 Debian 存档中的包裹信息，以及 dpkg -I 来显示从一个 .deb 文件中提取的包裹信息。

dpkg-reconfigure
重新配制一个已经安装的包裹，如果它使用的是 debconf (debconf 为包裹安装提供了一个统一的配制界面)。你能够重新配制 debconf 它本身，如你想改变它的前端或提问的优先权。例如，重新配制 debconf，使用一个 dialog 前端，简单运行：

dpkg-reconfigure --frontend=dialog debconf (如果你安装时选错了，这里可以改回来哟：)

echo " hold" | dpkg --set-selections
设置 的状态为 hlod (命令行方式)

dpkg --get-selections ""
取的 的当前状态 (命令行方式)

支持通配符，如：
Debian:~# dpkg --get-selections *wine*
libwine                                         hold
libwine-alsa                                    hold
libwine-arts                                    hold
libwine-dev                                     hold
libwine-nas                                     hold
libwine-print                                   hold
libwine-twain                                   hold
wine                                            hold
wine+                                           hold
wine-doc                                        hold
wine-utils                                      hold

例如：
大家现在用的都是 gaim-0.58 + QQ-plugin，为了防止 gaim 被升级，我们可以采用如下方法：

方法一：
Debian:~# echo "gaim hold" | dpkg --set-selections
然后用下面命令检查一下：
Debian:~# dpkg --get-selections "gaim"
gaim                                            hold
现在的状态标志是 hold，就不能被升级了。

如果想恢复怎么办呢?
Debian:~# echo "gaim install" | dpkg --set-selections
Debian:~# dpkg --get-selections "gaim"
gaim                                            install
这时状态标志又被重置为 install，可以继续升级了。

同志们会问，哪个这些状态标志都写在哪个文件中呢?
在 /var/lib/dpkg/status 里，你也可以通过修改这个文件实现 hold。

有时你会发现有的软件状态标志是 purge，不要奇怪。
如：事先已经安装了 amsn，然后把它卸了。
apt-get remove --purge amsn
那么状态标志就从 install 变成 purge。

方法二：
在/etc/apt 下手动建一个 preferences 文件
内容：
Package: gaim
Pin: version 0.58*
保存

dpkg -S
在包裹数据库中查找 ，并告诉你哪个包裹包含了这个文件。(注：查找的是事先已经安装的包裹)