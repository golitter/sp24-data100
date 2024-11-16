PPT：[Lec 05 - DS100 Sp24 - Data Cleaning and EDA - Google Slides](https://docs.google.com/presentation/d/10hMVxwNOFpC5kjZ18xVB1aHR_MnNLf6OmRFfYAZBKNY/edit#slide=id.SLIDES_API1479587541_0)

Notes：[Principles and Techniques of Data Science - 5 Data Cleaning and EDA (ds100.org)](https://ds100.org/course-notes/eda/eda.html)

数据清理，也称为数据整理，是对原始数据进行转换以方便后续分析的过程。它通常用于解决以下问题：结构或格式不清晰，丢失或损坏的值，单位转换...

**探索性数据分析（EDA）**是了解新数据集的过程。这是一种开放式、非正式的分析，涉及熟悉数据集中的变量、发现潜在的假设并识别数据中的潜在问题。最后一点通常会激励进一步的数据清洗以解决数据格式中的任何问题；因此，EDA和数据清洗通常被认为是一个“无限循环”，每个过程都驱动着另一个过程。

EDA中的关键属性：

**Structure** -- the “shape” of a data file
**Granularity** -- how fine/coarse is each datum
**Scope** -- how (in)complete is the data
**Temporality** -- how is the data situated in time
**Faithfulness** -- how well does the data capture “reality”

#  Structure

通常更倾向于使用矩形数据进行数据分析。矩形结构的数据易于操作和分析。数据清洗的一个重要方面就是将数据转换为更接近矩形的结构。矩形数据有两种类型：**表格和矩阵**。

- 表格包含具有不同数据类型的命名列，并使用数据转换语言进行操作。
- `csv`：记录行用换行符，字段用逗号分隔。
- 矩阵包含同一类型的数值数据，并使用线性代数进行操作。



希望表头不是第零行：

```python
tb_df = pd.read_csv('./data/cdc_tuberculosis.csv', header=1)
tb_df.head()
```

重命名列：

```python
rename_dict = {'2019': 'TB cases 2019',
               '2020': 'TB cases 2020',
               '2021': 'TB cases 2021',
               '2019.1': 'TB incidence 2019',
               '2020.1': 'TB incidence 2020',
               '2021.1': 'TB incidence 2021'}
tb_df = tb_df.rename(columns=rename_dict)
```

# **Granularity**

```python
tb_df.drop(0)
tb_df.drop(0).sum()
```

发现，本应该是数值类型，但是是字符串类型。

```python
tb_df.dtypes
```

> 原因，`csv`文件中的total行内的数值类型带引号，所以哪些行被识别为字符串类型。

`thousands` 参数的作用是指定分隔数字中千位分隔符的符号。例如，如果数字是用逗号（`,`）分隔千位的（如 `1,000`），通过指定 `thousands=','`，`pandas` 可以正确地解析这些数字为整数或浮点数。

```python
tb_df = (
    pd.read_csv('./data/cdc_tuberculosis.csv',
        header = 1,
        thousands = ',',
    )
.rename(columns=rename_dict)
)

tb_df

```

> 建议使用链式范式实现，更加直观
>
> 声明变量时，不能换行，但是可以用`()`，这样就可以进行换行。
>
> Python 允许在 **小括号**、**中括号**、**大括号**内的代码块自动换行，而无需显式使用反斜杠 (`\`)。

