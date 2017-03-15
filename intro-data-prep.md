Data Preparation with Python & R
==================

Brought to you by [Lesley Cordero](http://www.columbia.edu/~lc2958), [Byte Academy](byteacademy.co), and [ADI](https://adicu.com). 

## Table of Contents

- [0.0 Setup](#00-setup)
	+ [0.1 Python & Pip](#01-python--pip)
	+ [0.2 R & R Studio](#02-r--r-studio)
	+ [0.3 Other](#03-other)
	+ [0.4 Virtual Environment](#04-virtual-environment)
- [1.0 Introduction](#10-introduction)
	+ [1.1 Overview](#11-overview)
	+ [1.2 Glossary](#12-glossary)
- [2.0 Pandas](#20-pandas)
	+ [2.1 Series](#21-series)
	+ [2.2 DataFrames](#22-dataframes)
		* [2.2.1 Apply](#221-apply)
		* [2.2.2 Sorting](#222-sorting)
- [3.0 dplyr](#30-dplyr)
	+ [3.1 Select](#31-select)
	+ [3.2 Filter](#32-filter)
	+ [3.3 Pipe Operator](#33-pipe-operator)
	+ [3.4 Arrange](#34-arrange)
	+ [3.5 Functions](#35-functions)
- [4.0 Extracting Zipfiles](#40-extracting-zipfiles)
	+ [4.1 OS Module](#41-os-module)
	+ [4.2 Zipfile](#42-zipfile)
- [5.0 Data Merging](#50-data-merging)
	+ [5.1 Merging on Index](#51-merging-on-index)
- [6.0 Final Words](#50-final-words)
	+ [6.2 Mini Courses](#62-mini-courses)

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
pip3 install os
pip3 install pandas
```

Next, to install the R packages, cd into your workspace, and enter the following, very simple, command into your bash: 

```
R
```

This will prompt a session in R! From here, you can install any needed packages. For the sake of this tutorial, enter the following into your terminal R session:

```
install.packages("dplyr")
install.packages("downloader")
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

Cool, now we're ready to start! 

## 1.0 Introduction

We've gone over Data Acquisition as of now, so we know how to <i>get</i> our data. But once you have the data, it might not be in the best shape. You might have scraped a bunch of data from a website, but need it in the form of a dataframe to work with it in an easier manner. This process is called data preparation - preparing your data in a format that's easiest to form with.

### 1.1 Overview

<b> Data Acquisition: </b> Reading and writing with a variety of file formats and databases. <br>
<b> Preparation: </b> Cleaning, munging, combining, normalizing, reshaping, slicing and dicing, and transforming data for analysis. <br>
<b> Transformation: </b> Applying mathematical and statistical operations to groups of data sets to derive new data sets. For example, aggregating a large table by group variables. <br>
<b> Modeling and computation: </b> Connecting your data to statistical models, machine learning algorithms, or other computational tools <br>
<b> Presentation: </b> Creating interactive or static graphical visualizations or textual summaries <br>

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
```
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

Naive Bayes works on Bayes Theorem of probability to predict the class of a given data point. Naive Bayes is extremely fast compared to other classification algorithms and works with an assumption of independence among predictors. 

The Naive Bayes model is easy to build and particularly useful for very large data sets. Along with simplicity, Naive Bayes is known to outperform even highly sophisticated classification methods.


### 2.3 Challenge

Recall Bayes Theorem, which provides a way of calculating the posterior probability. Its formula is as follows:

![alt text](https://github.com/ByteAcademyCo/stats-programmers/blob/master/bayes.png?raw=true "Logo Title Text 1")

Let's go through an example of how the Naive Bayes Algorithm works using `pandas`. We'll go through a classification problem that determines whether a sports team will play or not based on the weather. 

Let's load the module data:

``` python
import pandas as pd
f1 = pd.read_csv("./weather.csv")
```

#### 2.3.1 Frequency Table

The first actual step of this process is converting the dataset into a frequency table. Using the `groupby()` function, we get the frequencies:

``` python
df = f1.groupby(['Weather','Play']).size()
```

Now let's split the frequencies by weather and yes/no. Let's start with the three weather frequencies:

``` python
df2 = f1.groupby('Weather').count()
```

Now let's get the frequencies of yes and no:

``` python
df1 = f1.groupby('Play').count()
```

#### 2.3.2 Likelihood Table


Next, you would create a likelihood table by finding the probabilites of each weather condition and yes/no. This will require that we add a new column that takes the play frequency and divides it by the total data occurances. 


``` python
df1['Likelihood'] = df1['Weather']/len(f1)
df2['Likelihood'] = df2['Play']/len(f1)
```

This gets us a dataframe that looks like:

```
          Play  Likelihood
Weather                   
Overcast     4    0.285714
Rainy        5    0.357143
Sunny        5    0.357143
```

Now, we're able to use the Naive Bayesian equation to calculate the posterior probability for each class. The highest posterior probability is the outcome of prediction.

#### 2.3.1 Calculation

So now we need a question. Let's propose the following: "Players will play if the weather is sunny. Is this true?"

From this question, we can construct Bayes Theorem. So what's our P(A|B)? P(Yes|Sunny), which gives us:

P(Yes|Sunny) = (P(Sunny|Yes)*P(Yes))/P(Sunny)

Based off the likelihood tables we created, we just grab P(Sunny) and P(Yes). 

``` python
ps = df2['Likelihood']['Sunny']
py = df1['Likelihood']['Yes']
```

That leaves us with P(Sunny|Yes). This is the probability that the weather is sunny given that the players played that day. In `df`, we see that the total number of `yes` days under `sunny` is 3. We take this number and divide it by the total number of `yes` days, which we can get from `df`. 

``` python
psy = df['Sunny']['Yes']/df1['Weather']['Yes']
```

Now, we just have to plug these variables into bayes theorem: 

``` python
p = (psy*py)/ps
```

And we get:

```
0.59999999999999998
```

That means the answer to our original question is yes!

## 3.0 dplyr

Similar to pandas, dplyr allows us to transform and summarize tabular data with rows and columns. It contains a set of functions that perform common data manipulation operations like filtering rows, selecting specific columns, re-ordering rows, adding new columns, and summarizing data.

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

Oftentimes, you'll have to download a large number of files. They might come in the form of zipfiles. Instead of manually unzipping them, you can use Python to extract these files for you, using the `os` and `zipfile` modules. 

### 4.1 OS Module

The `os` module provides us a portable way of using operating system dependent functionality. We'll begin exploring it's capabilities now:

``` python
import os
```

With the `os.getcwd()` method, we can get the current directory we're in. This is particularly useful when working with a large number of files in a certain folder. In this case, we'll use `os` to work with the zipfiles:

``` python
cwd = os.getcwd()
dir_path  = os.path.join(cwd, 'Example')
```

With `os`, you can check to see if a certain directory exists. Here, if it doesn't exist, we create that folder, which we can do with the `os.makedirs()` function:

``` python
if not os.path.exists(dir_path):
    os.makedirs(dir_path)
```

Lastly, as we do with the `ls` command on the terminal, we can use `os.listdir()` to get the contents of whatever path we provide as an argument.

``` python
os.listdir(cwd)
```

From there, we can move onto working with zipfiles!


### 4.2 ZipFile

The `zipfile` module is a powerful module that allows us to extract files from a zipped folder. First, we import the needed modules and assign the name of the zipfile we'll be extracting to a variable:
``` python
import zipfile
import os
zip_name = 'example.zip'
```

Next, we get the current directory path and join it with the zipfile name to get its exact path:

``` python
cwd = os.getcwd()
zip_path = os.path.join(cwd, zip_name)
```

Next, we use the ZipFile class to change the file to a Zipfile object. With this object, we use the `ZipFile.extract()` function to extract all the contents:
``` python
with zipfile.ZipFile(zip_path, 'r') as z:
	z.extractall(cwd)
```

And lastly, we can see what's in the zipfile with: 

``` python
os.listdir(dir_path)
```

## 5.0 Data Merging

Now we'll head into the data merging portion of Data Science. We'll begin by initializing two dataframes to work with: 

``` python
from pandas import DataFrame
df1 = DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1': range(7)})
df2 = DataFrame({'key': ['a', 'b', 'd'], 'data2': range(3)})
```

This is an example of a many-to-one merge situation; the data in df1 has multiple rows labeled a and b, whereas df2 has only one row for each value in the key column. Calling merge with these objects we obtain:

``` python
pd.merge(df1, df2)
```
Note that I didn’t specify which column to join on. If not specified, merge uses the overlapping column names as the keys. It’s a good practice to specify explicitly, though:

``` python
pd.merge(df1, df2, on='key')
```

If the column names are different in each object, you can specify them separately:

``` python
df3 = DataFrame({'lkey': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1': range(7)})
```

You probably noticed that the 'c' and 'd' values and associated data are missing from the result. By default merge does an 'inner' join; the keys in the result are the intersection. Other possible options are 'left', 'right', and 'outer'. The outer join takes the union of the keys, combining the effect of applying both left and right joins:

``` python
pd.merge(df1, df2, how='outer')
```

### 5.1 Merging on Index

In some cases, the merge key or keys in a DataFrame will be found in its index. In this case, you can pass `left_index=True` or `right_index=True` (or both) to indicate that the index should be used as the merge key:

``` python
left1 = DataFrame({'key': ['a', 'b', 'a', 'a', 'b', 'c'], 'value': range(6)})
right1 = DataFrame({'group_val': [3.5, 7]}, index=['a', 'b'])
```

Since the default merge method is to intersect the join keys, you can instead form the union of them with an outer join:

``` python
pd.merge(left1, right1, left_on='key', right_index=True, how='outer')
```

## 6.0 Final Words

That wraps up the Data Preparation portion of this course. Next we'll go into Data Cleaning and Munging, where we deal with messy and inaccurate data.

### 6.2 Mini Courses

Learn about courses [here](www.byteacademy.co/all-courses/data-science-mini-courses/).

[Python 101: Data Science Prep](https://www.eventbrite.com/e/python-101-data-science-prep-tickets-30980459388) <br>
[Intro to Data Science & Stats with R](https://www.eventbrite.com/e/data-sci-109-intro-to-data-science-statistics-using-r-tickets-30908877284) <br>
[Data Acquisition Using Python & R](https://www.eventbrite.com/e/data-sci-203-data-acquisition-using-python-r-tickets-30980705123) <br>
[Data Visualization with Python](https://www.eventbrite.com/e/data-sci-201-data-visualization-with-python-tickets-30980827489) <br>
[Fundamentals of Machine Learning and Regression Analysis](https://www.eventbrite.com/e/data-sci-209-fundamentals-of-machine-learning-and-regression-analysis-tickets-30980917759) <br>
[Natural Language Processing with Data Science](https://www.eventbrite.com/e/data-sci-210-natural-language-processing-with-data-science-tickets-30981006023) <br>
[Machine Learning with Data Science](https://www.eventbrite.com/e/data-sci-309-machine-learning-with-data-science-tickets-30981154467) <br>
[Databases & Big Data](https://www.eventbrite.com/e/data-sci-303-databases-big-data-tickets-30981182551) <br>
[Deep Learning with Data Science](https://www.eventbrite.com/e/data-sci-403-deep-learning-with-data-science-tickets-30981221668) <br>
[Data Sci 500: Projects](https://www.eventbrite.com/e/data-sci-500-projects-tickets-30981330995)

