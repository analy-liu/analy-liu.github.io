---
title:  内置函数-本地数据读取与保存
layout: default
---
[![返回](/assets/images/back.png)](../../../../2022/07/05/Python_Index.html)

# Python 文件I/O

## Open使用

```python
# 简易模式
f = open("test.txt") # 在当前文件夹打开文件
f = open(r"F:\data\test\English_Path\test.txt") # 指定完整路径,默认只读模式
f.close() # 关闭文件

# try-finally形式
try:
    f = open(r"F:\data\test\English_Path\test.txt", mode='r',encoding = 'utf-8')
    # mode指定文件打开模式 encoding指定文件编码
finally:
    # 即使引发了导致程序流停止的异常，也可以保证文件正确关闭
    f.close()

# with形式 能保证在退出 with 语句中的块时关闭文件
with open(r"F:\data\test\English_Path\test.txt", mode='w',encoding = 'utf-8') as f:
    f.write("第一行\n") # \n换行
    f.write("第二行")
    f.write("第二行\n")
    # 会覆盖原内容,无文件会新建
```

## Open参数

**open(file, encoding=None, mode='r', buffering=-1, errors=None, newline=None, closefd=True, opener=None)**

- **file 文件路径或文件描述符（必填）** 

- **encoding 编码模式 str类型**
  在mode参数包含t时不可指定，即仅文本模式可用
  None：默认值，windows下默认gbk
  常用：utf-8、ascii、gbk

- **mode 操作模式 str类型**
  基础模式：r w a x
  在基础模式上，可选择打开模式与是否升级同时读写，例如：rb r+b

|Mode 模式|描述|
|--|--|
|r |基础模式，只读不写，默认值，无文件报错|
|w |基础模式，覆盖写入，无文件则创建文件|
|a |基础模式，追加写入，无文件则创建文件|
|x |基础模式，新建写入，有文件则报错|
| t|打开形式，以文本模式打开，默认值|
| b|打开形式，以二进制模式打开|
| +|升级模式，使得基础模式能同时读写|

<details>
<summary>点击展开查看：非常用参数</summary>
<p>

- **buffering 缓冲设置 [-1,0,1]**
  -1：默认值，使用系统默认缓冲机制
  0:不使用缓冲，直接读写磁盘
  1:单行缓冲

- **errors 编解码报错的处理模式 str类型**
  在mode参数包含t时不可指定，即仅文本模式可用
  常用模式：
  strict：编解码错误则报错
  ignore：编解码出现错误会忽略，不报错
  replace：编解码出现错误不会报错，会用“?”替代要写入或读取的无法解析的数据

- **newline 换行符设置，str类型**
  None（默认）、"\r"、"\n"、"\r\n"

- **closefd 控制file参数的传入值类型 bool类型**
  True：默认，file参数可以是表示文件路径的字符串，也可以是文件描述符
  False：file参数只能是文件描述符，传入字符串会报错。

- **opener** 
  传递一个可调用的 opener 来使用自定义 opener
  ```python
  import os
  dir_fd = os.open('somedir', os.O_RDONLY)
  def opener(path, flags):
      return os.open(path, flags, dir_fd=dir_fd)

  with open('spamspam.txt', 'w', opener=opener) as f:
      print('This will be written to somedir/spamspam.txt', file=f)

  os.close(dir_fd)  # 不要泄漏文件描述符
  ```
  参考链接：
  https://stackoverflow.com/questions/37241711/what-is-the-use-of-opener-argument-in-built-in-open-function
</p>
</details>

## [其他python内置函数](../../../../2022/08/03/Python-Built-in-functions_Note.html)