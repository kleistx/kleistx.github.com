---
layout: post
title: "Octopress折腾记"
date: 2012-05-13 17:36
comments: true
categories: [tools]
---
年初，发现很多技术博客（特别是喜好ruby的）都换了新面孔，简洁大气，开始还以为是wordpress的new theme，后来发现不是，他们用了Ocotopress，其中贴代码的部分十分漂亮，让我心动不已，自己本来有个wordpress的博客在SAE上，但老要买云豆很不爽，于是也摆弄摆弄Octopress，顺便把尘封已久的github账号拿出来抖抖灰，抽空折腾了两天，小记一下，我是将其部署至github，本地系统为mac，整个过程参考了[此处](http://octopress.org/docs/deploying/)。

###了解
* [Why Octopress?](http://blog.xdite.net/posts/2011/10/07/what-is-octopress/)
* [Markdown语法](http://wowubuntu.com/markdown/#autoescape)

###环境
如果你已经安装了最新的git和ruby，请忽略。

* [安装最新的ruby](http://ihower.tw/rails3/installation.html)
* [使用homebrew在mac下安装git ](http://book.51cto.com/art/201107/278761.htm)

###github
* 注册github账号，这个很简单，不多说。
* [创建一个repository](http://help.github.com/create-a-repo/)，如果你的账号形如 username，你创建的这个repository名称应为username.github.com，切记。
* [设置你在github上的ssh key](http://help.github.com/mac-set-up-git/)  

###安装Octopress
####下载Octopress  
	git clone git://github.com/imathis/octopress.git octopress
	cd octopress # 如果使用 RVM, 这里将会询问是否信任该 .rvmrc (选yes).
	ruby --version # 应该大于 Ruby 1.9.2

 .rvmrc文件中默认的ruby版本是1.9.2，假如你的ruby版本高于1.9.2，修改它。
####安装相应的gem  
	bundle update
####生成模板文件  
	rake install
###部署至github
####连接至github  
	rake setup_github_pages
  输入url地址，形如
  
	git@github.com:username/username.github.com.git
####生成静态页面  
	rake generate
####本地预览
	rake preview 
访问http://localhost:4000 查看本地效果，用ctrl+c可结束  。 
####部署
	rake deploy
访问 http://username.github.com 查看博客服务器效果  。 
###更新Octopress
每过一段时间，可能需要[更新一下Octopress版本](http://octopress.org/docs/updating/)，使之保持最新。
###配置博客信息
修改Octopress/_config.yml，参考http://octopress.org/docs/configuring/ ，示例如下，若包含中文，请将文件格式保存成utf-8的格式。

	url: http://kleistx.github.com/
	title: Xway
	subtitle: 
	author: xway
	simple_search: http://google.com/search
	description:

###创建新文章和新页面
	rake new_post["article name"]
	rake new_page["page name"]
文章创建后，可在source/_post文件夹下找到，推荐使用[Mou](http://mouapp.com/)编辑，更新之后需要重新生成静态页面，并重新部署。

	rake generate
	rake deploy
###使用Disqus评论系统
注册一个Disqus账号，记住shortname，然后打开”_config.yml”，找到disqus相关的配置项，修改即可：

	#Disqus Comments
	disqus_short_name: xway
	disqus_show_comment_count: true
记住冒号后面是有空格的。
###贴代码
Octopress使用了pygments，所以支持多种编程语言的代码，具体方法见[此处](http://octopress.org/docs/plugins/codeblock/)，我常用的方式是：

	｛% codeblock %｝
	  Awesome code snippet
	｛% endcodeblock  %｝
	
效果如下：

{% codeblock %}
Awesome code snippet
{% endcodeblock %}

或者

	｛% codeblock Time to be Awesome - awesome.rb %｝
	  puts "Awesome!" unless lame
	｛% endcodeblock %｝

效果如下：

{% codeblock Time to be Awesome - awesome.rb %}
puts "Awesome!" unless lame
{% endcodeblock %}
###添加”关于我”
在source下新建about目录，并在里面添加`index.markdown`文件。  
编辑导航条`source/_includes/custom/navigation.html`  
注意:`index.markdown`文件需要加上头，否则会找不到。  
###首页只显示摘要
在文中加入`<!--more-->`来控制摘要截取位置。  
修改_config.yml里的excerpt_link。  
###更新文章
	rake new_post["title"]
	rake generate       #发布文件到public目录
	rake watch          #监控source和sass目录的变动
	rake preview        #启动服务器并监控变动，通过http://localhost:4000预览
	rake generate
	rake deploy
###保存源代码
在项目里建立source分支用于保存所有的代码（配置，sass，文章）。

	git add .
	git commit -m 'blog'
	git push origin source