---
layout: post
title: "我的新博客"
date: 2012-10-01 20:14
comments: true
categories:  octopress
---

新博客弄好了,之前用jekyll搭建的blog还是没有octopress强大啊,很感谢[imathis](https://github.com/imathis)的贡献...

博客的主题是参考别人的设置的CSS的,具体大家可以google,没有什么难度,还有就是文章的分类,这个在plugins目录下的category_generate.rb修改,就是想里面的方法替换掉
         def write_category_index(category_dir, category)
            index = CategoryIndex.new(self, self.source, category_dir, category)
            index.render(self.layouts, site_payload)
            index.write(self.dest)
            # Record the fact that this page has been added, otherwise Site::cleanup      will remove it.     
            self.pages <<      index    
      
              # Create an Atom-feed for each index.
              feed = CategoryFeed.new(self, self.source, category_dir, category)
              feed.render(self.layouts, site_payload)
              feed.write(self.dest)
              # Record the fact that this page has been added, otherwSite::cleanup      will remove it.
            self.pages << feed
        end

      然后就是修改_config.yml配置文件,octopress官方文档说的很清楚了,然后就是豆瓣个人秀,去自己的豆瓣开发者个人中心,然后_include/asides目录下添加douban.html,然后在_config.yml的default_asides里面加入douban.html就ok了.

      就这样博客就搭建好了,关于octopress的教材网上很多的,我就粗略的介绍一下 :)....