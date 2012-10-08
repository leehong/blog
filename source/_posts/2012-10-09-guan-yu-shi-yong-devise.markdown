---
layout: post
title: "关于使用devise"
date: 2012-10-09 01:08
comments: true
categories:  ruby&rails
---

[devise](https://github.com/plataformatec/devise)是rails的一个gem,rails 的gem的好处就是各种便利,即使你不知道它的内在原理,你也可以很好的使用,只要你按照doc操作.关于devise的好处就不用说了,使用它之后不用不用在关心帐号的各种处理了,只要你按照它的步骤操作,一分钟就搞定了,给我们带来了很大的便利.

怎么使用devise就不详细说了,官方和现在的资料已经很仔细了,我要说的是这个方法after_sign_up_path_for ,这个方法里面你对制定的帐号登陆之后跳转的到指定的path,例如:
      private 
      def after_sign_in_path_for(resource)
          这里进行处理
          if resource.is_a?(User)  
            这里指定的path
          else
            super
          end
      end
对应的登出也有这样的一个方法 
      private
      def after_sign_out_path_for(resource_or_scope)
          这里一般是跳到首页了
      end