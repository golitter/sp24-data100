PPT：[Lec 03 - DS100 Sp24 - Pandas II - Google Slides](https://docs.google.com/presentation/d/1D5temlrJ5sDRgTDO62bkAcju20qrKgUbARlajf_PIpM/edit#slide=id.SLIDES_API1626967423_0)

Notes：[Principles and Techniques of Data Science - 3 Pandas II (ds100.org)](https://ds100.org/course-notes/pandas_2/pandas_2.html)

## 上下文提取 - `[]`

- loc：通过标签进行选择。第一个参数是行，第二个参数是列
- iloc：通过数值进行选择。第一个参数是行，第二个参数是列

而`[]`只有一个参数：

- 切片数值

  ```python
  elections[2:5] # 2-4行
  ```

- 列列表

  ```python
  elections[ ['Year', 'Party']]
  ```

- 列单值

  ```python
  elections['Year'] # Series 类型
  ```

## 条件选择

> 在 `.loc` 里传递布尔列表的作用是基于布尔条件过滤行，而不是按列标签选择特定列。

在`pandas`中：

- `.loc[]`：**左闭右闭**，用于标签索引。

- `.iloc[]`：左闭右开，用于整数位置索引。

```python
# 获取前十的行数据
babynames_first_10_rows = babynames.loc[:9, :]
# 基于布条件进行过滤
babynames_first_10_rows[[True, False, True, False, True, False, True, False, True, False]]
```

**布尔数组**可以通过Series类型的逻辑操作得到。

```python
# 
logical_operator = (babynames['Sex'] == 'F')

babynames[logical_operator]
# 等价于
babynames.loc[logical_operator, :]
# 等价于
babynames[babynames['Sex'] == 'F']
# 等价于
babynames.loc[babynames['Sex'] == 'F', :]
```

Series类型可以使用逻辑操作符，`~ ^| & > < >= ...`。

> 在 `pandas` 中进行布尔条件组合时，需要用括号将每个条件包裹起来。

```python
# 性别为F，或出生年份小于2000
babynames[ (babynames['Sex'] == 'F') | (babynames['Year'] < 2000)]
```



```python
# 过滤出 Count > 250的数据，之后查看前三行
babynames.loc[ babynames['Count'] > 250, ['Name', 'Count']].head(3)
```

```python
# 查看前两行的所有标签信息
babynames.loc[babynames['Count'] > 250, ['Name', 'Count']].iloc[0:2,:]
```



当有很多逻辑式要过滤时，非常繁琐：

```python
babynames[ (babynames['Name'] == 'Bella') |
          (babynames['Name'] == 'Alex') |
          (babynames['Name'] == 'Narges') |
          (babynames['Name'] == 'Lisa')
          ]
```



`pandas`提供许多可供选择的：

- `.isin`

  ```python
  # 等价于上式
  names = ['Bella', 'Alex', 'Narges', 'Lisa']
  babynames[babynames['Name'].isin(names)]
  ```

  

- `.str.startswith`

  ```python
  # Name以N开头
  babynames[babynames['Name'].str.startswith('N')]
  ```

  

- `.groupby.filter`

## 增删改

### 增

#### 增加一列

1. **使用 `[]` 引用新列**：通过 `DataFrame['新列名']` 方式来引用新列。

2. **赋值**：将这列赋值为一个适当长度的 `Series` 或 `数组`，即与现有行数相同的长度。

```python
babyname_len = babynames['Name'].str.len()
babynames['babyname_len'] = babyname_len
babynames
```

### 改列名

使用`rename()`进行改名：

```python
babynames = babynames.rename(columns={'babyname_len':'Length'})
babynames
```

### 删除列

```python
babynames.drop('Length', axis= 'columns')
```

> 默认是创建副本，使用`inplace=True`才是在原来基础上删除。

## 内置函数

- `shape`：查看数据框的形状，以 `(行数, 列数)` 的元组形式显示。

- `size`：获取数据框中元素的总数（行数 × 列数）。

- `describe()`：生成数据框的统计摘要信息，包括计数、均值、标准差、最小值、四分位数和最大值。

- `sample()`：随机抽取数据框中的样本行，可以指定抽样数量，以便进行数据探索或抽样分析。

- `unique()`：获取指定列的唯一值列表，用于查看数据的去重值。

  ```python
  babynames['Name'].unique()
  ```

  

- `sort_values()`：按指定列排序数据，可以升序或降序排列，常用于数据整理。

  ```python
  # 按名字进行排序，降序
  babynames.sort_values(by='Name', ascending=False)
  ```

  

- `value_counts()`：统计每个唯一值的出现次数，常用于分析分类数据的分布情况。

  ```python
  babynames['Name'].value_counts()
  ```

  