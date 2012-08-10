---
layout: post
title: "回文数和利克瑞尔数"
date: 2012-08-09 00:21
comments: true
categories: python
---
回文数就是看起来左右对称的自然数，比如121、876678，如果将一个自然数与将该数按位翻转后形成的新数相加，并将此过程反复迭代后，往往都会得出一个回文数。例如：

    56在一次迭代后形成回文数：56+65 = 121.  
    57在两次迭代后形成回文数：57+75 = 132, 132+231 = 363.  
    89经过少有的24次迭代（它是10,000以下的数中迭代次数最多的）而得到回文数8813200023188.    
假如某数翻转又不停迭代相加后得不出回文数呢？那么它就被称为利克瑞尔数（Lychrel Number）。   
利克瑞尔数实际上很罕见，而且难以证明，90%的10000以内的数经过不多于7次迭代后会变成回文数， 
唯一例外的是196，至今还未被迭代成回文数，也许它就是最小的利克瑞尔数。  
科普完毕，下面写个小程序来验证利克瑞尔数：
{% codeblock lang:python %}
def Lychrel_Number(num,counter):
	'''验证一个数是否为利克瑞尔数'''
	sum = num + reverse_num(num)
	print '%d+%d=%d'%(num,reverse_num(num),sum)
	counter = counter + 1    #迭代计数
	if is_palindrome(sum):
		print('%d times!'%counter)
		return
	else:
		Lychrel_Number(sum,counter) 
		
def reverse_num(n):
	'''将一个数翻转'''
	l = num_to_list(n)    		
	r = 0
	for i in range(len(l)):
		r = r + l.pop()*pow(10,i)    	
	return(r)

def num_to_list(n):
	'''将一个数从个位开始放入一个列表'''
	l=[]
	while n:
		l.append(n%10)
		n = n/10
	return l

def is_palindrome(n):
	'''判断回文数'''
	original = num_to_list(n)
	result = original[::-1]		#翻转序列
	return original == result

num = input("please enter a number:")
Lychrel_Number(num,0)
{% endcodeblock %}

运行示例如下：

    please enter a number:99
    99+99=198
    198+891=1089
    1089+9801=10890
    10890+9801=20691
    20691+19602=40293
    40293+39204=79497
    6 times!
别验证196哦，会溢出的哦！