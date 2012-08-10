---
layout: post
title: "在mac上使用MySQLdb"
date: 2012-07-01 17:48
comments: true
categories: python
---
Django的缘故，需要用到python下的mysql接口，但mac上安装MySQLdb比我想象的要麻烦的多，看起来一切顺利，就是用不成，折腾了半天才发现一些注意事项，现将其写在这里：

##安装步骤

###1.安装正确的python和mysql版本
若未安装python和mysql，可以使用二者官方网站上的img镜像安装，但记住若你的mac os是64位的，一定安装**64位**的版本。  
若已安装python和mysql，用file命令搞清楚mac上的python和mysql是什么版本（32位还是64位）：   
  
    file $(which python)
    file $(which mysql)

现在的mac基本都是64位，看看是不是，千万别搞错，否则安装肯定出错。    

###2.增加路径  
安装上述软件之后，须将其加入环境变量path，一般来说，python的路径在`/Library/Frameworks/Python.framework/Versions/2.7/bin`，mysql的路径在`/usr/local/mysql/bin`，增加方法：

    sudo vim /etc/paths
将路径添加在里面即可。 

###3.安装MySQL-python  
使用pip或者easy_install来安装MySQL-python，建议用pip安装：
  
    pip install  MySQL-python

完成之后进入python，import一下：
          
    import MySQLdb
    
若无问题，则安装成功。 

###4.卸载mysql的问题  
mac下用DMG格式的mysql安装的话，出错后卸载很麻烦，卸载方法如下：

    sudo rm /usr/local/mysql
    sudo rm -rf /usr/local/mysql*
    sudo rm -rf /Library/StartupItems/MySQLCOM
    sudo rm -rf /Library/PreferencePanes/My*
    vim /etc/hostconfig and removed the line MYSQLCOM=-YES-
    rm -rf ~/Library/PreferencePanes/My*
    sudo rm -rf /Library/Receipts/mysql*
    sudo rm -rf /Library/Receipts/MySQL*
    sudo rm -rf /var/db/receipts/com.mysql.*

看起来很简单，可是浪费了我整个下午。