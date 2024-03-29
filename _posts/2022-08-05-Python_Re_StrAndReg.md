---
title:  python-字符串与正则表达式
layout: default
---
[![返回](/assets/images/back.png)](../../../../2022/07/05/Python_Index.html)

# 1. 字符串
## 1.1 字符串格式化

```python
score = 100
'{}{}'.format("成绩:", score)
----
成绩:100
```

## 1.2 列表合并成字符串

```python
list_str = ["成绩",100]
",".join(list_str)
----
成绩,100
```

# 2. 正则表达式
## 2.1 正则表达式基础语法

**字符选择**

|符号|意义|符号|意义|
|:--:|:--|:--:|:--|
|.|除换行符以外的所有字母|||
|^|字符串开头|$|字符串结尾|
|\d|数字(或用[0-9])|\D|非数字|
|\w|字符|\W|非字符|
|\s|空格|\S|非空格|
|\b|单词边界(隐式占位)|\B|非单词边界|
|[abc]|abc中任意一个字母|[^abd]|除了abc之外的字母|
|[a-z]|a到z中的一个字母|[A-Z]|A到Z中的一个字母|
|aa\|bb|aa或bb|

notes：  
隐式占位；没有实体，例如 the world中e与空格之间的位置。

**数量选择**

|符号|意义|符号|意义|符号|意义|
|:--:|:--|:--:|:--|:--:|:--|
|?|0或1次|*|0或多次|+|1或多次|
|{n}|n次|{n,}|n次以上|{n,m}|n至m次|

notes:  
正则表达式默认贪婪匹配，在数量符后加上“?”可以变为非贪婪匹配  
例如：  

    字符串：1010  
    正则表达式1: /1\d*0/  返回：["1010"]  
    正则表达式2: /1\d*?0/ 返回：["10", "10"]  

**子表达式**

子表达式（exp）是一个大的表达式的一部分，把一个表达式划分为多个子表达式的目的是为了把那些子表达式当作一个独立的元素来使用。子表达式必须用(和)括起来。

**选择**

?=：exp1(?=exp2)：查找 exp2 前面的 exp1。

?<=：(?<=exp2)exp1：查找 exp2 后面的 exp1。

?!：exp1(?!exp2)：查找后面不是 exp2 的 exp1。

?<!：(?<!exp2)exp1：查找前面不是 exp2 的 exp1。
    
    方法:(?<=(子表达式1)).*?(?=(子表达式2))
    字符串：0123456789
    要求：输出0到9之间的数，且不包括0和9
    正则表达式：(?<=(0)).*?(?=(9))
    输出：12345678

**修饰符**

i: ignore,不区分大小写  
g: global,全局匹配，全部为一个整体  
m: multi line 多行匹配，一行为一个整体  
s: “."可以匹配“\n”

## 2.2 在python中使用正则表达式

在python中使用正则表达式，需要使用re模块。
re模式会先编译正则表达式，如果正则表达式错误会报错
使用格式：

```python
# 开始
import re
# 用法
## 用法一：预编译正则表达式
Match_rule = re.compile(pattern)
"""创建一个匹配模式对象，内含匹配规则，之后可以反复使用"""
Match_rule.match(string)
### 例子
Match_rule = re.compile(r"\d")

## 用法二：直接使用re.mod
MatchObject = re.match(pattern, string, flags)

# MatchObject的方法和属性
MatchObject.group(num)#返回匹配结果,num表示子表达式层数
MatchObject.start()#返回匹配结果的开头位置
MatchObject.end()#返回匹配结果的结束位置
MatchObject.span()#返回匹配结果的位置范围

# 具体使用
## 匹配
re.match(pattern, string, flags)
re.match(r'正则表达式', '待匹配的字符串', 修饰符)
# 修饰符: 多行模式 re.M 单行模式 re.S 忽略大小写 re.l 忽略空格和#后面的注释 
# re.L表示特殊字符集依赖当前环境 re.U 表示特殊字符集依赖于Unicode字符属性数据库(\w \W \b \B \d \D \s \S)
"""匹配成功返回一个MatchObject,不成功返回None"""
## 查找
re.search(pattern, string, flags)
"""查找匹配的第一个
如果查找到则返回查找到的值，否则返回为None。"""
re.findall(pattern, string, flags)
"""以列表格式返回所有匹配内容,注意在有分组时，即正则中有括号的情况
当只有一个分组，就只匹配分组里面的内容，然后返回列表
如果多个分组，就把每个分组看成一个单位，返回一个含有多个元组的列表
在python中，但一个分组头部出现 ?: 时，表示这是一个非捕获分组，正常进行匹配，不作为独立结果输出
"""
re.finditer # 会返回一个迭代器
[i.group() for i in re.finditer(pattern, string, flags)]
## 拆分
re.spilt(r'正则表达式', '待拆分的字符串')
## 替换
re.sub(r'正则表达式', '替换字符', '被替换字符', 替换个数->int)
```
