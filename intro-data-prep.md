Data Preparation with Python & R
==================

Brought to you by [Lesley Cordero](http://www.columbia.edu/~lc2958) and [ADI](https://adicu.com)

## Table of Contents

- [0.0 Setup](#00-setup)
	+ [0.1 Python & Pip](#01-python--pip)
	+ [0.2 R & R Studio](#02-r--r-studio)
	+ [0.3 Other](#03-other)
- [1.0 Background](#10-background)
	+ [1.1 What is Data Science?](#11-what-is-data-science)
		* [1.1.1 What do you mean by data?](#111-what-do-you-mean-by-data)
	+ [1.2 Is data science the same as machine learning?](#12-is-data-science-the-same-as-machine-learning)
	+ [1.3 Why is Data Science important?](#13-why-is-data-science-important)
	+ [1.4 Data Roles](#14-data-roles)
		* [1.4.1 Data Analysis](#141-data-analyst)
		* [1.4.2 Data Engineer](#142-data-engineer)
		* [1.4.3 Data Scientist](#143-data-scientist)
- [2.0 Data Science Process](#20-data-science-process)
	+ [2.1 What is a "Data Science" Problem?](#21-what-is-a-data-science-problem)
	+ [2.2 ...So where do I begin?](#22-so-where-do-i-begin)
	+ [2.3 Given Problem](#23-given-problem)
		* [2.3.1 Open Data](#231-open-data)
	+ [2.4 Given Data](#24-given-data)    
- [3.0 Important Concepts](#30-important-concepts)
	+ [3.1 Machine Learning](#31-machine-learning)
		* [3.1.1 Supervised Learning](#311-supervised-learning)
		* [3.1.2 Unsupervised Learning](#312-unsupervised-learning)
		* [3.1.3 Reinforcement Learning](#313-reinforcement-learning)
- [4.0 Tackling a Data Problem](#40-tackling-a-data-problem)
	+ [4.1 Data Preparation](#41-data-preparation)
	+ [4.2 Data Cleaning](#42-data-cleaning)
	+ [4.3 Data Analysis](#43-data-analysis)
		* [4.4.1 Basics](#431-basics)
		* [4.4.2 Time Series Analysis](#432-time-series-analysis)
		* [4.4.3 Deep Learning](#433-deep-learning)
		* [4.4.4 Natural Language Processing](#434-natural-language-processing)
- [5.0 Resources](#50-resources)


## 0.0 Setup

This guide was written in Python 3.5 and R 3.2.3.

### 0.1 Python & Pip

Download [Python](https://www.python.org/downloads/) and [Pip](https://pip.pypa.io/en/stable/installing/).

### 0.2 R & R Studio

Install [R](https://www.r-project.org/) and [R Studio](https://www.rstudio.com/products/rstudio/download/).

### 0.3 Other

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
nano matplotlibrc
```
And then, write `backend: TkAgg` in the file. Now you should be set up with your virtual environment!

Cool, now we're ready to start! 

## 1.0 Introduction


### CSVs with R 


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

#### 2.1.3 Functions

`arrange()`: re-order or arrange rows <br>
`filter()`: filter rows <br>
`group_by()`: allows for group operations in the “split-apply-combine” concept <br>
`mutate()`: create new columns <br>
`select()`: select columns <br>
`summarise()`: summarise values




## 5.0 Final Words

### 5.1 Resources
