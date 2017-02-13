Data Preparation with Python & R
==================

Brought to you by [Lesley Cordero](http://www.columbia.edu/~lc2958) and [ADI](https://adicu.com)

## Table of Contents

- [0.0 Setup](#00-setup)
	+ [0.1 Python & Pip](#01-python--pip)
	+ [0.2 R & R Studio](#02-r--r-studio)
	+ [0.3 Other](#03-other)
	+ [0.4 Virtual Environment](#04-virtual-environment)
- [1.0 Introduction](#10-introduction)
- [2.0 DataFrames](#20-dataframes)
	+ [2.1 Pandas](#21-pandas)
	+ [2.2 dplyr](#22-dplyr)
		* [2.2.1 Select](#221-select)
		* [2.2.2 Filter](#222-filter)
		* [2.2.3 Pipe Operator](#223-pipe-operator)
		* [2.2.4 Arrange](#224-arrange)
		* [2.2.5 Functions](#225-functions)
- [5.0 Resources](#50-resources)


## 0.0 Setup

This guide was written in Python 3.5 and R 3.2.3.

### 0.1 Python & Pip

Download [Python](https://www.python.org/downloads/) and [Pip](https://pip.pypa.io/en/stable/installing/).

### 0.2 R & R Studio

Install [R](https://www.r-project.org/) and [R Studio](https://www.rstudio.com/products/rstudio/download/).

### 0.3 Other

Let's install the modules we'll need for this tutorial. Open up your terminal and enter the following commands to install the needed python modules: 

```
pip3 install zipfile
```

Next, to install the R packages, cd into your workspace, and enter the following, very simple, command into your bash: 

```
R
```

This will prompt a session in R! From here, you can install any needed packages. For the sake of this tutorial, enter the following into your terminal R session:

```
install.packages("")
```

### 0.4 Virtual Environment

If you'd like to work in a virtual environment, you can set it up as follows: 

```
pip3 install virtualenv
virtualenv your_env
```

And then launch it with: 
```
source your_env/bin/activate
```

To execute the visualizations in matplotlib, do the following:

```
cd ~/.matplotlib
vim matplotlibrc
```
And then, write `backend: TkAgg` in the file. Now you should be set up with your virtual environment!

Cool, now we're ready to start! 

## 1.0 Introduction

We've gone over Data Acquisition as of now, so we know how to <i>get</i> our data. But once you have the data, it might not be in the best shape. You might have scraped a bunch of data from a website, but need it in the form of a dataframe to work with it in an easier manner. This process is called data preparation - preparing your data in a format that's easiest to form with.

### 1.1 Overview

<b> Data Acquisition: </b> Reading and writing with a variety of file formats and databases.
<b> Preparation: </b> Cleaning, munging, combining, normalizing, reshaping, slicing and dicing, and transforming data for analysis.
<b> Transformation: </b> Applying mathematical and statistical operations to groups of data sets to derive new data sets. For example, aggregating a large table by group variables.
<b> Modeling and computation: </b> Connecting your data to statistical models, machine learning algorithms, or other computational tools
<b> Presentation: </b> Creating interactive or static graphical visualizations or textual summaries

### 1.2 Glossary

Here is some common terminology that we'll encounter throughout the workshop:

<b> Munging/Wrangling: </b> This refers to the overall process of manipulating unstructured or messy data into a structured or clean form. 


## 2.0 Pandas

Pandas allows us to deal with data in a way that us humans can understand it - with labelled columns and indexes. It allows us to effortlessly import data from files such as CSVs, allows us to quickly apply complex transformations and filters to our data and much more. Along with Numpy and Matplotlib, it helps create a really strong base for data exploration and analysis in Python. 

``` python
import pandas as pd 
from pandas import Series, DataFrame
```

### 2.1 Series

A Series is a one-dimensional array-like object containing an array of data (of any NumPy data type) and an associated array of data labels, called its index. The simplest Series is formed from only an array of data:

``` python
obj = Series([4, 7, -5, 3])
```

Often it will be desirable to create a Series with an index identifying each data point:

``` python
obj2 = Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
```

You can also take a dictionary and convert it to a Series:
``` python
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj3 = Series(sdata)
```

### 2.2 DataFrames

A DataFrame represents a tabular, spreadsheet-like data structure containing an ordered collection of columns, each of which can be a different value type (numeric, string, boolean, etc.).

There are numerous ways to construct a DataFrame, though one of the most common is from a dict of equal-length lists or NumPy arrays:

``` python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'], 'year': [2000, 2001, 2002, 2001, 2002], 'pop': [1.5, 1.7, 3.6, 2.4, 2.9]}
```

Then we take this and convert it to a DataFrame:
``` python
frame = DataFrame(data)
```
This gets us:
```
   pop   state  year
0  1.5    Ohio  2000
1  1.7    Ohio  2001
2  3.6    Ohio  2002
3  2.4  Nevada  2001
4  2.9  Nevada  2002

You can also specify the sequence of columns by:

``` python
DataFrame(data, columns=['year', 'state', 'pop'])
```

### 2.2.1 Apply

Lets's generate a random dictionary:

``` python
frame = DataFrame(np.random.randn(4, 3), columns=list('bde'), index=['Utah', 'Ohio', 'Texas', 'Oregon'])
```

With this, we can apply a function on a DataFrame:
``` python
np.abs(frame)
```

We can also apply functions with the `apply()` method:

``` python
f = lambda x: x.max() - x.min()
frame.apply(f)
```

#### 2.2.2 Sorting

To sort lexicographically by row or column index, use the sort_index method, which returns a new, sorted object:

``` python
frame.sort_index()
```

## 3.0 dplyr

Similar to Dplyr, dplyr allows us to transform and summarize tabular data with rows and columns. It contains a set of functions that perform common data manipulation operations like filtering rows, selecting specific columns, re-ordering rows, adding new columns, and summarizing data.

First we begin by loading in the needed packages:

``` R
library(dplyr)
library(downloader)
```

Using the data available in [this](https://github.com/lesley2958/data-science-r/blob/master/msleep_ggplot2.csv) repo, we''ll load the data into R:

``` R
url <- "https://raw.githubusercontent.com/genomicsclass/dagdata/master/inst/extdata/msleep_ggplot2.csv"
filename <- "msleep_ggplot2.csv"
if (!file.exists(filename)) download(url,filename)
msleep <- read.csv("msleep_ggplot2.csv")
head(msleep)
```

### 3.1 Select

To demonstrate how the `select()` method works, we select the name and sleep_total columns.

``` R
sleepData <- select(msleep, name, sleep_total)
head(sleepData)
```

To select all the columns except a specific column, you can use the subtraction sign:

``` R
head(select(msleep, -name))
```

You can also select a range of columns with a colon:

``` R
head(select(msleep, name:order))
```

#### 3.2 Filter

Using the `filter()` method in dplyr we can select rows that meet a certain criterion, such as in the following:

``` R
filter(msleep, sleep_total >= 16)
```
There, we filter out the animals whose sleep total is less than 16 hours. If you want to expand the criteria, you can: 

```R
filter(msleep, sleep_total >= 16, bodywt >= 1)
```

#### 3.3 Pipe Operator

dplyr imports the pipe operator from another package, `magrittr`. This operator allows you to pipe the output from one function to the input of another function. Instead of nesting functions. 

Recall: 

``` R
head(select(msleep, name, sleep_total))
```

Instead, you can rewrite this as: 

``` R
msleep %>% 
    select(name, sleep_total) %>% 
    head
```

This function becomes particularly useful later on. 

### 3.4 Arrange

To re-order rows by a particular column, you can list the name of the column you want to arrange the rows by: 

``` R
msleep %>% arrange(order) %>% head
```


### 3.5 Functions

`arrange()`: re-order or arrange rows <br>
`filter()`: filter rows <br>
`group_by()`: allows for group operations in the “split-apply-combine” concept <br>
`mutate()`: create new columns <br>
`select()`: select columns <br>
`summarise()`: summarise values


## 4.0 Extracting Zipfiles


### 4.1 OS Module

``` python
import os
```

``` python
cwd = os.getcwd()
dir_path  = os.path.join(cwd, 'Example')
```

``` python
if not os.path.exists(dir_path):
    os.makedirs(dir_path)
```

``` python
os.listdir(cwd)
```


### 4.2 ZipFile

``` python
import zipfile
import os
zip_name = 'example.zip'
```

``` python
cwd = os.getcwd()
zip_path= os.path.join(cwd, zip_name)
```

``` python
with zipfile.ZipFile(zip_path, 'r') as z:
	z.extractall(cwd)
```

``` python
os.listdir(dir_path)
```

## 5.0 Final Words

### 5.1 Resources
