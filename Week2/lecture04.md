PPT：[Lec 04 - DS100 Sp24 - Pandas III - Google Slides](https://docs.google.com/presentation/d/1N_yRYvYeerJjVjMXez7Vc0qO7B2xRbY-_k2zZnzWD30/edit#slide=id.SLIDES_API2072879288_0)

Notes：[Principles and Techniques of Data Science - 4 Pandas III (ds100.org)](https://ds100.org/course-notes/pandas_3/pandas_3.html)

- `sort_values()` - `key`参数

  ```python
  babynames.sort_values(by = 'Name', key=lambda x: x.str.len(), ascending=False)
  ```

- `map`

  ```python
  def dr_ea_count(s:str) -> int:
      return s.count('ea') + s.count('dr')
  
  babynames['ea_dr_count'] = babynames['Name'].map(dr_ea_count)
  babynames = babynames.sort_values(by = 'ea_dr_count', ascending=False)
  babynames.head()
  ```

### `group`

目标是将属于同一类别的行分组，并执行可以跨越该类别所有行的聚合操作。

- 将属于同一类别的行组合在一起。
- 执行对类别中的所有行进行聚合的操作。

> 分组是一个强大的工具，可以：
>
> 1)一次执行大型操作，
>
> 2)总结数据集中的趋势。

![gb.png (1630×704) (ds100.org)](https://ds100.org/course-notes/pandas_3/images/gb.png)

```python
baby_by_year = babynames[['Year', 'Count']].groupby('Year').agg(sum)
baby_by_year
```

![agg.png (1646×542) (ds100.org)](https://ds100.org/course-notes/pandas_3/images/agg.png)

```python
babynames.groupby('Year')[['Count']].agg(sum)
```

可以将许多不同的聚合应用于分组数据。要求聚合函数必须：

- 接收一系列数据（分组子框架的一列）。

- 返回一个汇总该系列的值。

内置的Python操作-例如`sum`， `max`和`min` 会被pandas自动识别。

- 内置Python：`agg(sum)`
- Numpy：`agg(np.sum)`
- 内置pandas：`agg('sum')`

```python
# 相对受欢迎度
def ratio_to_peak(series):
    return series.iloc[-1] / np.max(series)
# # 按 Name 分组，并对每个分组应用 ratio_to_peak
rtp_table = f_babynames.groupby("Name")[['Year', 'Count']].agg(ratio_to_peak)
rtp_table.head()
```

> Year也被聚合，与原意不同。

```python
# 删除 Year 列
rtp_table.drop('Year', axis='columns', inplace=True)
# 按 Count 降序排列
rtp_table.sort_values(by='Count', ascending=False).head()
```

```python
# 重命名 Count 列
rtp_table = rtp_table.rename(columns = {"Count": "Count RTP"})
rtp_table
```



**`.size`**：生成一个 `Series`，表示每个分组中的总条目数（包括缺失值）。

**`.count`**：生成一个 `DataFrame`，表示每个分组中非缺失值的条目数。

```python


# 创建一个示例 DataFrame
df = pd.DataFrame({
    'Category': ['A', 'A', 'B', 'B', 'C', 'C', 'C'],
    'Value': [10, np.nan, 20, 25, 30, np.nan, 35]
})

# 使用 .size 方法，返回每个分组的总条目数
size_counts = df.groupby('Category').size()
print("Using .size:\n", size_counts)

# 使用 .count 方法，返回每个分组中非缺失值的条目数
count_counts = df.groupby('Category').count()
print("\nUsing .count:\n", count_counts)
```

### `fliter`

`groupby.filter` 接受一个参数 `func`，其中 `func` 是一个函数，它：

- 以 `DataFrame` 对象为输入；
- 返回一个单一的布尔值（`True` 或 `False`）。

`groupby.filter` 会将 `func` 应用于每个分组（子 `DataFrame`）：

- 如果 `func` 对某个分组返回 `True`，则该组的所有行会被保留。
- 如果 `func` 对某个分组返回 `False`，则该组的所有行会被过滤掉。

换句话说，`func` 返回 `True` 的子 `DataFrame` 会出现在最终结果中，而返回 `False` 的子 `DataFrame` 会被排除在外。重要的是，`groupby.filter` 与 `groupby.agg` 不同，后者只返回单行数据，而 `groupby.filter` 返回的是整个子 `DataFrame`，并且会保留原来的索引，而我们分组的列不会成为索引！

![filter_demo.png (1112×694) (ds100.org)](https://ds100.org/course-notes/pandas_3/images/filter_demo.png)

```python
# 筛选出在某些年份中，投票百分比最高的候选人得票率低于 45%的年份数据。
elections.groupby("Year").filter(lambda sf: sf["%"].max() < 45).head(9)
```

```python
elections.groupby("Year").filter(lambda sf: sf["%"].max() < 45).head(9)
elections_sorted_by_percent.groupby("Party").agg(lambda x : x.iloc[0]).head(10)

# Equivalent to the below code
# elections_sorted_by_percent.groupby("Party").agg('first').head(10)
```

![puzzle_demo.png (961×508) (ds100.org)](https://ds100.org/course-notes/pandas_3/images/puzzle_demo.png)

### 透视表

要使用 `.groupby` 对多个列进行分组，只需要将列名列表传递给 `.groupby`。

```python
babynames.groupby(["Year", "Sex"])[["Count"]].agg(sum).head(6)
```



![pivot.png (1084×593) (ds100.org)](https://ds100.org/course-notes/pandas_3/images/pivot.png)

```python
import numpy as np
babynames.pivot_table(
    index = "Year",
    columns = "Sex",    
    values = "Count", 
    aggfunc = "sum", 
).head(5)
```

### 合并表格

在数据科学项目中，通常不会拥有所有所需的数据都在一个单一的 DataFrame 中。实际情况是，数据往往来自多个不同的来源。如果我们有多个相关的数据集，可以使用 **合并**（`merge`）将两个或多个表格合并成一个 DataFrame。

```python
# elections 数据集中提取候选人的 第一个名字。
elections["First Name"] = elections["Candidate"].str.split().str[0]
elections.head(5)
# 过滤出2022年的 babynames 数据
babynames_2022 = babynames[babynames["Year"]==2022]
babynames_2022.head()
# 合并 elections 和 babynames_2022 数据集
merged = pd.merge(left = elections, right = babynames_2022, \
                  left_on = "First Name", right_on = "Name")
merged.head()
```

