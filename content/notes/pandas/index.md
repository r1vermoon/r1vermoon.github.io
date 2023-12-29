---
title: "Pandas"
date: 2023-12-29T13:16:42+08:00
draft: false
---

## Intro

[Getting started](https://pandas.pydata.org/docs/getting_started/intro_tutorials/01_table_oriented.html), [User-guide](https://pandas.pydata.org/docs/user_guide/index.html#user-guide), [API refs](https://pandas.pydata.org/docs/reference/index.html#api).

Install:

```bash
pip install pandas
```

or

```bash
conda install -c conda-forge pandas
```

Import:

```python
import pandas as pd
```

## Reading and Saving Files

See [official guide](https://pandas.pydata.org/docs/getting_started/intro_tutorials/02_read_write.html)

### Reading

Use`pd.read_*()`, which returns a `DataFrame`:

- csv

    ```python
    df = pd.read_csv("xxx.csv")
    ```

- xslx

    ```python
    df = pd.read_xslx("xxx.xslx")
    ```

- parquet:

    ```python
    df = pd.read_parquet("xxx.parquet")
    ```

### Saving

Use `df.to_*()`, where `df` is a `DataFrame`:

{{< admonition type=note title="Efficiency of files" open=true >}}

`parquet` > `csv` > `xslx` by efficiency

{{< /admonition >}}

- parquet:

    ```python
    df.to_parquet("xxxx.parquet")
    ```

- excel:

    ```python
    df.to_excel("xxxx.xlsx", sheet_name="sheet1", index=False)
    ```

## Inspecting

See [official guide](https://pandas.pydata.org/docs/getting_started/intro_tutorials/06_calculate_statistics.html)

- check rows

    ```python
    df
    ```

- check types

    ```python
    df.dtypes
    ```

- check shape

    ```python
    df.shape
    ```

- brief technical summary

    ```python
    df.info()
    ```

- brief statistical summary

    ```python
    df.describe()
    ```

- other statistical summary

    ```python
    df[["Age", "Fare"]].median()
    df[["Age", "Fare"]].sum()
    ...
    ```

- first/last `n` rows:

    ```python
    df.head()
    df.head(8)
    df.tail()
    df.tail(8)
    ```

## Selecting

See [official guide](https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html)

### Columns

- select one column

    ```python
    df_col = df["col name"]
    ```

- select columns

    ```python
    df_cols = df[["col name1", "col name2"]]
    ```

### Rows (filtering)

- filter

    ```python
    df_above_35 = df[df["Age"] > 35]
    ```

    where `df["Age"] > 35` returns an array of `bool`s

- filter within a range

    ```python
    df_class_23 = df[df["Pclass"].isin([2, 3])]
    ```

- combine conditions

    ```python
    df_class_23 = df[(df["Pclass"] == 2) | (df["Pclass"] == 3)]
    ```

{{< admonition type=note title="Note when combining conditions" open=true >}}

- each condition must be surrounded by parentheses `()`
- use `|` and `&` instead of `and` or `or`

{{< /admonition >}}

- non `Null` values

    ```python
    df_age_no_na = df[df["Age"].notna()]
    ```

### Indexing

- `n`th row

    ```python
    df.iloc[3]
    ``````

- rows starts from `n`th

    ```python
    df.iloc[3:6]
    ```

### Rows and columns

- filter

    ```python
    df_adult_names = df.loc[df["Age"] > 35, ["Name", "Sex"]]
    ```

    It shows rows that `Age` > 35 and column `Name` and `Sex`

- indexing

    ```python
    df.iloc[9:25, 2:5]
    ```

    rows 10 till 25 and columns 3 to 5.

## Ploting

See [official documents](https://pandas.pydata.org/docs/getting_started/intro_tutorials/04_plotting.html)

## Creating and Merging

See [Official guide](https://pandas.pydata.org/docs/getting_started/intro_tutorials/05_add_columns.html) and [this](https://pandas.pydata.org/docs/getting_started/intro_tutorials/08_combine_dataframes.html)

- create new column based on another column(s):

    ```python
    df["london_mg_per_cubic"] = df["station_london"] * 1.882
    ```

- concatenate tables

    ```python
    df = pd.concat([df1, df2, df3])
    ```

## Reshaing and Renaming

See [Official guide](https://pandas.pydata.org/docs/getting_started/intro_tutorials/07_reshape_table_layout.html)

- sort/reorder

    ```python
    df = df.sort_values("count")
    ```

- reset index

    ```python
    df = df.reset_index()
    ```

    original index will be added as a new column `index`

- drop column(s)

    ```python
    df = df.drop(by=["col name1", "col name2"], axis=1) 
    ```

- rename

    ```python
    df = df.rename(
        columns={
            "index": "Index",
            "start time": "start date",
        }
    )
    ```

## Grouping

- group by colmun(s):

    ```python
    groups = groups.groupby(["end date", "user id"])
    groups_size = groups.size()
    sums = groups.sums()
    ```
