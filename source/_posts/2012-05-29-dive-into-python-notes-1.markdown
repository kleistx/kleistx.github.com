---
layout: post
title: "Dive Into Python 3 ：Notes 1"
date: 2012-05-29 22:25
comments: true
categories: [python,notes]
---
以下是我最近阅读[《深入python3》](http://woodpecker.org.cn/diveintopython3/table-of-contents.html#your-first-python-program)后记的笔记。

##第一份Python程序
###条件表达式
C语言中的条件表达式是这样 `x = condition ? true_value : false_value`  
python中是这样 `x = true_value if condition else false_value`，例如：

	X = true if logging else flase
	
###文档字符串docstring
紧跟在函数声明的下一行，用引号或三重引号包围，描述函数必备，在有些ide，当用鼠标指向函数名时，其docstring以tooltip的方式显示出来，很便利。  
[PEP 257: Docstring](http://www.python.org/dev/peps/pep-0257/) 约定解释了用什么来从大量的 docstring 中分辨出一个好的 docstring。
###异常
在被调用程序中使用 raise 语句来抛出异常，在调用程序中使用 try...except 块来处理异常。
###`__name__` 和 `__main__`
	if __name__ == '__main__'
这种写法的原理：模块是对象，所有的模块对象都有一个内置的属性 `__name__` ,如果import这个模块，那么`__name__`就等于这个模块的文件名（不包含路径和扩展名），但如果将此模块当做一个独立的程序来运行的话，`__name__` 将是一个特殊的默认值`__main__`，所以才用此if语句来判断。

##内置数据类型
八大内置数据类型，booleans，numbers，strings，bytes，lists，tuples，sets，dictionaries。  

用 type() 函数来检测任何值或变量的类型。  
用 isinstance() 函数判断某个值或变量是否为给定某个类型。如：

	isinstance(1, int)
	
###常见数值运算 
其他的都和c语言差不多，除了这两个： `//`除后取整 ； `**`求幂运算 ； 
###列表`[ ]`
* 列表用 `[ ]` 来定义。
* 可用`list[-1]`来从反方向索引。
* 布尔上下文，空列表为false，有元素的列表为true。  

切片几种用法   

	list[1:3] ; list[1:-1]; list[0:3]; list[:3]; list[3:]; list[:]		
 list[:]是一个新列表，与list并不一样，只是拥有完全相同的元素，是对列表进行复制的一条捷径。

* 增加元素 
  + +号，`list+[2.0,3] `，此方法效率较低。
  + append()，`list.append(5)`，append只能添加一个元素。
  + insert()，`list.insert(2,'j')`，insert需要一个索引位置。
  + expend()，`list.extend(['four', 'Ω']) `，参数是列表，主要用于合并列表。
 * 删除元素
   + del，直接 `del list[1]` ，命令行用法。
   + remove()，`list.remove(item)` ，删除item的第一次出现。
   + pop(), `list.pop()` 删除最后的元素，`list.pop(2) `删除该索引位置的元素，此方法返回被pop的值。
* 检索
  + count()，`list.count(item)` 返回item出现的次数。
  + in，`if x in list` 返回布尔值。
  + index()，`list.index(item)` 返回item的索引值。

###元组`( )`
以下是与list的相同点：   

* 元组用`（ ）`来定义，可以用`[ ]`来索引。  
* 负索引同于列表。
* 布尔上下文同于列表。
* 切片同于列表。
* 索引，也可用count(),in,index()。

以下是与list的不同点：

* 创建单元素元组，须如此`（“a”，）`，加上逗号，python才认元组，否则认为括号多余。
* 元组不可修改，故无增加删除之方法。

**元组的优点：**

可同时赋多个值

	v=('a',2,true)
	(x,y,z) = v
 有多种用途，例如：

    (MONDAY,TUESDAY,WEDNESDAY,THURSDAY,FRIDAY,SATURDAY,SUNDAY)=range(7)
        
或者让函数返回元组，赋值给不同的变量。
* 进行遍历操作比列表快。
* “写保护”，让代码更安全。
*  可用作字典键，列表不行。
*  与列表可以相互转换，`tuple()`冻结列表，`list()`融合元组。	

###集合`{ }`
* 集合用 `{ }` 来定义，用此方法定义时，至少含一个元素，纯`{ }`是表示空字典。
* 创建空集合，用不带参数的`set()`，如 `a_set = set()` 不能用 `a_set = { }` 。
* 可用列表创建集合，如 `a_set = set(list) ` 初始化。
* 集合装载唯一值，所以会自动消除重复元素。
* 增加元素
  + add()，增加单值，`a_set.add(11)`。
  + udate()，增加一个序列，list或者tuple，`a_set.update([10,30,59])`。
* 删除元素
  * discard() ，删除单值，`a_set.discard(10)` ，若值不存在，无反应。
  * remove()，删除单值， `a_set.remove(21)`，若值不存在，抛出错误。
  * pop()，弹出单值并返回该值，是随机弹出。
  * clear()，清空集合。

###字典`{ }`
*  集合装载唯一值，所以会自动消除重复元素。

同一字典的值可以装入不同数据类型和任何对象，但是键只能是数据类型，如：

    {1000: ['KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'],
     1024: ['KiB', 'MiB', 'GiB', 'TiB', 'PiB', 'EiB', 'ZiB', 'YiB']}

* 可以使用`len()`和`in`。
* 可以使用类似集合的增加和减少元素的方法。

##推导式
紧凑，直观，有点像sql语句。

###列表推导式  
对某列表中每个元素应用一个函数，将其映射到另一个列表。  
将a_list中的元素乘以3

	[elem * 3 for elem in a_list]
	
将当前目录下所有xml文件加上全路径

	[os.path.realpath(f) for f in glob.glob('*.xml')]  
	
过滤列表，返回当前目录下所有大小大于6000字节的py文件

	[f for f in glob.glob('*.py') if os.stat(f).st_size > 6000]

返回元组列表，每个元组包含文件字节数和绝对路径

	[(os.stat(f).st_size, os.path.realpath(f)) for f in glob.glob('*.xml')] 

###字典推导式
与列表推导式不同之处有两点：  
1.被花括号包围而非方括号。   
2.对于每个元素它包含由冒号分隔的两个表达式。冒号前为键，其后为值。  

	{f:os.stat(f) for f in glob.glob('*.py')}

也可以加上if形成过滤字典
	
	{os.path.splitext(f)[0]:humansize.approximate_size(meta.st_size) \ 
		 for f, meta in metadata_dict.items() if meta.st_size > 6000} 

小技巧，交换字典的键和值

	{value:key for key, value in a_dict.items()}

###集合推导式
与列表推导式和字典推导式类似，其输入不一定是集合，可以是任何序列：

	 {2**x for x in range(10)} 