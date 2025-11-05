---
title: "Pandas: Filter Rows based on cell values"
date: 2023-09-26
author: Sarfraaz Ahmed
category: Tutorial
---

One of the coolest tricks in Python when you want to search multiple conditions
is the clever use of `in` operator

Let's say you want to check if the user agrees with your proposal or not. May
be you ask for the user's opinion ( or may be read it from the frontend or a
file ) and write some code like this


```python
>>> answers = [ "Yes", "Yeah", "Nods"]
>>> opinion = input("Will you marry me? ")
Will you marry me? Nods
>>>
>>> print("Hurray !!!" if opinion in answers else "Try again later ...")
Hurray !!!
>>>
```

All that is good with simple strings or integers but how can we do something
similar in a Pandas DataFrame?

## Problem Statement

Consider this dataset that contains details about when a particular student
submitted their exercises in my Course

The values in the table contain the date a particular student submitted any
given exercise


```python
>>> import pandas as pd
>>> df = pd.read_csv('exercises_table.csv')
>>> df.set_index('Name', inplace=True)
>>> df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Ex01_Date</th>
      <th>Ex02_Date</th>
      <th>Ex03_Date</th>
      <th>Ex04_Date</th>
    </tr>
    <tr>
      <th>Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Ajay</th>
      <td>12-Jan-2023</td>
      <td>14-Jan-2023</td>
      <td>18-Jan-2023</td>
      <td>16-Jan-2023</td>
    </tr>
    <tr>
      <th>Chetan</th>
      <td>17-Jan-2023</td>
      <td>18-Jan-2023</td>
      <td>19-Jan-2023</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Girish</th>
      <td>17-Jan-2023</td>
      <td>NaN</td>
      <td>15-Jan-2023</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Mohan</th>
      <td>19-Jan-2023</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Vishal</th>
      <td>14-Jan-2023</td>
      <td>15-Jan-2023</td>
      <td>17-Jan-2023</td>
      <td>18-Jan-2023</td>
    </tr>
  </tbody>
</table>
</div>


From the above information, how can we find out which students submitted which
exercises on 18-Jan-2023 and 19-Jan-2023 ?


### Fixing the Row index name

Before we jump to the solution let's first fix the Row index name of the
DataFrame. One neat trick ( more of a hack though ) to remove the name for the
Row index called as "Name" is to set it's name attribute to None

```python
>>> df.index.name = None
>>> df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Ex01_Date</th>
      <th>Ex02_Date</th>
      <th>Ex03_Date</th>
      <th>Ex04_Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Ajay</th>
      <td>12-Jan-2023</td>
      <td>14-Jan-2023</td>
      <td>18-Jan-2023</td>
      <td>16-Jan-2023</td>
    </tr>
    <tr>
      <th>Chetan</th>
      <td>17-Jan-2023</td>
      <td>18-Jan-2023</td>
      <td>19-Jan-2023</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Girish</th>
      <td>17-Jan-2023</td>
      <td>NaN</td>
      <td>15-Jan-2023</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Mohan</th>
      <td>19-Jan-2023</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Vishal</th>
      <td>14-Jan-2023</td>
      <td>15-Jan-2023</td>
      <td>17-Jan-2023</td>
      <td>18-Jan-2023</td>
    </tr>
  </tbody>
</table>
</div>

Now that the table is in a better shape, let's move on to the solution


## Solution

Let's start by defining the required dates in a list `imp_dates`, similar to
the `answers` variable we used earlier


```python
>>> imp_dates = ['18-Jan-2023', '19-Jan-2023']
```

Now, we need to check which of the values in `df` contain any of the dates
present in `imp_dates`. This can be achieved using `isin` method of DataFrame.
This does an elementwise comparison as was done in the earlier Python code at
the beginning of this post. And all elementwise operations can be run
parallelly, so this should be a fast operation.

The `isin` method returns a boolean DataFrame with cells that contain any of
our values of interest ( any date present in `imp_dates` ) having `True` and
the rest as `False`


```python
>>> df.isin(imp_dates)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Ex01_Date</th>
      <th>Ex02_Date</th>
      <th>Ex03_Date</th>
      <th>Ex04_Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Ajay</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>Chetan</th>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>Girish</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>Mohan</th>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>Vishal</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>


Now, to filter the rows that contain our dates of interest, we use the values
present in the Boolean DataFrame as a filter over the DataFrame. 

But, the filter needs to be a Series object which contains the row labels
containing our dates of interest.

In the above boolean DataFrame, the row lables that contain True would be
"Ajay", "Chetan", "Mohan" and "Vishal"

To get the required Series, we can sum up the values along each row.

Any row that contains atleast one True value would sum up into a value greater
than 0

Or, in other words, if a row has only False values, then its sum would be 0

We can sum up all the boolean values along each row using `sum(axis=1)`

**Note:** Using `sum()` by default sums the boolean values along each column


```python
>>> df.isin(imp_dates).sum(axis=1)

    Ajay      1
    Chetan    2
    Girish    0
    Mohan     1
    Vishal    1
    dtype: int64
>>> 
```

We are interested in only those rows that contain atleast one True value. So,
the sum value for such rows would be > 0

Let's create a mask of the boolean Series for the above comparison 

```python
>>> mask_1819 = df.isin(imp_dates).sum(axis=1) > 0
>>> mask_1819

    Ajay       True
    Chetan     True
    Girish    False
    Mohan      True
    Vishal     True
    dtype: bool
>>> 
```

Now, let's use the mask to filter the values in the DataFrame


```python
>>> df[mask_1819]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Ex01_Date</th>
      <th>Ex02_Date</th>
      <th>Ex03_Date</th>
      <th>Ex04_Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Ajay</th>
      <td>12-Jan-2023</td>
      <td>14-Jan-2023</td>
      <td>18-Jan-2023</td>
      <td>16-Jan-2023</td>
    </tr>
    <tr>
      <th>Chetan</th>
      <td>17-Jan-2023</td>
      <td>18-Jan-2023</td>
      <td>19-Jan-2023</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Mohan</th>
      <td>19-Jan-2023</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Vishal</th>
      <td>14-Jan-2023</td>
      <td>15-Jan-2023</td>
      <td>17-Jan-2023</td>
      <td>18-Jan-2023</td>
    </tr>
  </tbody>
</table>
</div>

And that is our expected result.

Compare the result with the highlighted values shown below from the original
DataFrame


<style type="text/css">
#T_b7ce2_row0_col2, #T_b7ce2_row1_col1, #T_b7ce2_row1_col2, #T_b7ce2_row3_col0, #T_b7ce2_row4_col3 {
  background-color: bisque;
}
</style>
<table border="1" id="T_b7ce2_">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th class="col_heading level0 col0" >Ex01_Date</th>
      <th class="col_heading level0 col1" >Ex02_Date</th>
      <th class="col_heading level0 col2" >Ex03_Date</th>
      <th class="col_heading level0 col3" >Ex04_Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_b7ce2_level0_row0" class="row_heading level0 row0" >Ajay</th>
      <td id="T_b7ce2_row0_col0" class="data row0 col0" >12-Jan-2023</td>
      <td id="T_b7ce2_row0_col1" class="data row0 col1" >14-Jan-2023</td>
      <td id="T_b7ce2_row0_col2" class="data row0 col2" >18-Jan-2023</td>
      <td id="T_b7ce2_row0_col3" class="data row0 col3" >16-Jan-2023</td>
    </tr>
    <tr>
      <th id="T_b7ce2_level0_row1" class="row_heading level0 row1" >Chetan</th>
      <td id="T_b7ce2_row1_col0" class="data row1 col0" >17-Jan-2023</td>
      <td id="T_b7ce2_row1_col1" class="data row1 col1" >18-Jan-2023</td>
      <td id="T_b7ce2_row1_col2" class="data row1 col2" >19-Jan-2023</td>
      <td id="T_b7ce2_row1_col3" class="data row1 col3" >nan</td>
    </tr>
    <tr>
      <th id="T_b7ce2_level0_row2" class="row_heading level0 row2" >Girish</th>
      <td id="T_b7ce2_row2_col0" class="data row2 col0" >17-Jan-2023</td>
      <td id="T_b7ce2_row2_col1" class="data row2 col1" >nan</td>
      <td id="T_b7ce2_row2_col2" class="data row2 col2" >15-Jan-2023</td>
      <td id="T_b7ce2_row2_col3" class="data row2 col3" >nan</td>
    </tr>
    <tr>
      <th id="T_b7ce2_level0_row3" class="row_heading level0 row3" >Mohan</th>
      <td id="T_b7ce2_row3_col0" class="data row3 col0" >19-Jan-2023</td>
      <td id="T_b7ce2_row3_col1" class="data row3 col1" >nan</td>
      <td id="T_b7ce2_row3_col2" class="data row3 col2" >nan</td>
      <td id="T_b7ce2_row3_col3" class="data row3 col3" >nan</td>
    </tr>
    <tr>
      <th id="T_b7ce2_level0_row4" class="row_heading level0 row4" >Vishal</th>
      <td id="T_b7ce2_row4_col0" class="data row4 col0" >14-Jan-2023</td>
      <td id="T_b7ce2_row4_col1" class="data row4 col1" >15-Jan-2023</td>
      <td id="T_b7ce2_row4_col2" class="data row4 col2" >17-Jan-2023</td>
      <td id="T_b7ce2_row4_col3" class="data row4 col3" >18-Jan-2023</td>
    </tr>
  </tbody>
</table>


Wait a minute. How did we get those nicely colored cells above?

Well, this is more of a digression from the topic we are discussing here.  But,
it's a good one I guess :-) 

## Highlighting specific cells

For highlighting specific rows or columns or cells in a Pandas Dataframe, we
use it's `style` property

One handy method on the `style` property is `highlight_null`. As the name
suggests, it is used to highlight empty or `NaN` entries in a DataFrame. This
comes in pretty handy when we want to visually spot the empty cells.

Following code explains how `highlight_null` can be used to highlight empty
cells with a "lightsalmon" color


```python
>>> df.style.highlight_null('lightsalmon')
```

<style type="text/css">
#T_12a1f_row1_col3, #T_12a1f_row2_col1, #T_12a1f_row2_col3, #T_12a1f_row3_col1, #T_12a1f_row3_col2, #T_12a1f_row3_col3 {
  background-color: lightsalmon;
}
</style>
<table id="T_12a1f_">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th class="col_heading level0 col0" >Ex01_Date</th>
      <th class="col_heading level0 col1" >Ex02_Date</th>
      <th class="col_heading level0 col2" >Ex03_Date</th>
      <th class="col_heading level0 col3" >Ex04_Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_12a1f_level0_row0" class="row_heading level0 row0" >Ajay</th>
      <td id="T_12a1f_row0_col0" class="data row0 col0" >12-Jan-2023</td>
      <td id="T_12a1f_row0_col1" class="data row0 col1" >14-Jan-2023</td>
      <td id="T_12a1f_row0_col2" class="data row0 col2" >18-Jan-2023</td>
      <td id="T_12a1f_row0_col3" class="data row0 col3" >16-Jan-2023</td>
    </tr>
    <tr>
      <th id="T_12a1f_level0_row1" class="row_heading level0 row1" >Chetan</th>
      <td id="T_12a1f_row1_col0" class="data row1 col0" >17-Jan-2023</td>
      <td id="T_12a1f_row1_col1" class="data row1 col1" >18-Jan-2023</td>
      <td id="T_12a1f_row1_col2" class="data row1 col2" >19-Jan-2023</td>
      <td id="T_12a1f_row1_col3" class="data row1 col3" >nan</td>
    </tr>
    <tr>
      <th id="T_12a1f_level0_row2" class="row_heading level0 row2" >Girish</th>
      <td id="T_12a1f_row2_col0" class="data row2 col0" >17-Jan-2023</td>
      <td id="T_12a1f_row2_col1" class="data row2 col1" >nan</td>
      <td id="T_12a1f_row2_col2" class="data row2 col2" >15-Jan-2023</td>
      <td id="T_12a1f_row2_col3" class="data row2 col3" >nan</td>
    </tr>
    <tr>
      <th id="T_12a1f_level0_row3" class="row_heading level0 row3" >Mohan</th>
      <td id="T_12a1f_row3_col0" class="data row3 col0" >19-Jan-2023</td>
      <td id="T_12a1f_row3_col1" class="data row3 col1" >nan</td>
      <td id="T_12a1f_row3_col2" class="data row3 col2" >nan</td>
      <td id="T_12a1f_row3_col3" class="data row3 col3" >nan</td>
    </tr>
    <tr>
      <th id="T_12a1f_level0_row4" class="row_heading level0 row4" >Vishal</th>
      <td id="T_12a1f_row4_col0" class="data row4 col0" >14-Jan-2023</td>
      <td id="T_12a1f_row4_col1" class="data row4 col1" >15-Jan-2023</td>
      <td id="T_12a1f_row4_col2" class="data row4 col2" >17-Jan-2023</td>
      <td id="T_12a1f_row4_col3" class="data row4 col3" >18-Jan-2023</td>
    </tr>
  </tbody>
</table>

Here, we have used the color name as "lightsalmon". You can use any other color
name as well. You can find the list of the colors that you can provide at [this
stackoverflow answer](https://stackoverflow.com/a/37232760/4106458){:target="_blank"}

Unfortunately, there is no direct method like `highlight_null` that can be used
to highlight specific cells. To be able to highlight specific cells based on a
condition, we need to tweak the underlying html properties as mentioned in
[this stackoverflow answer](https://stackoverflow.com/a/53185070/4106458){:target="_blank"}

We use `applymap` method to element-wise apply these CSS style changes

We use a `lambda` function to decide if the CSS style changes are needed, based
on the comparison of the cell value with the values present in `imp_date` list


```python
>>> (df
...   .style
...   .applymap(lambda x: 'background-color:bisque' 
...             if x in imp_dates else None)
...   )
>>> 
```

<style type="text/css">
#T_5d3bb_row0_col2, #T_5d3bb_row1_col1, #T_5d3bb_row1_col2, #T_5d3bb_row3_col0, #T_5d3bb_row4_col3 {
  background-color: bisque;
}
</style>
<table id="T_5d3bb_">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th class="col_heading level0 col0" >Ex01_Date</th>
      <th class="col_heading level0 col1" >Ex02_Date</th>
      <th class="col_heading level0 col2" >Ex03_Date</th>
      <th class="col_heading level0 col3" >Ex04_Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_5d3bb_level0_row0" class="row_heading level0 row0" >Ajay</th>
      <td id="T_5d3bb_row0_col0" class="data row0 col0" >12-Jan-2023</td>
      <td id="T_5d3bb_row0_col1" class="data row0 col1" >14-Jan-2023</td>
      <td id="T_5d3bb_row0_col2" class="data row0 col2" >18-Jan-2023</td>
      <td id="T_5d3bb_row0_col3" class="data row0 col3" >16-Jan-2023</td>
    </tr>
    <tr>
      <th id="T_5d3bb_level0_row1" class="row_heading level0 row1" >Chetan</th>
      <td id="T_5d3bb_row1_col0" class="data row1 col0" >17-Jan-2023</td>
      <td id="T_5d3bb_row1_col1" class="data row1 col1" >18-Jan-2023</td>
      <td id="T_5d3bb_row1_col2" class="data row1 col2" >19-Jan-2023</td>
      <td id="T_5d3bb_row1_col3" class="data row1 col3" >nan</td>
    </tr>
    <tr>
      <th id="T_5d3bb_level0_row2" class="row_heading level0 row2" >Girish</th>
      <td id="T_5d3bb_row2_col0" class="data row2 col0" >17-Jan-2023</td>
      <td id="T_5d3bb_row2_col1" class="data row2 col1" >nan</td>
      <td id="T_5d3bb_row2_col2" class="data row2 col2" >15-Jan-2023</td>
      <td id="T_5d3bb_row2_col3" class="data row2 col3" >nan</td>
    </tr>
    <tr>
      <th id="T_5d3bb_level0_row3" class="row_heading level0 row3" >Mohan</th>
      <td id="T_5d3bb_row3_col0" class="data row3 col0" >19-Jan-2023</td>
      <td id="T_5d3bb_row3_col1" class="data row3 col1" >nan</td>
      <td id="T_5d3bb_row3_col2" class="data row3 col2" >nan</td>
      <td id="T_5d3bb_row3_col3" class="data row3 col3" >nan</td>
    </tr>
    <tr>
      <th id="T_5d3bb_level0_row4" class="row_heading level0 row4" >Vishal</th>
      <td id="T_5d3bb_row4_col0" class="data row4 col0" >14-Jan-2023</td>
      <td id="T_5d3bb_row4_col1" class="data row4 col1" >15-Jan-2023</td>
      <td id="T_5d3bb_row4_col2" class="data row4 col2" >17-Jan-2023</td>
      <td id="T_5d3bb_row4_col3" class="data row4 col3" >18-Jan-2023</td>
    </tr>
  </tbody>
</table>


## Discussion

[Start a discussion on GitHub](https://github.com/asarfraaz/share2learn/discussions/new/choose){:target="_blank"} about this article and let me know your thoughts or suggestions.

