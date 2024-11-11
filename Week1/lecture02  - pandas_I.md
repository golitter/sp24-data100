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

在`pandas`中有三种基本的数据结构：

`Series`：1D带标签数组数据；最好将其视为列数据。

`DataFrame`：具有行和列的2D表格数据。

`Index`：行/列标签的序列。

> `DataFrame` - **表**；`Series` - **列**。

![df_elections.png (1292×650) (ds100.org)](https://ds100.org/course-notes/pandas_1/images/df_elections.png)

> **直接输入 `elections`**：Jupyter Notebook 会自动渲染 DataFrame，并以表格形式美观地显示出来，包括样式、列宽调整等。这种显示方式由 Notebook 环境优化。
>
> **使用 `print(elections)`**：`print()` 输出 DataFrame 时，会以纯文本格式显示，没有优化的样式。列对齐、行限制等方面可能也会有所不同，看起来不如 Jupyter Notebook 渲染的表格美观。

**慎用`inplace=True`**，使用时需要时刻注意。

> 当使用`inplace=True`时，设置索引列后，再次使用`inplace=True`恢复默认列，此时与最开始相同。但是当再次使用`inplace=True`时，默认索引列`index`将不再是默认列，转而是`level_0`...
>
> 没有确切把握不要使用`inplace=True`。

# Data extraction with loc, iloc, and []

- loc：基于标签：[number, string]
- iloc：基于基数：[number, number]