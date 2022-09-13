---
title:  python目录
layout: default
---
[![返回](/assets/images/back.png)](../../../../)

# python数据分析

## 1. 数据获取

数据有多种获取途径，下面是常用的几种获取数据的方式，最终数据以DataFrame格式保存

- [x] [pandas-数据导入与导出](../../../../2022/07/04/Python_Pandas_DataIO.html)
- [x] [requests-web常规爬虫](../../../../2022/07/25/Python_Requests.html)
- [x] [lxml-html文件内容处理](../../../../2022/07/25/Python_Lxml.html)
- [x] [seleniumwire-web模拟用户爬虫](../../../../2022/07/25/Python_Seleniumwire.html)
- [x] [内置函数-本地数据读取与保存](../../../../2022/08/01/Python_Built-in-functions_LocalDataIO.html)
- [x] [函数实现-腾讯文档爬虫](../../../../2022/08/02/Python_Self-build-functions_DocqqAPI.html)

## 2. 数据清洗与特征工程

数据清洗主要使用pandas进行增查删改的操作，有时也需要对文本进行清洗和提取

- [x] [pandas-增查删改](../../../../2022/08/04/Python_Pandas_CRUD.html)
- [x] [python-字符串与正则表达式](../../../../2022/08/05/Python_Re_StrAndReg.html)
- [x] [函数实现-vlookup](../../../../2022/08/06/Python_Self-build-functions_Nbvlookup.html)

**增查删改内容目录**

1. 增
   - 增加行、列
   - 数据拆分
   - 数据合并-merge、concat
   - 大杀器apply
   - 函数实现-vlookup
2. 查
   - 切片loc、iloc、query
3. 删
   - 删除行、列
   - 数据去重duplicates
4. 改
   - 行与列排序与重命名
   - 数据类型转换
   - 空值处理
   - 值修改

**pd.set_option()：显示多行多列 调整精度**  

```
# 显示所有列
pd.set_option('display.max_columns', None)
pd.set_option('display.max_columns', 5)  #最多显示5列
# 显示所有行
pd.set_option('display.max_rows', None)
pd.set_option('display.max_rows', 10)#最多显示10行
#显示小数位数
pd.set_option('display.float_format',lambda x: '%.2f'%x) #两位
#显示宽度
pd.set_option('display.width', 100)
#
import warnings
warnings.filterwarnings('ignore')  # 关闭运行时的警告
np.set_printoptions(linewidth=100, suppress=True)   # 打印numpy时设置显示宽度，并且不用科学计数法显示
pd.set_option('display.width', 100)   # pandas设置显示宽度
pd.set_option('precision', 1)   # 设置显示数值的精度
```

## 3. 数据透视与描述性分析

在数据透视方面，excel的功能比python完善，如果条件允许，数据透视在excel里进行更方便  
但有时候需要在python里完成透视工作，所有完善了一些基础透视功能

- [x] [函数实现-pdpivot：分级汇总与排序](../../../../2022/08/10/Python_Self-build-functions_Pdpivot.html)

## 4. 数据可视化

- [x] [python-数据可视化](../../../../2022/08/15/Python_Matplotlib.html)
- [x] [函数实现-pyecharts简化函数](../../../../2022/08/15/Python_Pyecharts.html)

## 5. 建模分析

1. 分类
   - [ ] [XGBoost]
2. 回归
   - [ ] [线性回归]
3. 聚类
   - [ ] [K均值聚类]
4. 降维
   - [ ] [PAC]
5. 时间序列
时间序列通常不用python做

## 6. 未归类

- [x] [pandas使用手册-总](../../../../2022/06/02/Python_Pandas_Note.html)
- [x] [python内置函数整理](../../../../2022/08/03/Python-Built-in-functions_Note.html)