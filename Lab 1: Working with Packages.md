---
title: "Working with Packages"
author: "Shaila Jamal"
date: "31/01/2022"
output: pdf_document
---

## Installing/ loading packages:

Before we start working with different functions, we should install required packages to conduct our intended work. In simple words, 'R' packages are a bundle of R functions stored as a package. There are numerous packages in 'R' and programmers use packages that suits their work.

For this exercise, we will use package 'tidyverse'. Install it into your computer using the following code:

```{r eval = FALSE}
install.packages("tidyverse")
```

Once you install the package for the first time, you need to load it as a library by using the following code.

```{r}
library(tidyverse)
```

Note: you don't need to install it each time you work in R. After your first installation, you can just load it as a library in your R session using the above code.

Here in this exercise, we will go through some more basics on RStudio. This time, we will work with a dataset.


##Loading/ Importing a dataset using 'R' functions:

How to import a csv. file into R [https://datatofish.com/import-csv-r/ ]

We will be running the following codes to load/ import the dataset and give it a name 'data'.

```{r}
data <- read.csv("C:/Users/shail/Downloads/Lab02Data.csv")
```

If you keep both of the .Rmd file and the .csv file in the same folder, you can use the following code to import your data in the R environment:

```{r}
data <- read.csv("Lab02Data.csv")
```

We used 'read.csv()' as our data file is in csv format. There are other functions such 'read.excel()' , 'read.text()', etc. depending on the file type you are loading.

Once we import our data, we can check the dataset in the 'Environment'.
In case you need, here is a quick tour of RStudio [https://www.r-bloggers.com/2018/08/a-tour-of-rstudio/]
In the 'Environment', you can see that the dataset contains 31 observations with 4 variables. Click on the 'data' in the 'Environment and it will show you the data table in a separate tab.

## Deleting a variable/ column in a data table:

Now let's delete column 'ID' (column number 1) from the dataset. By creating a subset, we can use the following code and store the data in a new dataset called 'new_data':

```{r}
new_data <- data[2:4]
```


Alternatively, you can use: new_data <- data[-c(1)]. With the minus '-' sign, you are specifying deletion and by c(1), you are indicating that it is column 1 that you want to delete.


## Renaming a column in a dataset:

Let's change the name of the variable 'avg_T_c' into 'Temperature_in_celsius' by using function rename(). Remember, there shouldn't be any space between letters in a variable name.

```{r}
new_data <- rename(new_data, Temperature_in_celsius = avg_T_C)
```


Now, rename 'precip_mm' as 'Precipitation_in_mm' (without quotation '') in the data.

```{r}
new_data <- rename(new_data, Precipitation_in_mm = precip_mm)
```


Note: Check your dataset by clicking it from the "Environment" to see whether it was created.


## Sorting/ Ordering a dataset in ascending or descending order:

Use the order( ) function for sorting data. By default, in order() function, sorting is ASCENDING. You need to use a minus sign to indicate DESCENDING order. Helpful resource: [https://www.guru99.com/r-sort-data-frame.html]

We are ordering the data in ascending according to the temperature:

```{r}
new_data <- new_data[order(new_data$Temperature_in_celsius), ]
```


We use the '$' sign to specify the column/ variable in a dataset. Here our data is 'new_data' and 'Temperature_in_celsius' is the column/ variable name.

Note: you can save this ordered data into another table.

Order the data in DESCENDING by only Precipitation_in_mm and store it in a new dataset called 'new_data_2'.

```{r}
new_data_2 <- new_data[order(-new_data$Precipitation_in_mm), ]
```


## Adding a new variable/ column in the dataset and doing calculation

You can add a column to the data table using '$' as well. Let's say, we want to create a variable called 'Random' in the dataset new_data_2 and fill it with the value that show the temperature in celsius multiplied by 9. Here will be the code:

```{r}
new_data_2$Random <- new_data_2$Temperature_in_celsius*9
```


Add another variable/ column called 'Random_2' (without quatation '') in the new_data_2 and store it with values containing precipitation in mm multiplied by a number of your choice (I choose 400220927).

```{r}
new_data_2$Random_2 <- new_data_2$Precipitation_in_mm*400220927
```


## What was the highest and lowest temperature and precipitation recorded in the dataset?:

```{r}
min(new_data_2$Temperature_in_celsius)
max(new_data_2$Temperature_in_celsius)
min(new_data_2$Precipitation_in_mm)
max(new_data_2$Precipitation_in_mm)
```

## Creating variables/ columns based on conditions:

Now, we will learn how to use function 'ifelse()' [for more details, check: https://www.datacamp.com/community/tutorials/if-else-function-r].

The syntax for ifelse statement is: ifelse(logical_expression, a , b)

The argument above in 'ifelse' states that:
logical_expression: Indicates an input vector, which in turn will return the vector of the same size as output.
a: Executes when the logical_expression is TRUE.
b: Executes when the logical_expression is FALSE.

To check which number is odd and which number is even in the 'Random_2' variable, we will use 'ifelse()' function and store the output in a variable/ column called 'Odd_Numbers'.

```{r}
new_data_2$Odd_Numbers <- ifelse(new_data_2$Random_2 %% 2 == 1, "Odd Number", "Even Number")
```

Check the 'new_data_2' in the 'Environment'. After running the above code, a new column called 'Odd_Numbers' have been created and values are showing according to odd numbers in 'Random_2'.

If you don't know what is '%%', then do a google search on '%%' operator in R and let me know what it is. 

Similarly, you can create a column showing which temperature is above zero. Let's assume the column is 'Plus_temperature'. The codes will be:

```{r}
new_data_2$Plus_temperature <- ifelse(new_data_2$Temperature_in_celsius > 0, "Above zero", "zero or below zero")
```

Check the dataset to see your results to confirm that your worked.

## Using function 'nrow' to count data based on conditions.

[Check different use of function 'nrow' in R: https://statisticsglobe.com/nrow-r-function/]

Let's assume that you want to count up the days that has temperature above 0. We can use the following code:

```{r}
nrow(new_data_2[new_data_2$Temperature_in_celsius > 0, ])
```


Check the console. Your results will be showing there. 

## As a final step to for today's practice, do the following using 'new_data_2':

1. Insert a new column titled "Above 3" and use the examples above to create a new variable in this column, where there is a value of "1" when Temperature in Celsius is above 3, and "0" otherwise.

2. Count up the number of days with temperature below 3 celsius.

3. Count up the number of days with precipitation 10 mm and above.

4. Add a variable called "Precipitation" and code this variable so that it is a value of "TRUE" when there is precipitation, and "FALSE" when there is not.


---------------------------


