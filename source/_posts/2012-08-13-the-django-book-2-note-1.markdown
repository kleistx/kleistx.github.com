---
layout: post
title: "The Django Book 2 ：Notes 1"
date: 2012-08-13 11:11
comments: true
categories: [python,notes]
---
##入门
1. 先安装好python和mysql等。  
2. 下载django，运行命令 `python setup.py install` 安装之。  
3. 选择好安装目录，进入，运行命令 `django-admin.py startproject mysite` 。
4. cd mysite，运行命令 `python manage.py runserver` ，也可以指定端口 `python manage.py runserver 0.0.0.0:8000` 。
5. 访问 http://127.0.0.1:8000/，看Django是否启动。

##视图和URL配置
###静态页面请求处理过程
1. 进来的请求转/hello/
2. Django通过在ROOT_URLCONF配置来决定根URLconf
3. Django在URLconf的所有URL模式中，查找第一个匹配/hello/的条目
4. 如果找到匹配，将调用相应的视图函数
5. 视图函数返回一个HttpResponse
6. Django转换HttpResponse为一个适合的HTTP response，以web page显示出来

##模板系统
###名词
**变量**：两个大括号包围，`{{person_name}}`  
**标签**：大括号和百分号包围，`{% if ordered_warranty %}`   
**过滤器**：用（|）来调用，`{{ship_date|date:"Fj,Y"}}`    
标签和过滤器的列表，见附录F，可以用其创建自己的tag和filters。  
###如何使用模板
1.创建模板对象，就是将模板的代码或者html文件变成Template

    from django.template import Template
    t = Template('Your name is {{ name }}.')
    
2.渲染模板，就是将模板中的变量用Context对象中的变量替换

    form diango.template import Context
    t.render(Context({'name':'john'}))
    
###常用模板标签
1.`{%if%}` ，标签需要成对出现，可以嵌套，`{%else%}`可选。

    {% if athlete_list %}
        ……
    {% else %}
        …… 
    {% endif %}
`{%ifequal%} 和 {%ifnotequal%}`，用来比较变量

    {% ifequal section 'sitenews' %} 
        <h1>Site News</h1>
    {% else %}
        <h1>No News Here</h1>
    {% endifequal %}

2.`{%for%}`，标签需要成对出现，可以嵌套

    {% for athlete in athlete_list %}
        <li>{{ athlete.name }}</li> 
    {% endfor %}
a.增加reversed使列表反向迭代：

    {% for athlete in athlete_list reversed %} 
        ……
    {% endfor %}
b.循环之前检测列表大小，列表为空时输出提示：

    {% if athlete_list %}
        {% for athlete in athlete_list %}
            <p>{{ athlete.name }}</p> 
        {% endfor %}
    {% else %}
        <p>There are no athletes. Only computer programmers.</p>
    {% endif %}
  以上语句等价于使用`{%empty%}`，如下：
  
    {% for athlete in athlete_list %} 
        <p>{{ athlete.name }}</p>
    {% empty %}
        <p>There are no athletes. Only computer programmers.</p>
    {% endfor %}
  方便的多，请使用`{%empty%}`。
c.forloop变量,详见文档。  

3.注释
    
    {# This is a comment #}
  或者
    
    {% comment %}
        This is a multiline comment. 
    {% endcomment %}
    
###过滤器
过滤器使用管道字符，可以套接（一个管道输出作为下一个管道输入），例如查找列表第一个元素并转换为大写：
    
    {{my_list|first|upper}}
过滤器可带参数，以双引号包围，例如显示变量bio的前30个词：

    {{ bio|truncatewords:"30" }}
几个重要过滤器：  
`addslashes `:添加反斜杠到任何反斜杠、单引号、双引号之前。这在JavaScript处理中十分有用。  
`date`：按指定格式参数化date或datetime对象。  
`length`返回变量的长度。

###在视图中使用模板
1.定位至settings.py配置文件，找到TEMPLATE_DIR，设置模板目录，别忘了*逗号*。  
2.使用`render_to_response（）`  

    from django.shortcuts import render_to_response 
    import datetime
    
    def current_datetime(request):
        now = datetime.datetime.now()
        return render_to_response('current_datetime.html', 
                                    {'current_date': now})
如果不提供第二个参数，render_to_response()使用一个空字典。  
3. 使用`locals()` ，上面的代码可修改为：

    def current_datetime(request):
        current_date = datetime.datetime.now()
        return render_to_response('current_datetime.html', locals())

locals()可以让模板中的变量和代码中的变量相通，locals()记住所有已定义的局部变量。

###模板继承
{%include%}标签仅仅是引入部分内容

    {% include 'includes/nav.html' %}
对这部分内容无法修改，比如引入header，对于其中的title就无法修改，而模板继承就可以。要用到`{% extends %}`标签，基础模板范例（base.html）：

    <!DOCTYPE HTML PUBLIC "//W3C//DTD HTML 4.01//EN"> <html lang="en">
    <head>
        <title>{% block title %}{% endblock %}</title> </head>
    <body>
        <h1>My helpful timestamp site</h1> 
        {% block content %}{% endblock %} 
        {% block footer %}
        <hr>
        <p>Thanks for visiting my site.</p> {% endblock %}
    </body> 
    </html>
其中`{%block%}`标签告诉模板引擎可以重载这些部分。  
使用基础模板（current_datetime.html）：

    {% extends "base.html" %}
    {% block title %}The current time{% endblock %}
    {% block content %}
        <p>It is now {{ current_date }}.</p> 
    {% endblock %}
注意：  
1.`{% extends %}`必须为第一个标签。  
2.基础模板中`{% block %}`标签越多越好。  

##数据模型
###数据库查询简单方法
使用MySQLdb类库执行SQL语句查询并处理：

    db = MySQLdb.connect(user='me',db='mydb',passwd='secret',host='localhost')
    cursor = db.cursor()
    cursor.execute('SELECT name FROM books ORDER BY name')
    db.close()
###数据库配置
在'setting.py'中找到DATABASES设置数据库，注意sqlite和其他数据库的用法不同。

###第一个app
一个project可以包含很多app，如果使用了Django的数据库层（模型），必须建一个app，模型放在apps中。

假设要建立一个books的app，步骤如下：  

1.在`manage.py`目录下，建app目录：  

    python manage.py startapp books
2.修改`models.py`，设计数据库表结构,注意添加`__unicode__`函数：

    from django.db import models
    
    class Publisher(models.Model):
        name = models.CharField(max_length=30)
        address = models.CharField(max_length=50)
        city = models.CharField(max_length=60)
        state_province = models.CharField(max_length=30)
        country = models.CharField(max_length=50)
        website = models.URLField()        
        def __unicode__(self):
		    return self.name
        
    class Author(models.Model):
        first_name = models.CharField(max_length=30) 
        last_name = models.CharField(max_length=40)
        email = models.EmailField()
        def __unicode__(self):
		    return self.first_name
        
    class Book(models.Model):
        title = models.CharField(max_length=100)
        authors = models.ManyToManyField(Author)
        publisher = models.ForeignKey(Publisher)
        publication_date = models.DateField()
        def __unicode__(self):
		    return self.first_name
3.安装数据模型，在`setting.py`的`INSTALLED_APPS`最后添加books：
    
    INSTALLED_APPS = (
     'books',
    )
4.验证数据模型的有效性：
    python manage.py validate
5.根据`models.py`中的描述生成创建表的sql语句：
    python manage.py sqlall books
6.使用5中生成的语句创建表：
    python manage.py syncdb
