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

- 矩阵包含同一类型的数值数据，并使用线性代数进行操作。