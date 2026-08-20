---
layout: page
nav_order: 9
title: 🐍 Python Reference
description: >-
    Python Reference Guide
---

# **Detailed Python Reference Sheet**

{:.no_toc}
Created by Nishant Kheterpal and Jessica Hu; contributions by Jonathan Ferrari, Edwin Vargas, Bing Concepcion, Tim Xie, and Isaac Chung

Updated and maintained by Marissa Lumpkin
## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Abbreviated Reference Sheet

Students will receive an abbreviated reference sheet for exams.

* [Past Midterm Reference Sheet](https://drive.google.com/file/d/1OD3ix5Ffch5vZGaJlDX6KSwc9Dfyov-D/view)

---

## Table Functions and Methods
In the examples in the left column, `np` refers to the NumPy module, as usual. Everything else is a function, a method, an example of an argument to a function or method, or an example of an object we might call the method on. For example, `tbl` refers to a table, `array` refers to an array, `str` refers to a string, and `num` refers to a number. `array.item(0)` is an example call for the method item, and in that example, `array` is the name previously given to some array.

| **Name** | **Description** | **Input** | **Output** |
| `Table()` | Create an empty table, usually to extend with data ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | None | **Table:** an empty table |
| `Table().read_table(filename)` | Create a table from a data file ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | **string**: name of the file | 	**Table** with the contents of the data file |
| `tbl.num_rows` | Compute the number of rows in a table ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | None | **int**: number of rows in the table |
| `tbl.num_columns` | Compute the number of columns in a table ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | None | **int**: number of columns in the table |
| `tbl.labels` | Lists the column labels in a table ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | None | **array**: names of each column (as strings) in the table |
| `tbl.split(n)` | Splits `tbl` into two distinct tables. Where the first table contains `n` rows randomly sampled without replacement, and the second table contains the remaining rows ([Ch 17.6](https://inferentialthinking.com/chapters/17/6/Multiple_Regression.html#multiple-linear-regression)) | **int**: number of rows randomly sampled into the first table. `n` must be between 1 and `tbl.num_rows` - 1 | **tuple** containing the two tables |
| `tbl.show(n)` | Display `n` rows of a table. If no argument is specified, defaults to displaying the entire table. ([Ch 6.1](https://inferentialthinking.com/chapters/06/1/Sorting_Rows.html)) | (Optional) **int**: number of rows you want to display | None: displays a table with `n` rows |
{: .py-ref-table}

### Column Operations

| **Name** | **Description** | **Input** | **Output** |
| `tbl.column(column_name_or_index)` | The values of a column (an array) ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | **string** _or_ **int**: column name or index | **array**: values in that column |
| `tbl.with_columns(name, values)` <br> `tbl.with_columns(n1, v1, n2, v2, ...)` | A table with an additional or replaced column or columns. `name` is a string for the name of a column, `values` is an array ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | 1. **string**: name of the new column; <br> 2. **array**: values in that column | **Table**: a copy of the original Table with the new columns added |
| `tbl.select(col1, col2, ...)` | Create a copy of a table with only some of the columns. Each column is the column name or index. ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | **string** _or_ **int**: column name(s) or index(es) | **Table** with the selected columns|
| `tbl.drop(col1, col2, ...)` | Create a copy of a table _without_ some of the columns. Each column is the column name or index. ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | **string** _or_ **int**: column name(s) or index(es) | **Table** without the selected columns |
| `tbl.relabeled(old_label, new_label)` |	Creates a new table, changing the column name specified by the old label to the new label, and leaves the original table unchanged. ([Ch 6](https://inferentialthinking.com/chapters/06/Tables.html)) | 1. **string**: the old column name <br> 2. **string**: the new column name | **Table**: a new Table |
{: .py-ref-table}

### Row Operations

| **Name** | **Description** | **Input** | **Output** |
| `tbl.where(column, predicate)` | Create a copy of a table with only the rows that match some _predicate_. See `Table.where` predicates below. ([Ch 6.2](https://inferentialthinking.com/chapters/06/2/Selecting_Rows.html)) | 1. **string** _or_ **int**: column name or index <br> 2. `are.(...)` predicate | **Table**: a copy of the original table with only the rows that match the predicate |
| `tbl.take(row_indices)` | A table with only the rows at the given indices. `row_indices` is either an array of indices or an integer corresponding to one index. ([Ch 6.2](https://inferentialthinking.com/chapters/06/2/Selecting_Rows.html)) | **array** of ints: indices of rows to include in the Table *or* **int**: index of the row to include | **Table**: a copy of the original table with only the rows at the given indices |
| `tbl.sort(column_name_or_index)` | Create a copy of a table sorted by the values in a column. **Defaults to ascending order** unless descending=True is included. ([Ch 6.1](https://inferentialthinking.com/chapters/06/1/Sorting_Rows.html)) | 1. **string** _or_ **int**: column index or name <br> 2. (Optional) **boolean:** default `descending=False` | **Table**: a copy of the original with the column sorted |
| `tbl.row(row_index)` | Accesses the row of a table by taking the index of the row as its argument. Note that rows are in general not arrays, as their elements can be of different types. However, you can use `.item` to access a particular element of a row using `row.item(label)`. ([Ch 17.3](https://inferentialthinking.com/chapters/17/3/Rows_of_Tables.html)) | **int**: row index | **Row object** with the values of the row and labels of the corresponding columns |
| `tbl.rows` | Can use to access all of the rows of a table. | None | **Rows object** made up of all rows as individual row objects |
| `tbl.sample(k)` <br> `tbl.sample(k, with_replacement)` | A new table where `k` rows are randomly sampled from the original table; by default, `k=tbl.num_rows`. Default is with replacement. For sampling without replacement, use argument `with_replacement=False`. For a non-uniform sample, provide a third argument `weights=distribution` where `distribution` is an array or list containing the probability of each row. ([Ch 10](https://www.inferentialthinking.com/chapters/10/Sampling_and_Empirical_Distributions.html)) | 1. **int**: sample size <br> 2. (Optional) **boolean**: default `with_replacement=True` | **Table**: a new table with `k` rows |
{: .py-ref-table}

### Data Manipulation

| **Name** | **Description** | **Input** | **Output** |
| `tbl.apply(function)` <br> `tbl.apply(function, col1, col2, ...)` | Returns an array of values resulting from applying a function to each item in a column. ([Ch 8.1](https://www.inferentialthinking.com/chapters/08/1/Applying_a_Function_to_a_Column.html)) | 1. **function**: function to apply to column <br> 2. (Optional) **string** *or* **int**: name or index of the column to apply function to (if you have multiple columns, the respective column's values will be passed as the corresponding argument to the function), and if there is no argument, your function will be applied to every row object in `tbl` | **array**: contains an element for each value in the original column after applying the function to it |
| `tbl.group(column_or_columns, collect)` | Group rows by unique values or combinations of values in a column(s). Multiple columns must be entered in array or list form. Other values aggregated by count (default) or optional argument `collect`. ([Ch 8.2](https://www.inferentialthinking.com/chapters/08/2/Classifying_by_One_Variable.html)) | 1. **string/int** *or* **array**: column(s) to group on <br> 2. (Optional) **function**: function to aggregate values in cells (defaults to `count`) | **Table**: a new table, with one row per group. Values in columns are aggregated. |
| `tbl.pivot(col1, col2)` <br> `tbl.pivot(col1, col2, values, collect)` | A pivot table where each unique value in `col1` has its own column and each unique value in `col2` has its own row. Count or aggregate values from a third column, collect with some function. Default `values` and `collect` return counts in cells. ([Ch 8.3](https://inferentialthinking.com/chapters/08/3/Cross-Classifying_by_More_than_One_Variable.html)) | 1. **string** *or* **int**: name or index of column whose unique values will make up columns of pivot table <br> 2. **string** *or* **int**: name or index of column whose unique values will make up rows of pivot table <br> 3. (Optional) **string** *or* **int**: name or index of column that describes the values of cell <br> 4. (Optional) **function**: how the values are collected (e.g. `sum` or `np.mean`) | **Table**: a new table reshaped so that unique values of `col1` form columns, and unique values of `col2` form rows. Optionally fills with aggregated values from another column. |
| `tblA.join(colA, tblB, colB)` <br> `tblA.join(colA, tblB)` | Generate a table with the columns of tblA and tblB, containing rows for all values of a column that appear in both tables. Default `colB` is `colA`. `colA` and `colB` must be strings specifying column names. ([Ch 8.4](https://www.inferentialthinking.com/chapters/08/4/Joining_Tables_by_Columns.html)) | 1. **string**: name of a column in `tblA` with values to join on <br> 2. **Table**: other Table <br> 3. (Optional) **string**: if column names are different between Tables, the name of the shared column in `tblB` | **Table**: a new table with matched rows from `tblA` and `tblB` based on join columns |
| `tbl.bin(column)` <br> `tbl.bin(column, bins)` | 	Groups values into intervals, known as bins. Results in a two-column table that contains the number of rows in each bin. The first column lists the left endpoints of the bins, except in the last row. If the `bins` argument isn't used, default is to produce 10 equally wide bins between the min and max values of the data. ([Ch 7.2](https://www.inferentialthinking.com/chapters/07/2/Visualizing_Numerical_Distributions.html)) | 1. **string** _or_ **int**: column name or index <br> 2. (Optional) **array** of ints/floats denoting bin boundaries or an **int** of the number of bins you want | **Table**: a new table with a column of bin labels, grouping values of that column into intervals |
{: .py-ref-table}

### Visualization

| **Name** | **Description** | **Input** | **Output** |
| `tbl.scatter(x_column, y_column)` <br> `tbl.scatter(x_column, y_column, fit_line)` | Draws a scatter plot consisting of one point for each row of the table. Note that `x_column` and `y_column` must be strings specifying column names. Include optional argument `fit_line=True` if you want to draw a line of best fit for each set of points. ([Ch 7](https://www.inferentialthinking.com/chapters/07/Visualization.html)) | 1. **string**: name of the column on the x-axis <br> 2. **string**: name of the column on the y-axis <br> 3. (Optional) `fit_line=True` | None: draws a scatter plot |
| `tbl.plot(x_column)` <br> `tbl.plot(x_column, y_column)` | Draw a line graph consisting of one point for each row of the table. If you only specify one column, it will plot the rest of the columns on the y-axis as different colored lines. ([Ch 7](https://www.inferentialthinking.com/chapters/07/Visualization.html)) | 1. **string**: name of the column on the x-axis <br> 2. **string**: name of the column on the y-axis | None: draws a line graph |
| `tbl.barh(categories)` <br> `tbl.barh(categories, values)`| Displays a bar chart with bars for each category in a column, with length proportional to the corresponding frequency. values argument unnecessary if table has only a column of categories and a column of values. ([Ch 7.1](https://www.inferentialthinking.com/chapters/07/1/Visualizing_Categorical_Distributions.html)) | 1. **string**: name of the column with categories <br> 2. (Optional) **string**: the name of the column with values for corresponding categories | None: draws a bar chart |
| `tbl.hist(column, unit, bins, group)` | 	Generates a histogram of the numerical values in a column. `unit` and `bins` are optional arguments, used to label the axes and group the values into intervals (bins), respectively. Bins have the form `[a, b)`, where `a` is included in the bin and `b` is not. ([Ch 7.2](https://www.inferentialthinking.com/chapters/07/2/Visualizing_Numerical_Distributions.html)) | 1. **string**: name of the column with numerical values <br> 2. (Optional) **string**: units of x-axis <br> 3. (Optional) **array** of ints/floats denoting bin boundaries <br> 4. (Optional) **string**: name of categorical column to draw separate overlaid histograms for | None: draws a histogram |
{: .py-ref-table}

## Table .where Predicates

Any of these predicates can be negated by adding `not_` in front of them, e.g. `are.not_equal_to(x)` or `are.not_containing(S)`.

| **Predicate** | **Description** |
| `are.equal_to(x)` | Equal to `x` |
| `are.above(x)` | Greater than `x` |
| `are.above_or_equal_to(x)` | Greater than or equal to `x` |
| `are.below(x)` | Less than `x` |
| `are.below_or_equal_to(x)` | Less than or equal to `x` |
| `are.between(x, y)` | Greater than or equal to `x` and less than `y` |
| `are.between_or_equal_to(x, y)` | Greater than or equal to `x`, and less than or equal to `y` |
| `are.strictly_between(x, y)` | Greater than `x` and less than `y` |
| `are.contained_in(A)` | Is a substring of `A` (if `A` is a string) or an element of `A` (if `A` is a list/array) |
| `are.containing(S)` | Contains the string `S` |

## Array Functions and Methods

| **Name** | **Chapter** | **Description** |
| `make_array(val1, val2, ...)` | [5](https://www.inferentialthinking.com/chapters/05/Sequences.html) | Makes a NumPy array with the values passed in |
| `np.array(obj)` | [5](https://www.inferentialthinking.com/chapters/05/Sequences.html) | Converts `obj` of another type (e.g. `row` object) into a NumPy array. |
| `array.item(index)` | [5.3](https://www.inferentialthinking.com/chapters/05/3/More_On_Arrays.html) | Returns the element at `index` (remember Python indices start at 0!) |
| `max(array)` | [3.3](https://www.inferentialthinking.com/chapters/03/3/Calls.html) | Returns the maximum value of an array |
| `min(array)` | [3.3](https://www.inferentialthinking.com/chapters/03/3/Calls.html) | Returns the minimum value of an array |
| `sum(array)` | [3.3](https://www.inferentialthinking.com/chapters/03/3/Calls.html) | Returns the sum of the values in an array |
| `len(array)` | [3.3](https://www.inferentialthinking.com/chapters/03/3/Calls.html) | Returns the length (number of elements) of an array. |
| `percentile(p, array)` | [13.1](https://www.inferentialthinking.com/chapters/13/1/Percentiles.html) | Returns the `p`-th percentile of the array. Note that this is **not** `np.percentile`. |
| `abs(num)`, `np.abs(array)` | [3.3](https://www.inferentialthinking.com/chapters/03/3/Calls.html) | Returns the absolute value of a number, or each number in an array |
| `round(num)`, `np.round(array)` <br> `round(num, decimals)`, `np.round(array, decimals)` | [3.3](https://www.inferentialthinking.com/chapters/03/3/Calls.html) | Rounds a number or an array of numbers to the nearest integer. If `decimals` (integer) is included, rounds to that many digits. If `decimals` is negative, remove that many digits of precision. |
| `np.mean(array)` <br> `np.average(array)` | [5.1](https://www.inferentialthinking.com/chapters/05/1/Arrays.html) | Returns the mean value of an array |
| `np.std(array)` | [14.2](https://www.inferentialthinking.com/chapters/14/2/Variability.html#Standard-Deviation) | Returns the standard deviation of an array |
| `np.diff(array)` | [5.1](https://www.inferentialthinking.com/chapters/05/1/Arrays.html) | Returns a new array of size `len(array)-1` with elements equal to the difference between adjacent elements (`array[i + 1] - array[i]`). |
| `np.sqrt(array)` | [5.1](https://www.inferentialthinking.com/chapters/05/1/Arrays.html) | Returns an array with the square root of each element |
| `np.arange(start, stop, step)` <br> `np.arange(start, stop)` <br> `np.arange(stop)` | [5.2](https://www.inferentialthinking.com/chapters/05/2/Ranges.html) | An array of numbers starting with `start`, going up in increments of `step`, and going up to **but excluding** `stop`. When `start` and/or `step` are left out, default values are used in their place. Default `step` is 1; default `start` is 0. |
| `np.random.choice(array, n)` <br> `np.random.choice(array)` <br> `np.random.choice(array, n, replace=...)` | [9](https://www.inferentialthinking.com/chapters/09/Randomness.html) | Picks one (by default) or some number (`n`) of items from `array` at random with replacement. |
| `np.count_nonzero(array)` | [9](https://www.inferentialthinking.com/chapters/09/Randomness.html) | Returns the number of non-zero (or `True`) elements in an array. |
| `np.append(array, item)` | [9.2](https://www.inferentialthinking.com/chapters/09/2/Iteration.html) | Returns a copy of the input array with `item` appended to the end. |

## String Methods

| **Name** | **Description** |
| `len(str)` | Returns the number of characters in the string. |
| `str.split(separator)` | Splits the string (`str`) into a list based on the `separator` that is passed in |
| `str.join(array)` | Combines each element of `array` into one string, with `str` being in-between each element |
| `str.replace(old_string, new_string)` | Replaces each occurrence of `old_string` in `str` with the value of `new_string` ([Ch 4.2.1](https://www.inferentialthinking.com/chapters/04/2/1/String_Methods.html)) |

## Miscellaneous Functions

These are functions in the `datascience` library that are used in the course that don't fall into any of the categories above. You can also read more about all functions in the `datascience` library on the [`datascience` documentation](http://data8.org/datascience/).

| **Name** | **Description** | **Input** | **Output** |
| `sample_proportions(n, model_proportions)` | `n` should be an integer; `model_proportions` should be an array of probabilities that sum up to 1. The function samples `n` objects from the distribution specified by `model_proportions`. It returns an array with the same size as `model_proportions`. Each item in the array corresponds to the proportion of times it was sampled out of the `n` times. ([Ch 11.1](https://inferentialthinking.com/chapters/11/1/Assessing_a_Model.html)) | 1. **int**: sample size <br> 2. **array**: an array of proportions; sums to 1 | **array**: proportion of times that each item was sampled from `model_proportions` in `n` samples; sums to 1 |
| `minimize(function)` | Returns an array of values such that if each value in the array was passed into `function` as arguments, it would minimize the output value of `function`. ([Ch 15.4](https://www.inferentialthinking.com/chapters/15/4/Least_Squares_Regression)) | **function**: name of function to minimize | **array**: arguments that minimize the function (order matches function’s arguments) |

## Histogram Bounds Language

Histogram bins include the data at their left endpoint, but not the data at their right endpoint. The only exception is the rightmost bin, which includes *both* endpoints. When we ask you questions relating to ranges of values in the bins of histograms, we will use one of the three phrases below.

| **Phrase** | **Interpretation** |
| Between x and y | Greater than or equal to x and less than y |
| Between or equal to x and y | Greater than or equal to x and less than or equal to y |
| Strictly between x and y | Greater than x and less than y |

## JupyterHub Keyboard Shortcuts
First, to enter shortcut mode / exit editing mode, press `Esc`. This will enable you to use any of the keyboard shortcuts below.

|Operation|Keys|
|---|---|
|To enter shortcut mode / exit editing mode|`Esc`|
|Enter edit mode | `Enter`|
|Insert cell above|`a`|
|Insert cell below|`b`|
|Delete selected cell|`d` + `d` (Press `d` twice)|
|Convert cell to Markdown cell | `m` |
|Convert cell to Code cell | `y` |
|Undo cell operation|`z`|
|Copy cell|`c`|
|Paste cell|`v`|
|Paste cell above|`Shift` + `v`|
|Redo|`Ctrl` + `Shift` + `z`|
|Undo|`Ctrl` + `z`|
