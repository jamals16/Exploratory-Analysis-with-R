---
title: "Creating Plots Part 3"
author: "Shaila"
date: "22/02/2022"
output: html_document
---

In this workshop, you will learn about creating line plots using R package 'ggplot2', and cross -tabulation.
Please read carefully all the notes and instructions stated in the lab.
As I say every time that through the labs, we will be going to learn very few of R codes and its' applicability. You are encouraged to explore the different functions, codes in R and their application for your own development/ learning.

3.1: At the beginning, clear the environment

```{r}
rm(list=ls())
```

Now, load the required packages.

```{r}
library(tidyverse)
library(ggplot2)
```

We will be using the datasets we used in the previous workshop for creating a line graph. Let's begin with the historical index dataset. Be mindful of the path you are specifying. Feel free to use the other method.

```{r}
data_1 <- read.csv("C:/Users/shail/Downloads/Lab 5 Datasets/historical_index.csv")
```

Check the data type

```{r}
str(data_1)
```

We need to create each year's data into numeric value. I am converting the character variables into number. Note that, in my computer, after loading the data, only HDI for 1990 and 2000 are showing as character variables. It could be possible (although rarely) that in your computer, other years' values are showing as character. In that case, covert them into numeric variables as well.

```{r}
data_1$Human.Development.Index..1990.<- as.numeric(data_1$Human.Development.Index..1990.)
data_1$Human.Development.Index..2000. <- as.numeric(data_1$Human.Development.Index..2000.)
```

As there are missing values in some years, we will be dropping them so that the lines in our graph become continuous lines.

```{r}
data_1 <- drop_na(data_1)
```

Ignore the warning messages unless there is an error message.

3.2: Now, let's start working on creating line graph or let's say here we will be doing a time series analysis.

As the names are too long, I will be changing the names of the columns containing HDI for each year

If rename() function is not working for you, use names() function. I have shown some examples in the later sections of this lab (see 3.13).

```{r}
data_1 <- rename(data_1, Y1990 = Human.Development.Index..1990., 
                 Y2000 = Human.Development.Index..2000., 
                 Y2010 = Human.Development.Index..2010.,
                 Y2011 = Human.Development.Index..2011., 
                 Y2012 = Human.Development.Index..2012., 
                 Y2013 = Human.Development.Index..2013.,
                 Y2014 = Human.Development.Index..2014.)
```

3.3: You need to learn about wide data form and long data form in R. Most cases, you will find data that are in wide form, like the HDI index data we are using. However, most of the R functions can work when data are in long format, specially, ggplot2

This is a required reading for you to understand the long data and wide data form and how they work. Please read: https://www.datacamp.com/community/tutorials/long-wide-data-R

Note that wide-form displays many measurements from one individual/ place/ location in one row and the column names show what the measurements are, whereas you'll only see one measurement of an individual/ place/ location in one row in long-form.

Please read every instruction and reading posted here. Going through the above resource should certainly clear your concept on wide and long data form.

The resource above has showed us multiple functions to convert wide data into long data. I will be using gather() function. You are free to use other functions. If you are using another function, remember to install and load (as library) the corresponding packages.

Here are the detail arguments used in the gather() function, read carefully: https://tidyr.tidyverse.org/reference/gather.html

Please learn about the arguments used in each function we are using. Otherwise, you won't be able to work independently with functions.
As you can understand after reading the resources, for the long format of the HDI dataset, you need to create a column that will contain the year name and another column that will show the corresponding HDI. I am naming them "Year" and "HDI" respectively and finally, in the function, I will specify the columns in the wide-form dataset containing HDI values for each year.

```{r}
data_long <- gather(data_1, Year, HDI, Y1990:Y2014)
```

Now look into the data_long data you created by clicking it from the environment. The dataset contains the required variables for our line graph in long form.
For better understanding, look into data_1 which is in the wide form and data_long which is in the long form containing the same information. 

3.4: Here is another resource on creating line graphs - something you might find more useful for your data analysis project: http://www.sthda.com/english/wiki/ggplot2-line-plot-quick-start-guide-r-software-and-data-visualization

The procedure/ coding pattern for creating the line graph is almost same like previous graphs using ggplot2 accept few changes such as using geom_line(). As in this graph, we will be showing each country's change/ trend of HDI from 1990 to 2014 (which means multiple lines), we need to specify the country name as 'group' under aes().

```{r}
line_graph_1 <- ggplot(data_long, aes(x=Year, y=HDI, group=Country)) +
  geom_line()+
  geom_point()

print(line_graph_1)
```


Note: Instead of print() you are free to use ggsave() if you find them more convenient. 

3.5: Do it yourself: Check the graph: it is showing the changes in HDI from 1990 to 2014 of different country. Now make it a good visual, with proper axis name, title, show the lines by their country name (try your best as for many of you it won't be a perfect one as there will be lots of overlaps). How about changing the color of the lines into something lighter?

Paste your edited graph here including the codes you used to create them. Don't get stressed. We want to see the efforts you made to make it nice!

3.6: As you can see, it's very hard to make everything properly visible in the graph. One way could be to show them into different parts. Let's say you want to show the countries' trend of HDI change whose HDI in 2014 was below/above world average. I am giving you the workflow, you will be doing them with codes and paste the output when asked.

At first, go to HDI index dataset (wide form), where we already converted values into numeric and dropped missing values. From that dataset calculate the mean HDI of 2014. Write the code and your answer.

```{r}
mean(data_1$Y2014)
```

3.7: Now select the countries that has 2014's HDI values above and below the mean HDI values of 2014. Two tables will be created. Name the one with countries above mean value as "data_above_mean' and the one with countries below mean value as "data_below_mean".

Hint: from our previous workshopd, remember subseting dataset? use subset() function. Check this resource as well: https://www.statmethods.net/management/subset.html. Be mindful about which column (which year of HDI?) you are specifying for selecting the countries. You can use other functions if you want.

```{r}
data_above_mean <- subset(data_1, Y2014 > 0.7089)

data_below_mean <- subset(data_1, Y2014 < 0.7089)
```


3.8: Covert both newly created tables/ datasets into long-form. Write down the codes.Please also check whether your datasets have been converted into long-form. Otherwise, entire calculations will be wrong.

```{r}
data_long_above_mean <- gather(data_above_mean, Year, HDI, Y1990:Y2014)

data_long_below_mean <- gather(data_below_mean, Year, HDI, Y1990:Y2014)
```

3.9: Do it youself: Now create two graphs with fulfilling proper visualizations requirements. Write your codes for both graphs and paste both graphs you created. Try your best, there is a possibility of having overlaps as well.

As you can see it's not always easy to visualize what you want. You need to keep trying and look for alternatives (i.e., methods to do the same thing or do some different visualizations that will serve your purpose). You need to explore and decide what you want to do.


3.10: Let's say, you want to focus only on five countries: Canada, Norway, Peru, Egypt and South Africa. Let's create subset of these countries from data_1 and name the table as fav_5. If you are using subset(), check how character values are specified in subset function and the signs used - available on the resource posted above.

```{r}
fav_5 <- subset(data_1, Country == "Canada" | Country == "Norway" |
                  Country == "Peru" | Country == "Egypt" |
                  Country == "South Africa")
```

3.11: Do it yourself: Now select another 5 countries of your choice, and create a line graph showing the changes of HDI from 1990 to 2014 following proper visualization guidelines. Write all the codes including comments of each step and then paste your graph.


3.12: Interpret the line graph you just created. Max 100 words.


3.13: At this point, we will be learning on creating cross tabulations and their interpretation.

These are required reading for this workshop:
1. https://www.surveyking.com/help/cross-tabulation-analysis
2. https://www.qualtrics.com/experience-management/research/cross-tabulation/ (we will learn about the chi-square analysis later, but still it is good to go through everything here)
3. https://libraryguides.mcgill.ca/c.php?g=699776&p=4968551
4. https://www.statmethods.net/stats/frequencies.html

Feel free to do some google search if some terms seem unknown to you.

We will be using a different dataset this time - it is a part of response table (contains only a limited number of questions) from a quantitative survey conducted in Bangladesh between June-Aug 2020 labled as Lab06_data.csv)

Let's load the data.

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

Check this resource about using names() function and possible errors that could happen: https://stackoverflow.com/questions/6081439/changing-column-names-of-a-data-frame

Now we will be changing the names of the columns that contains information on birth year, gender, income, living_arrangement and occupation. If you look into the dataset, and put your cursor over the corresponding column, it will show you the column number. These column numbers are 12, 13, 14, 15 and 16 respectively. Here is the code how you change multiple column names using names() function.

```{r}
names(data_2) [12:16] <- c("Year", "Gender", "Income", "Living_arrangement", "Occupation")
```

3.14: Now create a column called 'Age' in data_2 that shows the age of the respondents during the survey year (which is 2020). 

```{r}
data_2$Age <- 2020 - data_2$Year
```

3.15: As you can see from the data type (we used str() function to see that), gender, income, living_arrangement and occupation are character type, you need to convert them into factor variables to the cross tabulation analysis. Let's do that. 

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

3.16: Now let's do a cross tabulation that will show the frequency by gender and occupation. we will be using the table() function.

```{r}
table_1 <- table(data_2$Occupation, data_2$Gender)
```

To view the table in your console:

```{r}
table_1
```


3.17: Do it yourself: Can you switch the row and columns? create a table (name it "table_2") that shows frequency of the respondents by gender and occupation where gender is shown as a row and occupation as column. Write the code and show us the table you created. Please note that it should look like a proper table. You can either create a manual table in MS Word and then paste the values from the console, or take a good screenshot of the table you created from the console and paste it in your assignment document (make sure that by seeing it we can understand it). 

One of the required resource above actually talked about it.

```{r}
table_2 <- table(data_2$Gender, data_2$Occupation)
```

To view the table in your console:

```{r}
table_2
```

3.18: We want to see the proportion of responses under each category of table_1. Therefore, we will be using the prop.table() function.

```{r}
table_3 <- prop.table(table_1, 1) # Note that we specified '1' in the argument of the function to show the data by row percentages, for column percentages we will be using '2'. Check the resource number 4 above for more clarification.

table_3
```


Note: outputs of prop.table() are showing as a proportion of 1. While interpreting/ discussing the results multiply the values by 100. 

There are resources above on row and column percentages, explore them  and remember that their interpretations will be different. The type of percentage we will use in our cross table analysis will depend on how we want to show our findings.  

3.19: Interpret the output table (table_3) in terms of Agriculture, Govt. employee and Private employee (3-4 sentences). Can you see any general pattern across all occupations (not only the 3 occupations I just mentioned) (1-2 sentences)? 

3.20: Now show the proportions of table_1 by column percentages (put the results in 'table_4). Write the code and paste your table according to the guidance above. 

```{r}
table_4 <- prop.table(table_1, 2)

table_4
```

3.21: Write 2-3 sentences on about the general pattern in this table you just created. By looking into the table_4 (no code needed), identify the top 3 professions of the males and females in the sample dataset (which is lab 06 data). To clarify your confusion, table_4 has been created using the dataset Lab06_data. So, the output table shows few characteristics/ pattern of the sample data (Lab 06 data).

3.22: Now create a cross table that shows proportion of checking social media (as rows) by gender type (in columns). Proportions should be shown by column proportions 

Hint: check the entire process we did here to prepare our data for cross tab analysis. You may need to convert data type and order them (use your judgements while ordering) to ensure proper sequence in the output. 

```{r}
data_2$Social_media_check <- as.factor(data_2$Social_media_check)

summary(data_2$Social_media_check)
```

Write the code and paste your table following the guidelines above.

3.23: Same as before, write max 100 words about the table (i.e., interpretation) you just created. While interpreting/ discussing (no code to convert them into percentages is needed at this moment) you should discuss them as percentages (by multiplying the proportions by 100).


3.24: This is the ultimate test for you to test your understanding on creating tables and their interpretation: Create another table that shows the  proportion of social media check either by occupation or income (your choice). It's up to you to decide which one will be the row and which one will be the column. Also, decide by your self whether you want show the output by row percentage or column percentage. This will also depend on which one is your row and which one is your column. Food for thought: what do you actually want to show from the table you are going to create?

Write the codes and paste your table according to the guidelines.

3.25: same as before interpret the table you just created by converting the proportions as percentages (no code for that is needed). max 100 words. 





