PPT: [Lec 02 - DS100 Sp24 - Pandas I - Google 幻灯片](https://docs.google.com/presentation/d/1-89bVbLcggrPuTfYAt6OveeLqvw_0UH51ULKBFBulHg/edit#slide=id.SLIDES_API2090287008_0)

Notes: [Principles and Techniques of Data Science - 2 Pandas I (ds100.org)](https://ds100.org/course-notes/pandas_1/pandas_1.html)

学习内容：

- Tabular data
- Series, DataFrames, and Indices

- Data extraction with loc, iloc, and []

# Tabular data

表格数据，形如下面的数据：

![row_col.png (801×499) (ds100.org)](https://ds100.org/course-notes/pandas_1/images/row_col.png)

- **<span style="color:blue">一行</span>代表一个观察结果**（在这里，一个人在某一年竞选总统）。
- **<span style="color:orange">一列</span>表示该观察的某些特征或特征**（这里是该人的政党）。

# 介绍`pandas`

`pandas`是`Python`的数据分析库，表示"**pan**el **da**ta"。

`pandas` 支持数据的格式化、条件筛选、运算和矢量化操作等功能，帮助数据科学家快速获取数据见解。

导入库：

```python
import pandas as pd
```



> 使用的数据集：[sp24-student/lecture/lec02/data/elections.csv at main · DS-100/sp24-student (github.com)](https://github.com/DS-100/sp24-student/blob/main/lecture/lec02/data/elections.csv)

# Series, DataFrames, and Indices

