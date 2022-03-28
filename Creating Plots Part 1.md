---
title: "Creating Plots Part 1"
author: "Shaila"
date: "22/02/2022"
output: html_document
---

At the beginning, let's clear the environment with the following codes:

```{r}
rm(list=ls())
```

Importing packages

```{r}
library(tidyverse)
```

1.1: Import data We will import some Airbnb (http://insideairbnb.com/get-the-data.html) data for Amsterdam that was collected during August, 2021.
I saved the 'listings' data of Amsterdam as Lab03Data.csv. Import data 'Lab03Data.csv'. 

```{r}
data <- read.csv("Lab03Data.csv")
#Here is another way to import data in the R environment by specifying the paths: data <- read.csv("C:/Users/shail/Downloads/Lab03Data.csv")
```


1.2: At first, let's check whether the imported data is a data frame. This code below tests whether the data object we imported is a data frame. Data frames are handy data objects in R.  

```{r}
is.data.frame(data)
```

In the console, you should see the '[1] TRUE' after you run the above code. This means that data is a data frame. 

1.3: The imported Airbnb data contains a wide array of variables. Check the environment by clicking on 'data'. Run the code below to see the names of the variables. 
The colnames() function shows the column names in a data frame:

```{r}
colnames(data)
```

Now we will check the data type of each variable by using function str():

```{r}
str(data)
```


Look into the console to see various data types in the dataset. For example, the variable 'id', which is an Integer. The variable 'name', 'neighbourhood' are character variables. 

1.4: One of the variables in the data is 'neighbourhood' which shows the neighbourhood name the Airbnb property falls into. Let's find out the names of the neighbourhoods in Amsterdam that contains Airbnb properties. We will use the unique() function to find out the unique names of the neighbourhoods

```{r}
unique_names <- unique(data$neighbourhood)
```

Type the below codes to see the unique neighbourhood names in the data in console (also can be visible in the environment).

```{r}
unique_names
```

1.5: Run the below code to count up the number of unique place names.

```{r}
length(unique_names)
```


1.6: Do it yourself: Write your own code to put the output from the length function above into a new vector called 'number_unique'.

```{r}
number_unique <- length(unique_names)
```

1.7: From the unique_names above, choose any neighbourhood name and run the code below by modifying it. I chose "Centrum-Oost". 
Important: replace the word "Centrum-Oost" with the name of the neighbourhood you have chosen.

```{r}
My_neighbourhood <- data[data$neighbourhood == "Centrum-Oost", ]
```

This code above selects all records with the name you chose equal to the variable name. Check this by clicking My_neighbourhood from the environment.

1.8: Creating frequency tables:

A common basic analysis task with data is to create a frequency table for a categorical variable. For example, the number of listings per neighborhood. 
This can be achieved using the table() function which prints a list of the unique attributes and the frequency of these observations.

```{r}
table(data$neighbourhood)
```

The resulting table is still not a data frame. Although we can see the table in the console, nothing has been created in the environment's 'Data'
To make the resulting table a data frame, we can use the following codes: 

```{r}
Freq_neigh <- as.data.frame(table(data$neighbourhood))
```

If you look into the newly created Freq_neigh from the environment, you will see that a new table has been created, where the column containing neighbourhood names is showing as 'Var1'.

1.9: Recall your learning from previous sessions, and rename the variable 'Var1' as 'Neighbourhood_names' in the Freq_neigh.

```{r}
Freq_neigh <- rename(Freq_neigh, Neighbourhood_names = Var1)
```

1.10: By using table 'data', find out the maximum, minimum, mean and median price for Airbnb property listings in Amsterdam. 
Do it yourself: Use functions and write your answers with your codes. 

```{r}
min(data$price_USD)
max(data$price_USD)
mean(data$price_USD)
median(data$price_USD)
```


1.11: Create aggregations and summary statistics

We have shown how we can use descriptive summary functions for single columns, however, it is also possible to combine these with further functions which make these calculations within aggregations. This is especially useful for geographic application where you can create summaries by a defined area. 
In the following example we will use the function aggregate() to find out what the mean price is by neighbourhood and save it in 'data_agg'. 

Here are few details on aggregate function in R: https://www.datasciencemadesimple.com/aggregate-function-in-r/

```{r}
data_agg <- aggregate(x=data[,"price_USD"],by=list(data[,"neighbourhood"]),FUN=mean)
```

1.12: Look into the output table (data_agg) and rename the columns of data_agg as 'neighbourhood' and 'mean_price'.

```{r}
data_agg <- rename(data_agg, neighbourhood = Group.1, mean_price = x)
```

1.13: Now find out the mean price by room_type in Amsterdam using 'data' and store the findings in 'data_agg_2'.

```{r}
data_agg_2 <- aggregate(x=data[,"price_USD"],by=list(data[,"room_type"]),FUN=mean)
```

1.14: Rename the columns of data_agg_2 as 'room_type' and 'mean_price'

```{r}
data_agg_2 <- rename(data_agg_2, room_type = Group.1, mean_price = x)
```

1.15: Do it yourself: Create a column 'Price_range' in 'data_agg' that shows 'TRUE' for mean prices of the neighbourhoods above median price of Amsterdam (calculated in 1.10) and 'False' for prices below or equal to median price.

```{r}
data_agg$Price_range <- ifelse(data_agg$mean_price > 130, "TRUE", "FALSE")
```

1.16: Do it yourself: Calculate the number of neighborhoods in 'data_agg' with mean accommodation prices higher and lower than the mean (calculated in 3.20) price in Amsterdam. Write down your answers with codes.

```{r}
nrow(data_agg[data_agg$mean_price > 156.87, ])
nrow(data_agg[data_agg$mean_price < 156.87, ])
```

Now, we will learn about creating plots in R and the application of R package 'ggplot2'. Please read carefully all the notes and instructions stated in the lab.
'ggplot2' is a very useful package with lots of functions to create graphs. We will learn very few of it's applicability in this lab because of the time constraints.
You are encouraged to explore the different functions of ggplot2 and it's application.
Here is a useful resource, but there are numerous resources available in the web: https://bookdown.org/rdpeng/RProgDA/basic-plotting-with-ggplot2.html

1.17: We will start by working with the same dataset. Before that, let's install (if you haven't yet) and load the required packages for this workshop.

```{r}
# we have already installed and loaded 'tidyverse'
library(ggplot2)
```

1.18: Now let's start creating a simple bar plot.
You are encouraged to explore this resource to develop better understandings. Something that may become more useful while conducting a Data Analysis Project: https://www.r-graph-gallery.com/218-basic-barplots-with-ggplot2.html

- To create any kind of plot using 'ggplot2', we have to start with the ggplot() function. After that, we need to specify our dataset/ data object.
- Please remember that the dataset must have to be a data frame. We have learned in the previous sections on how the check whether a dataset is a data frame.If your table is not a data frame, you can convert it into a data frame by using as.data.frame() function.
- After specifying the dataset, you need to specify the aesthetics, by using aes() function, where you will specify the variables from the dataset to be displayed in the bar plot and in which axis they will be displayed.
- Finally, you will use the geom functions to specify which type of plot you want to use. There are different types of geom functions such as geom_bar(), geom_point(), geom_line(), geom_histogram(), etc. based on the type of plot we want to create.

To create the barplot, we will use the geom_bar() function.

We are going to create a bar plot that shows the frequency distribution of the properties in different neighborhoods of Amsterdam and save the output in plot_1.

```{r}
plot_1 <- ggplot(data, aes(x=neighbourhood)) +
  geom_bar()
```

We can view our created plot using the print() function.

```{r}

print(plot_1)
```

The output will be visible in the right hand side under 'Plots'. You can export your output as a pdf or image from there from the 'Export' tab.

1.19: As you can see, there are too many neighbourhood names in the x-axis as the names have overlapped. Let's try to make it more readable. We can create a horizontal bar plot. For which we will be using coord_flip() function.

```{r}
plot_2 <- ggplot(data, aes(x=neighbourhood)) +
  geom_bar() +
  coord_flip()
```

To view the plot,

```{r}
print(plot_2)
```

1.20: Now the neighborhood names are clearly readable. We can also make other editing to make it more attractive and informative. For example, you need to put a title on your plot, change the axis names. Here are the codes using labs() function:
Also, rather than using coord_flip(), you can just change the axis as well, which I did here. Look into the aes() in the code below where I specified the axis as 'y' instead of 'x'.
Please look carefully into the codes while typing. Most of the errors occur because you forgot to use the '+' sign, comma "," and did spelling mistakes.

```{r}
plot_2 <- ggplot(data, aes(y=neighbourhood)) +
  geom_bar(fill = "blue", width = 0.2) +
  labs(title= "Counts of Airbnb Properties by Neighbourhoods in Amsterdam",
         x="Number of Properties", y = "Names of the Neighbourhood in Amsterdam")
```

Use the print function to see the changes

```{r}
  print(plot_2)
```

Looking into the output you can also see that I changed the colors and width of the bar indicating the fill color and width percentage in geom_bar() function. You can try them in the next section as well.


1.21: Do it yourself: Now it's you turn to create a bar plot using the variable 'room_type'. Recall your learning. The bar plot (could be a horizontal/ vertical) you will be creating here should definitely be an example of good data visualization and should contain appropriate titles, axis names with clear visibility and readability.
Remember to change the color (an appropriate color in terms of visualization) of the plot you are creating.


1.22: Remember the use of aggregate() function? Here the output table of the following codes shows the mean price of the properties by neighbourhood.

```{r}
data_agg <- aggregate(x=data[,"price_USD"],by=list(data[,"neighbourhood"]),FUN=mean)
data_agg <- rename(data_agg, neighbourhood = Group.1, mean_price = x)
```

You can use this output to create a green bar plot that shows the mean price by neighbourhoods. Here are the codes for the vertical bar plot:

```{r}
plot_3 <- ggplot(data_agg, aes(x=neighbourhood, y= mean_price)) +
  geom_bar(stat = "identity", fill = "green")
```

Use the print function to see the plot

```{r}
print(plot_3)
```

See that under aes(), I specified the y axis which is now mean price. You need to include stat=identity under geom_bar(), which is basically telling ggplot2 you will provide the y-values for the barplot, rather than counting the aggregate number of rows for each x value, which is the default 'stat=count' - something we did previously where we only use the counts of the neighbourhoods.

1.23: Do it yourself: Use you learning from the previous sections and make plot_3 a good data visualization with appropriate titles, axis names with clear visibility and readability. Change the color of the plot as well.


```{r}
plot_3 <- ggplot(data_agg, aes(x=mean_price, y= neighbourhood)) +
geom_bar(stat = "identity", fill = "blue", width = 0.2) +
  labs(title= "Mean of Airbnb Properties by Neighbourhoods in Amsterdam",
       x="Mean price of Properties", y = "Names of the Neighbourhood in Amsterdam")

print(plot_3)
```


1.24: Let's make a dot plot with the same variables. We will be using geom_point():

```{}
plot_4 <- ggplot(data_agg, aes(x=neighbourhood, y= mean_price)) +
  geom_point() +
  coord_flip()

print(plot_4)
```

I used coord_flip() here. Instead, you can just change the axis of the variables.


1.25: We can also create scatter plot using the geom_point() function. Suppose you want to create a scatter plot using the price_USD and minimum_nights. Here is the code for that:

```{r}
plot_5 <- ggplot(data, aes(x=price_USD, y= minimum_nights)) +
  geom_point()

print(plot_5)
```


1.26: Suppose you want to color the dot points by room type. You can specify it under the aes(): .

```{r}
plot_5 <- ggplot(data, aes(x=price_USD, y= minimum_nights, color = room_type)) +
  geom_point()

print(plot_5)
```

1.27: As you can see in the output plot, most of the values are clustered between price range less than 1500 USD and minimum nights less than 150 nights. We can specify the axis limits by using lims() function to zoom into those values:

```{r}
plot_5 <- ggplot(data, aes(x=price_USD, y= minimum_nights, color = room_type)) +
  geom_point() +
  lims(x = c(0, 1500), y = c(0, 150))

print(plot_5)
```

You might see a warning message on removing rows. Ignore it as long as no error message is showing up.


1.28:Do it yourself: Now create a scatter plot showing the price vs number of reviews. Make them visible by neighbourhood name. Like before, create it with appropriate titles, axis names with clear visibility and readability. Note that while exporting your graph from the 'Plots' tab, you can change the height and width of your image with the options there. You may need it to ensure proper visibility. 
Change the legend name as well. Hint: use labs(color = ).

```{r}
plot_6 <- ggplot(data, aes(x=price_USD, y= number_of_reviews, color = neighbourhood)) +
  geom_point() +
  labs(title= "Counts of Airbnb Properties by Neighbourhoods in Amsterdam",
       x="Price of Properties", y = "Names of the Neighbourhood in Amsterdam", color = "testing")

print(plot_6)
```


1.29: We will be creating a box plot using ggplot2 package. Details on boxplot and what it shows can be found here: https://en.wikipedia.org/wiki/Box_plot
Watch this video on boxplot and its interpretation: https://www.youtube.com/watch?v=2-dJCrgKnaY 
You are also welcomed to explore other resources.

Here is the code to create boxplots for the Airbnb property prices upto USD 300 by neighbourhoods:

```{r}
plot_6 <- ggplot(data, aes(x= price_USD, y = neighbourhood)) +
  geom_boxplot(fill = "blue", width = 0.2) +
  labs(title= "Boxplot of Airbnb Property Prices by Neighbourhoods in Amsterdam",
       x="Price of Properties (USD)", y = "Names of the Neighbourhood in Amsterdam") +
  lims(x = c(0, 300))

print(plot_6)
```

You might see a warning message on removing rows. Ignore it as long as no error message is showing up.

1.30: Do it yourself: Select any two neighbourhoods from the plot, and describe what their boxplots revealing. Here is another useful resource for interpreting boxplots: https://towardsdatascience.com/understanding-boxplots-5e2df7bcbd51

You don't have to go to that details as the resource, but talk about the position of the median, percentiles, outliers and what it means for the distribution pattern of the Airbnb properties less than USD 300 for those two neighbourhoods in Amsterdam.


