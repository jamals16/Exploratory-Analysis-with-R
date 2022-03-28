---
title: "Exploring and working with Datasets"
author: "Shaila"
date: "07/02/2022"
output: pdf_document
---

At the beginning, let's clear the environment with the following codes:

```{r}
rm(list=ls())
```


## Installing/ loading packages:

For this exercise, we will use package 'tidyverse'. Install it into your computer using the following code:

```{r}
library(tidyverse)
```


## Uploading/ Importing datasets

We will be working on four dataset (or object) that contain information on traffic accidents of Toronto from 2015 - 2018. To learn more about this dataset, check document in this link (Page 12-15): https://data.torontopolice.on.ca/pages/ksi

Let's upload/ import the dataset

```{r}
ksi_2015 <- read.csv("2015_KSI.csv")
ksi_2016 <- read.csv("2016_KSI.csv")
ksi_2017 <- read.csv("2017_KSI.csv")
ksi_2018 <- read.csv("2018_KSI.csv")
```

Note: 

You may need to delete the first row in the csv. file as those were blank and R is unable to read the column names as they were not first column.

## Tasks: 

1. Create a single data set that can be used to output a table that lists the number of persons injured or killed in traffic accidents in each neighborhoods of Toronto in the last 4 years. (Table similar to the assignment).

2. Write a sequence of code that shows the total vehicles in the street for each district for the last 4 years during the accidents. (Table similar to the assignment).

3. Write a sequence of code that lists the top 5 neighborhoods with the highest average number of vehicles in the streets. (Table similar to the assignment).



## Step 1: 

Need to identify the required columns to perform the task. Let's start with joining the dataset. 

At first, check the dataset for the columns. What we need? Year, vehicles in th street, district and Neighborhood. 

```{r}
ksi_2015_1 <- select(ksi_2015, YEAR, DISTRICT, NEIGHBOURHOOD, VEHICLES_IN_STREET) # another way of doing it is by specifying column numbers: ksi_2015_1 <- ksi_2015[, c(5, 13, 56, 58)]
ksi_2016_1 <- select(ksi_2016, YEAR, DISTRICT, NEIGHBOUR, VEHICLE_STREET) 
ksi_2017_1 <- select(ksi_2017, YEAR, DISTRICT, NEIGHBOURHOOD, VEHICLE_IN_STREET)
ksi_2018_1 <- select(ksi_2018, YEAR, DISTRICT, NEIGHBOURHOOD, VEHICLE_IN_STREET)
```

Note: sometimes, column names may mismatch between datasets. In those cases, you need to give all columns same names before joining them into one dataset.

Now, we will be changing some of the column names.showing examples using both names() and rename() function

```{r}
names(ksi_2016_1)[3] <- "NEIGHBOURHOOD"

ksi_2015_1 <- rename(ksi_2015_1, VEHICLE_IN_STREET = VEHICLES_IN_STREET)
ksi_2016_1 <- rename(ksi_2016_1, VEHICLE_IN_STREET = VEHICLE_STREET)
```

Look into the four dataset. All of the column names are same. 

Here is the code for turning the 4 dataset into one dataset using rbind() function

```{r}
data_1 <- rbind(ksi_2015_1, ksi_2016_1, ksi_2017_1, ksi_2018_1)
```

Now check the data summary or data type:

```{r}
summary(data_1)

str(data_1)
```


In this dataset, numerical varialbes are in numeric/ integer type. Remember if they are not in numeric type you need to convert you variable into  numeric data using as.numeric() function. 


## Creating a single data set that can be used to output a table that lists the number of persons injured or killed in traffic accidents in each neighborhoods of Toronto in the last 4 years

```{r}
# Creating the Table containing the number of libraries by city and year. 

library(janitor) # install the package before loading it as a library

Table <- tabyl(data_1, NEIGHBOURHOOD, YEAR)

print(Table)
```

Note: before doing the other two tasks, familiarize yourself with the aggregate() function and how it works.

## Creating a sequence of code that shows the total vehicles in the street for each district for the last 4 years during the accidents. (Table similar to the assignment 2)

```{r}
# aggregating data/ grouping data

Table1 <- aggregate(data_1$VEHICLE_IN_STREET, by = list(data_1$DISTRICT, data_1$YEAR), FUN = sum)
```

Note: For the ease of understanding, you can change the column names using names() or rename() function

Converting into final table

```{r}
Table2 <- spread(Table1, Group.2, x, fill = NA, convert = FALSE)
```


## Creating a sequence of code that lists the top 5 neighborhoods with the highest average number of vehicles in the streets. (Table similar to the assignment


```{r}
Table3 <- aggregate(data_1$VEHICLE_IN_STREET, by = list(data_1$NEIGHBOURHOOD, data_1$YEAR), FUN = sum)

# Converting into final table

Table4 <- spread(Table3, Group.2, x, fill = NA, convert = FALSE)
```


```{r}
# creating another column that shows the mean/ average of number of vehicles

Table4$Average <- rowMeans(Table4[,2:5])
```


```{r}
# Table with the top 5 libraries 

# Ordering the dataset

Table5 <- Table4[order(Table4$Average, decreasing = TRUE), ]

Table5 <- Table4[1:5, ]
```


### note: teahc the clients on gsub() for removing the commas. 
## how to deal with NA or missing values. 