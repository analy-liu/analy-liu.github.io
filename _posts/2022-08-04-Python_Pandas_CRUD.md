---
title:  pandas-增查删改
layout: default
---
[![返回](/assets/images/back.png)](../../../../2022/07/05/Python_Index.html)

为了更加直观的体现数据变化，本文每个函数都将作用在下面生成的示例表

```python
dict_ = {'编号':['A-01','A-02','B-01','B-01'],
         '销量':[36,75,24,34],'单价':[40,50,60,60],
         '日期':["2019-07-01","2021-03-21","2022-07-19","2022-08-20"],
         '客户':['张三','',np.nan,None]}
df = pd.DataFrame(dict_) 
```

||编号|销量|单价|日期|负责人|
|--|--|--|--|--|--|
|0|A-01|36|40|2019-07-01|张三|
|1|A-02|75|50|2021-03-21||
|2|B-01|24|60|2022-07-19|NaN|
|3|B-01|34|60|2022-08-20|None|

# 1. Create 增

## 1.1 增加行
```python
def insert_row(df,content,index):
    # 在插入处拆分为两个表
    df1 = df.iloc[0:index,:]
    df2 = df.iloc[index:,:]
    # 在表1末尾插入内容
    df1.loc[df1.shape[0]] = content
    # 合并表1表2
    df = pd.concat([df1,df2], ignore_index=True, sort=False)
    return df
# 在第2行后插入内容
df = insert_row(df,["C-01", 14, 30, '2021-03-01',"李四"],2)
```

||编号|销量|单价|日期|负责人|
|--|--|--|--|--|--|
|0|A-01|36|40|2019-07-01|张三|
|1|A-02|75|50|2021-03-21||
|2|C-01|14|30|2021-03-01|李四|
|3|B-01|24|60|2022-07-19|NaN|
|4|B-01|34|60|2022-08-20|None|

## 1.2 增加列

**方法一：使用insert函数**
优点：可以插入到任意位置
```python
# 使用函数insert
DataFrame.insert(loc,column,value,allow_duplicates=False)
# 参数(插入位置，新列名，内容，是否允许新增已存在的列)

df.insert(3,"流水",df['销量']*df['单价'])# 插入计算列到第3列后
df.insert(6,"备注",["无","空字符","np.nan","None"])# 插入内容列到第5列后
```

||编号|销量|单价|流水|日期|负责人|备注|
|--|--|--|--|--|--|--|--|
|0|A-01|36|40|1440|2019-07-01|张三|无|
|1|A-02|75|50|3750|2021-03-21||空字符|
|2|B-01|24|60|1440|2022-07-19|NaN|np.nan|
|3|B-01|34|60|2040|2022-08-20|None|Non|

**方法二：直接赋值**
优点：写法简单 缺点：只能添加在最后一列
```python
df['流水'] = df['销量']*df['单价']
df.loc[:,"备注"] = ["无","空字符","np.nan","None"]
# 两种写法均可
```

||编号|销量|单价|日期|负责人|流水|备注|
|--|--|--|--|--|--|--|--|
|0|A-01|36|40|2019-07-01|张三|1440|无|
|1|A-02|75|50|2021-03-21||3750|空字符|
|2|B-01|24|60|2022-07-19|NaN|1440|np.nan|
|3|B-01|34|60|2022-08-20|None|2040|None|

## 1.3 数据拆分

**方法一：使用split，使用split需要用str将数据集转化为字符串**  
Series.str.split(sep,n,expand=false)从前往后切分  
Series.str.rsplit(sep,n,expand=false)从后往前切分  
sep：用于分割的字符，n：分割次数，expand：True输出Dataframe，False输出Series。
```python
df.loc[:,"编号字母"] = df.loc[:,"编号"].str.split('-',n = 1, expand=True)[0]
df.loc[:,"编号数字"] = df.loc[:,"编号"].str.split('-',n = 1, expand=True)[1]
```

||编号|销量|单价|日期|负责人|编号字母|编号数字|
|--|--|--|--|--|--|--|--|
|0|A-01|36|40|2019-07-01|张三|A|01|
|1|A-02|75|50|2021-03-21||A|02|
|2|B-01|24|60|2022-07-19|NaN|B|01|
|3|B-01|34|60|2022-08-20|None|B|01|

**方法二：使用extract，正则表达式**  
Series.str.extract(pat, flags=0, expand=True)  
pat:具有捕获组的正则表达式模式  
flags:int，默认值为0(无标志)  
expand：True输出Dataframe，False输出Series。

```python
df.loc[:,"年份"] = df.loc[:,"日期"].str.extract('(^\d{4})', expand=False).astype("int")
df.loc[:,"编号字母"] = df.loc[:,"编号"].str.extract('(^.*(?=-))', expand=False)
```

||编号|销量|单价|日期|负责人|年份|编号字母|
|--|--|--|--|--|--|--|--|
|0|A-01|36|40|2019-07-01|张三|2019|A|
|1|A-02|75|50|2021-03-21||2021|A|
|2|B-01|24|60|2022-07-19|NaN|2022|B|
|3|B-01|34|60|2022-08-20|None|2022|B|

更多正则表达式相关知识请看[python-字符串与正则表达式](../../../../2022/08/05/Python_Re_StrAndReg.html)

## 1.4 数据合并

DataFrame合并主要使用两种方法，merge与concat  

merge以指定字段为连接索引，提供了类似于SQL数据库连接操作的功能，支持左联、右联、内联和外联等全部四种SQL连接操作类型  

concat以轴为连接索引，常用于合并结构相同的新旧表

**merge**  

DataFrame.merge(right, how='inner', on=None, left_on=None, right_on=None, 
left_index=False, right_index=False, sort=False, suffixes=('_x', '_y'), 
copy=True, indicator=False, validate=None)

|参数|说明|
|--|--|
|left|参与合并的左侧DataFrame|
|right|参与合并的右侧DataFrame|
|how|连接方式：‘inner’（默认）；还有，‘outer’、‘left’、‘right’|
|on|用于连接的列名，必须同时存在于左右两个DataFrame对象中，如果未指定，则以left和right列名的交集作为连接键|
|left_on|左侧DataFarme中用作连接键的列|
|right_on|右侧DataFarme中用作连接键的列|
|left_index|将左侧的行索引用作其连接键|
|right_index|将右侧的行索引用作其连接键|
|sort|根据连接键对合并后的数据进行排序，默认为True。有时在处理大数据集时，禁用该选项可获得更好的性能|
|suffixes|字符串值元组，用于追加到重叠列名的末尾，默认为（‘x’,‘y’）.例如，左右两个DataFrame对象都有‘data’，则结果中就会出现‘data_x’，‘data_y’|
|copy|设置为False，可以在某些特殊情况下避免将数据复制到结果数据结构中。默认总是赋值|

```python
# 写法一
df = df1.merge(df2,on = ['key1', 'key2'],how='outer')
# 写法二
df = pd.merge(df1,df2,on = 'key1',how='left')
```

**concat**  

pandas.concat(objs, axis=0, join='outer', ignore_index=False, keys=None, 
levels=None, names=None, verify_integrity=False, sort=False, copy=True)

|参数|默认|说明|
|:-:|:-:|--|
|objs|必填|参与连接的列表或字典，且列表或字典里的对象是pandas数据类型，唯一必须给定的参数|
|axis=0|0|指明连接的轴向，0是纵轴，1是横轴|
|join|‘outer’|‘inner’（交集），‘outer’（并集），指明轴向索引的索引是交集还是并集|
|ignore_index|False|不保留连接轴上的索引，产生一组新索引range（total_length）|
|keys|None|与连接对象有关的值，用于形成连接轴向上的层次化索引（外层索引），可以是任意值的列表或数组、元组数据、数组列表（如果将levels设置成多级数组的话）|
|levels|None|指定用作层次化索引各级别（内层索引）上的索引，如果设置keys的话|
|names|None|用于创建分层级别的名称，如果设置keys或levels的话|
|verify_integrity|False|检查结果对象新轴上的重复情况，如果发横则引发异常，允许重复|
|sort|False|根据连接键对合并后的数据进行排序。有时在处理大数据集时，禁用该选项可获得更好的性能|
|copy|True|默认True时是深拷贝，False时为浅拷贝|

```python
# 行拼接
pd.concat([df1,df2], ignore_index=True, sort=False)
# 列拼接
pd.concat([df1,df2], axis=1, sort=False)
```

## 1.5 函数apply使用

apply函数主要功能是生成条件列，其极高的自由度能实现几乎所有条件的批量数据处理，堪称数据处理大杀器。

**apply参数**

```python
DataFrame.apply(
    func: 'AggFuncType', # 应用到每行或列的函数，可以是lambda匿名函数或自定义函数
    axis: 'Axis' = 0, # 对每一行或列数据应用函数。列：0 or index；行：1 or columns
    raw: 'bool' = False, # 控制数据是以Series还是ndarray传递，False：Series；True：ndarray
    result_type=None, # axis=1时起作用，{'expand'：返回列, 'reduce'：返回Series, 'broadcast'：结果将被广播到 DataFrame 的原始形状, None：类似列表的结果将作为这些结果的 Series 返回。但是，如果应用函数返回一个 Series ，这些结果将被扩展为列}
    args=(), # 以元组传递额外参数
    **kwargs # 以字典传递额外参数)
```

**apply应用**

**示例一：** 根据每行情况，新增条件列

新增列，日期为今年显示今年，往年显示往年
```python
df['日期'] = pd.to_datetime(df['日期']) # 将文本格式日期转为日期格式
## 写法一：对单行进行处理，不用声明axis=1
df['是否今年'] = df['日期'].apply(lambda x: '今年' if x>datetime.datetime(datetime.datetime.today().year,1,1) else '往年')
## 写法二：对整个DataFrame处理，x.列名可表示单独列，需要加上axis=1
df['是否今年'] = df.apply(lambda x: '今年' if x.日期>datetime.datetime(datetime.datetime.today().year,1,1) else '往年',axis=1)
## 写法三：构造自定义函数，并调用
def YNyear(date):
    if date>datetime.datetime(datetime.datetime.today().year,1,1):
        return "今年"
    else:
        return "往年"
df['是否今年'] = df.apply(lambda x: YNyear(x.日期),axis=1)
```

||编号|销量|单价|日期|负责人|是否今年|
|--|--|--|--|--|--|--|
|0|A-01|36|40|2019-07-01|张三|往年|
|1|A-02|75|50|2021-03-21||往年|
|2|B-01|24|60|2022-07-19|NaN|今年|
|3|B-01|34|60|2022-08-20|None|今年|

**示例二：** 根据每列情况，计算数值

列出销量与单价的最大最小值

```python
## 写法一：匿名函数
df.loc[:,['销量','单价']].apply(lambda x: pd.Series([x.min(),x.max()],index=['min','max']),axis=0)
## 写法二：自定义函数
def minmax(x):
    return pd.Series([x.min(),x.max()],index=['min','max'])
df.loc[:,['销量','单价']].apply(minmax,axis=0) #axis=0可以不写，默认就是0
```

||销量|单价|
|--|--|--|
|min|24|40|
|max|75|60|

**示例三：** 自定义函数与匿名函数（含args与**kwargs使用）

新增列，判断流水是否大于均值，并标出最低流水，流水=销量*单价

```python
# 方法一： 匿名函数 优点：不用命名函数，缺点：条件多时不直观
df['流水等级'] = df.apply(lambda x: "低于均值,最低流水：{}".format(x.销量*x.单价) if x.销量*x.单价 == min(df['销量']*df['单价']) else "高于均值" if x.销量*x.单价>(df['销量']*df['单价']).mean() else "小于等于均值",axis=1)
# 方法二：自定义函数
def flow_level(x,v_min,v_mean):
    flow = x['销量']*x['单价']
    if flow == v_min:
        return "低于均值,最低流水：{}".format(flow)
    elif flow > v_mean:
        return "高于均值"
    else:
        return "小于等于均值"
## 使用args传递参数
df['流水等级'] = df.apply(flow_level,axis=1,args=(min(df['销量']*df['单价']),(df['销量']*df['单价']).mean(),))
## 使用**kwargs传递参数
df['流水等级'] = df.apply(flow_level,axis=1,v_min=min(df['销量']*df['单价']),v_mean=(df['销量']*df['单价']).mean())
```

||编号|销量|单价|日期|负责人|流水等级|
|--|--|--|--|--|--|--|
|0|A-01|36|40|2019-07-01|张三|低于均值,最低流水：1440|
|1|A-02|75|50|2021-03-21||高于均值|
|2|B-01|24|60|2022-07-19|NaN|低于均值,最低流水：1440|
|3|B-01|34|60|2022-08-20|None|小于等于均值|

## 1.6 函数实现-vlookup



# 2. Read 查

## 2.1 切片

# 3. Delete删

## 3.1 删除行

## 3.2 删除列

## 3.3 数据去重

# 4. Update 改

## 4.1 行列顺序调整

## 4.2 行列重命名

## 4.3 空值处理

## 4.4 日期时间格式处理