---
title: "Creating Plots Part 2"
author: "Shaila"
date: "22/02/2022"
output: html_document
---


In this workshop, you will learn about joining multiple datasets, and further application of R package 'ggplot2'.
Please remember, we will be going to learn very few of R codes and its' applicability. You are encouraged to explore the different functions, codes in R and their application for your own development/ learning.

2.1: As I mentioned, we will be working with multiple datasets/ data tables for this lab. Therefore it is important that we learn about joining different datasets.

We will be using datasets of Human Development Report 2015 from the United Nations Development Programs: https://www.kaggle.com/undp/human-development

Please spare 2-5 minutes and read the content from the website above to get a brief idea about dataset so that you have no confusion about the variables in the csv files.

Download all the dataset from Lab 5 module. You may need all of them or not for this lab, but that is something you to decide while answering questions in the later sections of this lab :) . That's why you need to read the content and explore all of the datasets.

Note that it's always good to clear the environment in the right before start working. You can click in the broom in the environment or use "Ctrl +L" or run the following codes:

```{r}
rm(list=ls())
```

Now, load the required packages for this lab 05.

```{r}
library(tidyverse)
library(ggplot2)
```

Now let's upload/ import the 'gender_inequality' and 'gender_development datasets'. I uploaded them by specifying the paths. But you can use other ways to import datasets.

```{r}
data_1 <- read.csv("C:/Users/shail/Downloads/Lab 5 Datasets/gender_development.csv")

data_2 <- read.csv("C:/Users/shail/Downloads/Lab 5 Datasets/gender_inequality.csv")
```

Note: so far, all the datasets we used in the workshops are in csv format. Therefore, we are using read.csv(). If you want to work on a dataset that is excel or in other formats, the read.csv() function won't work for importing your data. So always check for the format of your dataset and then use appropriate functions for importing the data in RStudio.


2.2: If you look into the two datasets, you will see that both datasets contain information on key development indicators of different countries. One showing values of the measure regarding gender development and another one regarding gender inequality.
Suppose, we want to explore the relationship between estimated mean years of education by the females (an information variable/column that belongs to data_1) and labour force participation rate by the females (a variable/ column that belongs to data_2).

How are you going to explore this? Before choosing the method, you need to put both variables in the same dataset.

There should be a common column based on which you will be joining datasets. Check the class lecture on database and datasets where I talked about them.
A common column between these datasets is 'Country'. Therefore, we will be joining the datasets by using merge() function and country as a common ID or column [check this resource: https://www.statmethods.net/management/merging.html]. Let's call the joined datasets data_3.

```{r}
data_3 <- merge(data_1, data_2, by = "Country")
```

Explore data_3. you will see that all columns of data_1 and data_2 has been joined by the respective country name.

2.3: Do it yourself: Let's create a scatter plot that shows mean years of education by the females vs labour force participation rate by the females. Both information are now available on data_3.

Like previous lab, make it a good example of data visualization. Label the dots/ points with country names and ensure proper visibility of the names (by changing the size of the texts, size of the graph, etc.).

Hint : Look for functions in google as you might need something extra for your visualization purpose. Check this helpful resource: https://r-graphics.org/recipe-scatter-labels

Note that you might need to install another package to ensure proper visibility.

Helpful tips (perhaps): In many cases, when we import data from csv or excel or other formats, the columns names in RStudio usually got changed. The reason has been explained in Lab 1. Variables and vectors in R can't named with a space. Therefore, while converting/ omitting spaces from other formats, variables names got changed in R.
One thing I always do is run the names() functions so that I get the exact column names in datasets imported in R. For example, running names(data_3) will give me the column names in R in the console. I can check the exact column name and paste in my codes where needed. Something I found very useful in avoiding spelling mistakes. Please note that in the console, the variable names are within the inverted comma and does not include the number in the left. If you have doubt and any error is showing, please cross check with the dataset. Most of the time it's an error in copying and pasting.

However, before creating graphs, you need to check the data type of your variables. Remember using str() function? Run it and see the data type in data_3. Write down the codes for it.

```{r}
str(data_3)
```

As we can see that almost all variables are character including the two variables of our interest. We need to convert them as numeric variables. We will be using as.numeric() function for that.

```{r}
data_3$Mean.Years.of.Education..Female. <- as.numeric(data_3$Mean.Years.of.Education..Female.)

data_3$Labour.Force.Participation.Rate..Female. <- as.numeric(data_3$Labour.Force.Participation.Rate..Female.)
```

Note: Ignore the warning message unless it's an error message. Also, there is a possibility (although very rare) that due to conversion into R, the variables names in my RStudio and your Rstudio may vary. For example, you may have "..." instead of ".". So check the variable names of yours.


2.4: This is actually where you are going to put your codes for the scatter plots and also paste the scatter plot you created. Go through the instruction in 5.3 properly and then create the graph.

We used 'ggrepel' to ensure proper visibility. Check the resource link posted above. 

```{r}
library(ggrepel)
```

```{r}
plot_1 <- ggplot(data_3, aes(x=Mean.Years.of.Education..Female., y= Labour.Force.Participation.Rate..Female.)) +
  geom_point() +
  labs(title= "Relationship between Mean Education rate of females vs Labour force participation rate by females",
       x="Mean years of Education by Females ", y = "Labour Force participation by Females") +
  # geom_text(aes(label = Country), size = 3) +

  geom_text_repel(aes(label = Country), size = 3)

print(plot_1)
```


2.5: Do it yourself: As in this course we are emphasizing more on data analysis and interpretation of the results, can you explain what most interesting thing you can see from the graph? Anything interesting? Write maximum 100 words.

2.6: Do it yourself: From data_3, find out the average estimated gross national income per capital by females in the world.

To calculate the average, our data type need to be numeric. Here is the code (same as previous):

```{r}
data_3$Estimated.Gross.National.Income.per.Capita..Female. <-
  as.numeric(data_3$Estimated.Gross.National.Income.per.Capita..Female.)
```

Due to missing values, sometimes our calculations show errors in R. Therefore, we will delete the missing values from data_3 using the following codes:

```{r}
data_3 <- drop_na(data_3)
```

Look at data_3 now: it only deleted rows that have missing values in numeric data.


2.7: Now write down the codes and answer for calculating the average estimated gross national income per capital by females in the world.

```{r}
mean(data_3$Estimated.Gross.National.Income.per.Capita..Female.)
```


2.8: Now, we will create a column in data_3 called "Per_capita_income_range_female" and insert values as follows:

- When estimated gross national income per capital by females is less than 5000, the column will show "less than 5000",
- when it is between 5000.1 to 10,000', it will show "between 5000-10000",
- when it is between 10,000.1 to 15,000, it will show "between 10000 - 15000"...it will follow the same interval till 40,000 and then the last one with above 40000.1 will be "above 40000". Here is the codes for that:

replace() can be very helpful in replacing values. Learn/ search more about them if you have confusion. You can use other functions if you like.

Pay attention to the ">", "<" and "&" operators used in this function.
It looks like a lot of codes, but they are almost same line of codes.

```{r}
data_3$Per_capita_income_range_female <- replace(
  data_3$Per_capita_income_range_female,
  data_3$Estimated.Gross.National.Income.per.Capita..Female.< 5000, "less than 5000")


data_3$Per_capita_income_range_female <- replace(
  data_3$Per_capita_income_range_female,
  data_3$Estimated.Gross.National.Income.per.Capita..Female. > 5000
  & data_3$Estimated.Gross.National.Income.per.Capita..Female.< 10000,
  "between 5000-10000")

data_3$Per_capita_income_range_female <- replace(
  data_3$Per_capita_income_range_female,
  data_3$Estimated.Gross.National.Income.per.Capita..Female. > 10000
  & data_3$Estimated.Gross.National.Income.per.Capita..Female.< 15000,
  "between 10000-15000")


data_3$Per_capita_income_range_female <- replace(
  data_3$Per_capita_income_range_female,
  data_3$Estimated.Gross.National.Income.per.Capita..Female. > 15000
  & data_3$Estimated.Gross.National.Income.per.Capita..Female.< 20000,
  "between 15000-20000")

data_3$Per_capita_income_range_female <- replace(
  data_3$Per_capita_income_range_female,
  data_3$Estimated.Gross.National.Income.per.Capita..Female. > 20000
  & data_3$Estimated.Gross.National.Income.per.Capita..Female.< 25000,
  "between 20000-25000")

data_3$Per_capita_income_range_female <- replace(
  data_3$Per_capita_income_range_female,
  data_3$Estimated.Gross.National.Income.per.Capita..Female. > 25000
  & data_3$Estimated.Gross.National.Income.per.Capita..Female.< 30000,
  "between 25000-30000")

data_3$Per_capita_income_range_female <- replace(
  data_3$Per_capita_income_range_female,
  data_3$Estimated.Gross.National.Income.per.Capita..Female. > 30000
  & data_3$Estimated.Gross.National.Income.per.Capita..Female.< 35000,
  "between 30000-35000")

data_3$Per_capita_income_range_female <- replace(
  data_3$Per_capita_income_range_female,
  data_3$Estimated.Gross.National.Income.per.Capita..Female. > 35000
  & data_3$Estimated.Gross.National.Income.per.Capita..Female.< 40000,
  "between 35000-40000")

data_3$Per_capita_income_range_female <- replace(
  data_3$Per_capita_income_range_female,
  data_3$Estimated.Gross.National.Income.per.Capita..Female. > 40000, "above 40000")
```

2.9: Do it yourself: Now you need to create a barplot with the variable "Per_capita_income_range_female" from the data_3 that showing the counts/ frequency/ number of countries that fall within those ranges.
As expected (it will be always expected), follow proper visualization strategies, write your codes and then paste the graph.

If you are following the exact codes we used in the previous workshop, most probably your bar plot will be showing in unordered form as those values are character type.
Character vectors/ variables cannot be used in most statistical functions in R, since they don't define groups.

If you want to show your ranges in the bar plot in an ordered form, you need to convert those character values into an ordered form. Before ordering your values, you need to convert it into a factor variable as values there are not numerical.

```{r}
data_3$Per_capita_income_range_female <- as.factor(data_3$Per_capita_income_range_female)
```

check the data type using str() function to check whether the data has been converted into a factor variable.

Now let's order the variable using order() by specifying the order levels using levels():

```{r}
data_3$Per_capita_income_range_female <- ordered(data_3$Per_capita_income_range_female,
                                                 levels = c("less than 5000", "between 5000-10000", "between 10000-15000",
                                                 "between 15000-20000", "between 20000-25000",
                                                 "between 25000-30000", "between 30000-35000",
                                                 "between 35000-40000", "above 40000"))
```

2.10: Do it yourself: Now create the bar plot, write your codes, paste you graph and interpret the graph.

2.11: What is your understanding from this bar graph? max 50 words.

2.12: Now upload the 'historical_index" as data_4.

```{r}
data_4 <- read.csv("C:/Users/shail/Downloads/Lab 5 Datasets/historical_index.csv")
```


2.13: Check the variable type of the dataset. Write down the code for that.

```{r}
str(data_4)
```

2.14: Variables that are other than numeric, need to be converted as numeric as they are not categorical variable. Please do that here (write the codes)

```{r}
data_4$Human.Development.Index..1990.<- as.numeric(data_4$Human.Development.Index..1990.)
data_4$Human.Development.Index..2000. <- as.numeric(data_4$Human.Development.Index..2000.)
```

2.15: Now drop the NA values. Write the codes.

```{r}
data_4 <- drop_na(data_4)
```

2.16: Create a column called 'Difference_Calculation' in data_4 which shows the change in HDI (Human Development Index) from 1990 to 2014.

```{r}
data_4$Difference_Calculation <- data_4$Human.Development.Index..2014 - data_4$Human.Development.Index..1990.
```

2.17: Plot the data (choose the type of graph that you think most appropriate to visualize the changes in HDI), and then discuss the results. Remember to make it an example of good visualization.
In your discussion, along with other information that you think are most informative, you must mention which countries have highest and lowest increase and decrease in HDI? Explain in 1-2 sentences about how you came to learn this from your graph (you need to explain this so that we could know that you are actually interpreting the data from the graph not directly from the table).

write the codes, paste the plot and then write the discussion.

```{r}
plot_2 <- ggplot(data_4, aes(y=Country, x=Difference_Calculation)) +
  geom_bar(stat = "identity", fill = "blue", width = 0.2) +
  labs(title= "Counts of Airbnb Properties by Neighbourhoods in Amsterdam",
       x="Number of Properties", y = "Names of the Neighbourhood in Amsterdam")

print(plot_2)
```


2.18: Create another plot (choose what you think most appropriate with appropriate visuals) that plots the changes in HDI and Labour force participation rate by the females. Explain your graph. Do you see anything surprising?

```{r}
data_5 <- merge(data_2, data_4, by = "Country")

data_5 <- drop_na(data_5)

str(data_5)

data_5$Labour.Force.Participation.Rate..Female. <- as.numeric(data_5$Labour.Force.Participation.Rate..Female.)
data_5$Difference_Calculation <- as.numeric(data_5$Difference_Calculation)

data_5$Labour.Force.Participation.Rate..Female. <- unlist(data_5$Labour.Force.Participation.Rate..Female.)

graphname3 <- ggplot(data_5, aes(y=Difference_Calculation, 
                               x=Labour.Force.Participation.Rate..Female.)) +
  geom_point() + 
  geom_text_repel(aes(aes(label = Country), size=3))


print(graphname3)
```

This the end of the workshop. I tried to give you an understanding on data visualization through graphs and what you need to do to prepare your data. Most cases, it takes more effort in preparing the data rather than to run the code for visualization. Please remember, you visualization will be ineffective if you can't interpret them. Interpretation and result discussion are very important in academic and research life.

As you can see, I haven't even touched line graphs and other types of visualizations. This is because of the time constraints. I wish I could show you more. However, feel free to explore resources on other types of visualizations and their interpretations.

