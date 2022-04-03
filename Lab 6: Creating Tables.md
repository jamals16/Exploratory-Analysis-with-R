---
title: "Creating Tables"
author: "Shaila"
date: "27/02/2022"
output: html_document
---

1.1: We will be learning on creating cross tabulations and their interpretation.(Note: some of reseources here are already at the last section of the creating plots part 3 file)

At the beginning, clear the environment

```{r}
rm(list=ls())
```

Now, load the required packages.

```{r}
library(tidyverse)
```


These are required reading for this workshop:
1. https://www.surveyking.com/help/cross-tabulation-analysis
2. https://www.qualtrics.com/experience-management/research/cross-tabulation/ (we will learn about the chi-square analysis later, but still it is good to go through everything here)
3. https://libraryguides.mcgill.ca/c.php?g=699776&p=4968551
4. https://www.statmethods.net/stats/frequencies.html

Feel free to do some google search if some terms seem unknown to you.

We will be using a different dataset this time - it is a part of response table (contains only a limited number of questions) from a quantitative survey conducted in Bangladesh between June-Aug 2020 labled as Lab06_data.csv)

Let's load the data. Use your own codes with which your are comfortable. 

```{r}
data_2 <- read.csv("C:/Users/shail/Downloads/Lab06_data.csv")
```

For your understanding, I kept the exact questions names as column names so that you can have an idea about the questions asked in the survey. Let's look into the columns.

```{r}
str(data_2)
```

This dataset contains only a few of the public opinions on COVID-19 and respondents' socio-demographic information. As the column names are very long, let's change the names of the variables we will be using in this exercise. This time, we will be using the names() function. You can use the rename() function if you like.
While using the names() function, we can just specify the column number of the variable from the dataset. Let's say we want to change the name of the column that asked about the frequency of social media checking regarding COVID-19 news, which is column number 2. We will rename it to 'Social_media_check'. Here is the code for that:

```{r}
names(data_2) [2] <- "Social_media_check"
```

Check this resource about using names() function and possible errors that could happen.
https://stackoverflow.com/questions/6081439/changing-column-names-of-a-data-frame

Now we will be changing the names of the columns that contains information on birth year, gender, income, living_arrangement and occupation. If you look into the dataset, and put your cursor over the corresponding column, it will show you the column number. These column numbers are 12, 13, 14, 15 and 16 respectively. Here is the code how you change multiple column names using names() function.

```{r}
names(data_2) [12:16] <- c("Year", "Gender", "Income", "Living_arrangement", "Occupation")
```


1.2: Now create a column called 'Age' in data_2 that shows the age of the respondents during the survey year (which is 2020). 

```{r}
data_2$Age <- 2020 - data_2$Year
```

1.3: As you can see from the data type (we used str() function to see that), gender, income, living_arrangement and occupation are character type, you need to convert them into factor variables to the cross tabulation analysis. Let's do that. 

```{r}
data_2$Gender <- as.factor(data_2$Gender)
data_2$Income <- as.factor(data_2$Income)
```

As there is a rank/ order associated with income data, we will convert it into ordered form which the options/ choices available.
We can use the summary() to see the choices/ options reported in questions/ variable. It shows us the frequency of options/ choices mentioned by the respondents for the income variable. I just used it to check the available options. To avoid spelling mistakes, you can just copy paste the options from here to use in your codes. 

```{r}
summary(data_2$Income)
```


Now ordering the choices with the following codes.

```{r}
data_2$Income <- ordered(data_2$Income, levels = c("Less than BDT 20,000",
                                                      "BDT 20,000 - 39,999",
                                                      "BDT 40,000 - 59,999",
                                                      "BDT 60,000 - 79,999",
                                                      "BDT 80,000 - 99,999",
                                                      "BDT 100,000 - 119,999",
                                                      "BDT 120,000 - 139,999","BDT 140,000 and above"))
```

Converting as factor where needed:

```{r}
data_2$Living_arrangement <- as.factor(data_2$Living_arrangement)
data_2$Occupation <- as.factor(data_2$Occupation)
```


Drop NA and missing values if there is any.

```{r}
data_2 <- drop_na(data_2)
```

1.4: Now let's do a cross tabulation that will show the frequency by gender and occupation. we will be using the table() function.

```{r}
table_1 <- table(data_2$Occupation, data_2$Gender)
```

To view the table in your console:

```{r}
table_1
```


1.5: Do it yourself: Can you switch the row and columns? create a table (name it "table_2") that shows frequency of the respondents by gender and occupation where gender is shown as a row and occupation as column. Write the code and show us the table you created. Please note that it should look like a proper table. You can either create a manual table in MS Word and then paste the values from the console, or take a good screenshot of the table you created from the console and paste it in your assignment document (make sure that by seeing it we can understand it). 

One of the required resource above actually talked about it.

```{r}
table_2 <- table(data_2$Gender, data_2$Occupation)
```

To view the table in your console:

```{r}
table_2
```

1.6: We want to see the proportion of responses under each category of table_1. Therefore, we will be using the prop.table() function.

```{r}
table_3 <- prop.table(table_1, 1) # Note that we specified '1' in the argument of the function to show the data by row percentages, for column percentages we will be using '2'. Check the resource number 4 above for more clarification.

table_3*100
```


Note: outputs of prop.table() are showing as a proportion of 1. While interpreting/ discussing the results multiply the values by 100. Uou can also multiply the output with 100

There are resources above on row and column percentages, explore them  and remember that their interpretations will be different. The type of percentage we will use in our cross table analysis will depend on how we want to show our findings.  

1.7: Interpret the output table (table_3) in terms of Agriculture, Govt. employee and Private employee (3-4 sentences). Can you see any general pattern across all occupations (not only the 3 occupations I just mentioned) (1-2 sentences)? 

1.8: Now show the proportions of table_1 by column percentages (put the results in 'table_4). To get the proportions into percentages, multiply the output with 100. 

```{r}
table_4 <- prop.table(table_1, 2)

table_4*100
```

1.9: Write 2-3 sentences on about the general pattern in this table you just created. By looking into the table_4 (no code needed), identify the top 3 professions of the males and females in the sample dataset (which is lab 06 data). To clarify your confusion, table_4 has been created using the dataset Lab06_data. So, the output table shows few characteristics/ pattern of the sample data (Lab 06 data).

1.10: Now create a cross table that shows proportion of checking social media (as rows) by gender type (in columns). Proportions should be shown by column proportions 

Hint: check the entire process we did here to prepare our data for cross tab analysis. You may need to convert data type and order them (use your judgments while ordering) to ensure proper sequence in the output. 

```{r}
data_2$Social_media_check <- as.factor(data_2$Social_media_check)

summary(data_2$Social_media_check)
```

Write the code and paste your table following the instructions above.

1.11: Same as before, write max 100 words about the table (i.e., interpretation) you just created..

1.12: This is the ultimate test for you to test your understanding on creating tables and their interpretation: Create another table that shows the  proportion/ percentages of social media check either by occupation or income (your choice). It's up to you to decide which one will be the row and which one will be the column. Also, decide by your self whether you want show the output by row percentage or column percentage. This will also depend on which one is your row and which one is your column. Food for thought: what do you actually want to show from the table you are going to create?


1.13: same as before interpret the table you just created by converting the proportions as percentages. max 100 words. 


Note: all of the above codes were also available on the creating plots 3 file. 

Now, will talk more about creating tables.

Please read carefully all the notes and instructions stated in the workshop. As I say every time that through the workshops, we will be going to learn very few of R codes and its' applicability. You are encouraged to explore the different functions, codes in R and their application for your own development/ learning.

1.14: In the previous section, we have learned to create cross-tabs with categorical data. However, based on our research purpose, we can also convert numerical data into categories/ ranges and then create cross-tabs.

For example, in the dataset Lab06_data.csv, we used income, occupations, gender, etc. to create the cross-tabs. Those were categorical data. In R, most of the time data type of categorical data is read as character (chr). To make it ready for analysis, we convert character type data into factor data. Check all our previous workshops on data conversions. By this time, it should have become clear to you.

Please note that cross-tabs are also known as cross-tabulation and contingency table. All of them are same, however, in statistics, people prefer to call it contingency tables. It's me and there are lots of other people like me who just prefer to call it cross-tabs.

Now, let's assume, one of our research objectives is to see which age groups are associated with which occupation. In the previous section, we created a column name "Age".

1.15: If you remember, previously while creating plots, we reassigned values to the females' gross national income using the replace() function and created categorical variable that shows the income in ranges. 

We will be doing something similar here with the Age variable.

Please pay attention to the ">", "<" and "&" operators used in this function. It's really important that you learn how a function works - which means learn the arguments used in a function. There is no point of using functions blindly, as they will be of no use in your final project.

At first, let's look into the minimum and maximum value of 'Age' in the sample.

```{r}
min(data_2$Age)
max(data_2$Age)
```


If you look into the minimum and maximum age of the sample, you will see that minimum age of the respondents in the sample is -1772117459 - which is absurd. This is indicates that there could be a systematic error such as error in data recording. There are many ways to dealing with systematics errors, however, in this case, we will just get rid of the absurd values.

We will only use records of the respondents whose age were between 0 - 65 years (as 65 is the maximum age in our sample).

We will be using the subset() function.

```{r}

data_3 <- subset(data_2, Age > 0 & Age <= 65)
```


Now calculate the minimum and maximum values of Age.

```{r}
min(data_3$Age)
max(data_3$Age)
```


As you can see, the minimum age is 16 years and maximum age is 65 years. We will be creating a new column/ variable called 'Age_10years' where age of the respondents will be showing with 10 years of interval and name them accordingly. For example, if one respondent's age is 17 years, they will fall under 16-25 years age range. Similarly, if one is 38 years old, they will fall under 36-45 years category.

We will be doing similar thing as we did while creating plots, however, check the operators we are using. If you still have confusion how the replace() function works, please check other workshops and relevant resources. 

```{r}
data_3$Age_10years <- replace(
  data_3$Age_10years,
  data_3$Age >= 16
  & data_3$Age <= 25,
  "16 - 25 years")

data_3$Age_10years <- replace(
  data_3$Age_10years,
  data_3$Age >= 26
  & data_3$Age <= 35,
  "26 - 35 years")

data_3$Age_10years <- replace(
  data_3$Age_10years,
  data_3$Age >= 36
  & data_3$Age <= 45,
  "36 - 45 years")

data_3$Age_10years <- replace(
  data_3$Age_10years,
  data_3$Age >= 46
  & data_3$Age <= 55,
  "46 - 55 years")

data_3$Age_10years <- replace(
  data_3$Age_10years,
  data_3$Age >= 56
  & data_3$Age <= 65,
  "56 - 65 years")
```


1.16: Now, check the data type of 'Age_10years" and if it is character type, we already know that we need to convert it into a factor type. So, check the data type of 'Age_10years' and covert it into necessary type if needed. 

```{r}
str(data_3$Age_10years)
```

```{r}
data_3$Age_10years <- as.factor(data_3$Age_10years)
```


1.17: Now, order 'Age_10years' from lowest to highest age range. Check previous two labs if you need to recall the function to order a variable and the reason behind doing it. and google is always helpful.

1.18: Through a visualization, can you tell which age group is over represented in the sample? Which age group has lowest representation? 

1.19: Now let's look into the which age group is engaged into which occupation. Can you please create a cross-tab based on which you can describe most and least represented occupation is each age group? Please note that it is always a better practice to use proportional tables while doing cross-tabs and describe them based on percentages. 

Careful about specifying row-columns and therefore, row-column percentages as this also depends on how you want to interpret your data. 

```{r}
table_5 <- table(data_3$Age_10years, data_3$Occupation)

table_6 <- prop.table(table_5, 1)

table_6*100
```


1.20: Let's check our understanding of using replace() function. Can you create another new column/ variable called 'Age_5years' where age of the respondents will be showing with 5 years of interval and name them accordingly? The age ranges will be 16-20, 21-25, 26-30..........up to 65 years. Check the data table to see whether it was successfully created or not. 


1.21: I hope all these practices gave you the minimum basis to create graphs and tables for your final project. 



Now, you will learn about correlation test and finally apply those concepts to your data for the final project.

Here are few resources to build your knowledge on correlation tests, their interpretations, and visualizations.

1: https://statsandr.com/blog/correlation-coefficient-and-correlation-test-in-r/
2: http://www.sthda.com/english/wiki/correlation-test-between-two-variables-in-r
3: http://www.sthda.com/english/wiki/best-practices-in-preparing-data-files-for-importing-into-r
4: http://www.sthda.com/english/wiki/correlation-matrix-a-quick-start-guide-to-analyze-format-and-visualize-a-correlation-matrix-using-r-software


Always remember that before analyzing the data, it is very important to prepare the data for the analysis. And data preparation will depend on how your data is organized in the main file you are using for you analysis. 

Today, we will be using Census data for Hamilton from Open Data Hamilton [https://open.hamilton.ca/datasets/cdc66a4b09a147c39eeb747c15d5f9d8_4/explore]

I have downloaded the data and saved it as "Census_Profile_by_Census_Tract_-_2016" for this lab in csv format. 

Let's load the data.

```{r}
data_1 <- read.csv("C:/Users/shail/Downloads/Census_Profile_by_Census_Tract_-_2016.csv")
```

As you can see from the dataset, in the columns, it is showing the Census Tract numbers of the Hamilton and in the rows, it is showing the census attributes collected through the census survey in 2016. 

Column "CENSUS_BREAKDOWN" shows the actual property/ attribute/ variable names for each of the Census tract of Hamilton. You can see there are different categories and sub categories for different characteristics. For example, it is not only showing the total number of people between 0-14 years, under that category, it is also showing the subcategories/ breakdowns - total people between 0- 4 years, 5-9 years and 10- 14 years. So be careful while selecting/ specifying the variables for your analysis.

Also, you can see that for this dataset, observations (which is individual census tract for this data) are in the columns and actual variables for the data are presented as a row. We need to fix that to analysis the data in RStudio.

First, let's get rid of some of the unnecessary columns. As the column "CENSUS_BREAKDOWN"  contains the variable names for this dataset, we will delete the first two columns (OBJECTID AND SEX). Please keep reviewing the data table to understand what I am saying here. I will also encourage you to explore https://www.statcan.gc.ca/ website to understand the arrangements of census data and data dictionary. 

```{r}
data_1 <- data_1[-c(1:2)]
```

Now, we need to convert our rows as a column and columns as row. This is how our will see the columns as variables and rows as observations.

This is a useful resource on flipping rows and columns. Explore this resource and understand the arguments used in the workflow-  https://stackoverflow.com/questions/33643181/how-do-i-flip-rows-and-columns-in-r/33643244

The reason behind asking to explore multiple resources is to enhance your knowledge and introduce you to different hands-on methods in statistics. I also encourage you to explore different sources whenever you are encountering a problem or have confusion in a theoretical concept. This is all about facilitating your learning through understanding (I completely discourage following codes or even theories blindly without understanding) and developing the growth mindset in you.   

Here is how we will flip the columns through the following codes. Again, check the resources and browse for more if needed to understand the arguments. I am not discussing these on purpose so that you go and check resources by yourself.

```{r}
data_4 <- data.frame(t(data_1[-1]))

colnames(data_4) <- data_1[, 1]
```


1.22: As I am seeing some indifference in terms of understanding the codes and their arguments, I want you to explain the codes we just used to flip the rows and columns. How did the functions above do it? The answer is within the resource posted above and please browse more if needed. In your own words, by referencing data_1 and data_2, tell use how the above codes converted data_1 to data_2 and specified the variable names. (2-3 sentences).


1.23: If you can see from data_2, our census tracts (observations) are arranged as a rows and variables are presented as columns.

The first observation "HAMILTON_537_00000" actually is the number representing the entire city. We may need it for other types of analysis such as graphs/ tables for comparison purposes (obviously that will depend on what you want to analyze), but for correlation test, we won't be needing it as it is showing the total number/ sum of each variables for all census tracts in Hamilton. 

Therefore, we need to delete the first row (HAMILTON_537_00000) from data_2. Please do that. We have learned it in our previous labs. You can check them or check online. 

```{r}
data_4 <- data_4[-c(1),]
```


1.24: As you can see, there are too many variables in this dataset. However, let's assume we want to explore correlations between population of 2016 (column 1 - recall from the previous labs on how to find the column number of a variable), total private dwellings (column 4), land area in square kilometers (column 7), average household size (column 58), median total income in 2015 (column 663), and number of non-immigrants (column 1141). Please remember that these information regarding the population characteristics of census tract and here it is given as total number of individuals that fall under that category/ attribute. These are not specific information about a person/ individual in Hamilton.

Let's create a table containing those variables of our interest only. We have done this before in our previous workshops with column numbers. Go through them if needed. You can do it by column names only if you like.

```{r}
data_5 <- data_4[c(1, 4, 7, 58, 663, 1141)]
```


1.25: Now check the data types in data_3. As they are numerical variables/ data, they need to be converted into numeric data type if any of them are not numeric. Please do that after checking the data type.

1.26: Note that you may need to change the variable names if direct conversion doesn't work. But it will depend how your machine reads the data. After conversion, check the data type again to be sure that the variables now contain numeric type data.

```{r}
str(data_5)
```

```{r}
data_5$`Population, 2016` <- as.numeric(data_5$`Population, 2016`)
data_5$`Total private dwellings`<- as.numeric(data_5$`Total private dwellings`)
data_5$`Land area in square kilometres` <- as.numeric(data_5$`Land area in square kilometres`)
data_5$`Average household size` <- as.numeric(data_5$`Average household size`)
data_5$`    Median total income in 2015 among recipients ($)` <- as.numeric(data_5$`    Median total income in 2015 among recipients ($)`)
data_5$`  Non-immigrants` <- as.numeric(data_5$`  Non-immigrants`)
```

Also, ignore warnings if there is any. 

Now, let's drop the missing values to avoid some errors that occurs mostly due to missing values in the dataset.

```{r}
data_5 <- drop_na(data_5)
```

1.27: Now we will be running correlation tests. If you have checked the resources posted above, now you know that there are multiple functions and packages that can be used to conduct correlation tests. In this lab, we will be using 'ggpubr' packgae. You can use other packages if you like.

I have already installed the package and now loading it as a library. If you haven't installed it before, install the package first before loading it as a library. 

```{r}
library(ggpubr)
```

1.28: We will be using the cor.test() function under 'ggpubr' package. Here is the argument used in the functionc - cor.test(x, y, method=c("pearson", "kendall", "spearman")). where, x, y: numeric vectors/ variables with the same length and method: correlation method. You need to specify the method you will be using and method you will be using will depend on your data (continuous/ ordered/ rank). 

Let's say, we want to explore the correlation between "Total private dwellings" and "Land area in square kilometres". and we will be using pearson correlation method.

```{r}
cor.test(data_5$`Total private dwellings`, data_5$`Land area in square kilometres`, method = "pearson")
```

Note: if you have changed your variable names for "Total private dwellings" and "Land area in square kilometres", you need to specify those names in the function above. 

Look into the results. The correlation coefficient between "Total private dwellings" and "Land area in square kilometres" is -0.01830163 (based on the program, there is a possiblity to get slughtly different values, but that is OK for the time being). The results also showed the p-value and confidence interval. You need to formulate a null hypothesis, then discuss the correlation test results based on correlation coeffienct, p-value and confident interval. Is the finding consistent or inconsistent with the null hypothesis?

1.29: To make the analysis efficient (by not testing each pair of variables separately), we can also test correlation between all variables of our interest and create a correlation matrix. Check the resources above on creating a correlation matrix. 

Among different available packages, we will be using 'correlation' package and the correlation() function under it. Feel free to use other functions/ packages if you like/ feel convenient. 

Let's install and load the package and then create the correlation matrix.

```{r}
library(correlation)
```

```{r}
correlation(data_5, include_factors = TRUE, method = "pearson")
```

Please check the function and the arguments under it. One of the resources have this information. In the outputs, you will see that all the correlation coefficients (denoted here as r) of the each pair/ combinations of the variables, their corresponding confidence interval (CI), t-stats and p-value. If you are planning to use correlations in your final project, you can use this and then interpret the results. 

1.30: To practice, create a table to show the results from the correlation matrix and based on the findings, discuss the strongest 3 and weakest 3 correlations (based on r and it's +/- sign) which are statistically significant (based on p-value). 

Note: Resources also include workflow for a graphical visualization of correlation matrix. You can check and apply them in your final project if you want.

1.31: Now, we will learn about the chi-square test which researchers use for categorical variables. Here is a required reading for that: https://data-flair.training/blogs/chi-square-test-in-r/. 

You can present your results in combination of cross-tabs and correlation results. In many cases, emphasis has been given more on analyzing the cross-tabs, but for your final project if you are performing correlation analysis, please interpret your correlation test results as well. Note that, correlation method will depend on the type of data you have/ want to use for your analysis (numeric/ ordinal/ categorical).

If you can remember, in the previous section, we created categorical variables (in the form of a range) from the numeric variable 'Age'. 

In the previous section, we created a cross-tab of Age_10years and Occupation. It is always wise to present the chi-square tests results (or other tests for categorical data) with cross-tabs (you can choose your row/ column percentages based your research objective).

These are the codes for the chi-square analysis.

```{r}

chisq.test(data_4$Age_10years, data_4$Occupation)
```

We interpret the chi-square test based on p-value. Whether there is a statistically significant different between the variables/ whether the test results are consistent/ inconsistent with the null hypothesis. Please discuss the chi-square results. 

Check the resource above for more details. 




