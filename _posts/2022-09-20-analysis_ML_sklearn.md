---
title:  sklearn-基础使用
layout: default
---
[![返回](/assets/images/back.png)](../../../../)

# Scikit-learn基础使用

## 流水线（Pipeline）

流水线的输入为一连串的数据挖掘步骤，其中最后一步必须是估计器，前几步是转换器。输入的数据集经过转换器的处理后，输出的结果作为下一步的输入。最后，用位于流水线最后一步的估计器对数据进行分类。
每一步都用元组（ ‘名称’，步骤）来表示。现在来创建流水线。

```
scaling_pipeline = Pipeline([
  ('scale', MinMaxScaler()),
  ('predict', KNeighborsClassifier())
])
```

## 预处理

[sklearn-数据清洗](../../../../2022/09/20/Python_SKlearn_Preprocessing.html)

## 特征

### 特征抽取

### 特征选择

## 降维

## 组合

## 模型评估（度量）

### 分类评估

### 回归评估

### 多标签评估

### 聚类评估

## 交叉验证

## 网格搜索

## 多分类、多标签分类