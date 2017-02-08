Data Preparation with Python & R
==================

Brought to you by [Lesley Cordero](http://www.columbia.edu/~lc2958) and [ADI](https://adicu.com)

## Table of Contents

- [0.0 Setup](#00-setup)
	+ [0.1 Python & Pip](#01-python--pip)
	+ [0.2 R & R Studio](#02-r--r-studio)
	+ [0.3 Other](#03-other)
- [1.0 Background](#10-background)

- [2.0 Data Science Process](#20-data-science-process)
    
- [3.0 Important Concepts](#30-important-concepts)

- [4.0 Tackling a Data Problem](#40-tackling-a-data-problem)

- [5.0 Resources](#50-resources)


## 0.0 Setup

This guide was written in Python 3.5 and R 3.2.3.

### 0.1 Python & Pip

Download [Python](https://www.python.org/downloads/) and [Pip](https://pip.pypa.io/en/stable/installing/).

### 0.2 R & R Studio

Install [R](https://www.r-project.org/) and [R Studio](https://www.rstudio.com/products/rstudio/download/).

### 0.3 Other

We'll soon get into the difference between packages in R and modules in Python. For now, let's install the ones we'll need for this tutorial. Open up your terminal and enter the following commands to install the needed python modules: 

```
pip3 install 
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


### CSVs with R 

for row in data_table.find_all("tr"):
    cells = row.find_all("td")
    # Ignore lines that don't have 3 cells of data because it is just spacing
    if len(cells) == 3:
        line = (str.strip(cells[0].text), convert_num(cells[2].text))
        # Once we get to the total line we start getting the funding lines
        if line[0] == "TOTAL":
            funding = True
            # We don't want to capture the total because we can calc it
            continue
        if funding:
            funding_lines[line[0]].append(line[1])
        else:
            expense_lines[line[0]].append(line[1])

## 3.0 Data Preparation

### Pandas

Pandas allows us to deal with data in a way that us humans can understand it - with labelled columns and indexes. It allows us to effortlessly import data from files such as CSVs, allows us to quickly apply complex transformations and filters to our data and much more. Along with Numpy and Matplotlib, it helps create a really strong base for data exploration and analysis in Python. 

``` python
import pandas as pd 
```

#### Data Types

Pandas is based around two data types, the series and the dataframe. A series is a one-dimensional data type where each element is labelled. A dataframe is a two-dimensional, tabular data structure. The Pandas dataframe can store many different data types and each axis is labelled. 


### 2.1 dplyr

dplyr allows us to transform and summarize tabular data with rows and columns. It contains a set of functions that perform common data manipulation operations like filtering rows, selecting specific columns, re-ordering rows, adding new columns, and summarizing data.

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

#### 2.1.1 select()

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

#### 2.1.2 filter()

Using the `filter()` method in dplyr we can select rows that meet a certain criterion, such as in the following:

``` R
filter(msleep, sleep_total >= 16)
```
There, we filter out the animals whose sleep total is less than 16 hours. If you want to expand the criteria, you can: 

```R
filter(msleep, sleep_total >= 16, bodywt >= 1)
```

#### 2.1.3 Pipe Operator

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

#### 2.1.4 Arrange

To re-order rows by a particular column, you can list the name of the column you want to arrange the rows by: 

``` R
msleep %>% arrange(order) %>% head
```


#### 2.1.5 Functions

`arrange()`: re-order or arrange rows <br>
`filter()`: filter rows <br>
`group_by()`: allows for group operations in the “split-apply-combine” concept <br>
`mutate()`: create new columns <br>
`select()`: select columns <br>
`summarise()`: summarise values



## 5.0 Final Words

### 5.1 Resources
