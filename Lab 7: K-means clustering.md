---
title: "K-means clustering"
author: "Shaila"
date: '2022-04-03'
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```


At the beginning, let's clear the environment with the following codes:

```{r}
rm(list=ls())
```

Loading packages 

```{r echo=FALSE}
library(tidyverse)  # data mining
library(cluster)    # clustering algorithms
library(factoextra) # clustering algorithms & visualization
```

# Recommended Readings: 

1. K-means Cluster Analysis: https://uc-r.github.io/kmeans_clustering
2. What is scaling: https://www.jigsawacademy.com/cluster-analysis-scaling/

# Importing data

```{r}
df <- read.csv("starbucks-menu-nutrition-drinks.csv")
```

# Removing any missing value that might be present in the data

```{r}
df <- na.omit(df)
```


```{r echo=FALSE}
# As we do not want the clustering algorithm to depend to an arbitrary variable unit, we start by scaling/standardizing the data:
df1 <- scale(df[,2:7]) # selecting only continuous/numeric variables for standardizing/ scaling
head(df1) # to see the column names
```
# Join datasets
```{r}
df1 <- data.frame(df$Item, df1)
df1
```


# K-Means Clustering of Starbucks items by Calories and Protein Consumption: creating 4 clusters using 2 variables

```{r}
# Cluster Analysis - kmeans
set.seed(123456789) ## to fix the random starting clusters
Cluster_1 <- kmeans(df1[,c("Calories","Protein")], centers=4, nstart=25)
Cluster_1
```

```{r}
## Finding cluster assignments
o=order(Cluster_1$cluster)
df2 <- data.frame(df1$df.Item[o],Cluster_1$cluster[o])
df2
```

# Visualization of Cluster Analysis

```{r, fig.width=12, fig.height=12, fig.fullwidth=TRUE}
visual_1 <- plot(df1$Calories, df1$Protein, type="n", xlab="Calories", ylab="Protein")
text(x=df1$Calories, y=df1$Protein, labels=df1$df.Item, col=Cluster_1$cluster)
```
** Two useful resources to solve the overlapping labels in a scattered plot using 'ggplot2' and 'ggrepel' package: 

1. https://r-graphics.org/recipe-scatter-labels
2. https://ggrepel.slowkow.com/articles/examples.html


Here is an example using 'ggplot2' and 'ggrepel' package to resolve the overlapping issue.

Loading the packages. Instead of here, you can load these at the beginning if you want (that is recommended). 

```{r}
library(ggplot2)
library(ggrepel)
```

Creating the graph

```{r, fig.width=12, fig.height=22, fig.fullwidth=TRUE}
visual_2 <- ggplot(df1, aes(x = Calories, y = Protein, label = df.Item)) +
    geom_point(col=Cluster_1$cluster, size = 3) +
    geom_label_repel(max.overlaps = Inf) # setting this so that all labels are shown in the plot.
visual_2
```

# Interpreting clusters:

Displaying the descriptive statistics:
```{r}
des_stat <- df1 %>% select("Calories", "Protein") %>%  # selecting these two variables as we created the cluster based on these two variables
  mutate(Cluster = Cluster_1$cluster) %>%
  group_by(Cluster) %>%
  summarise_all("mean")

des_stat
```
or the most easiest way to find the mean values of the clusters are displaying the centres of the Clusters you created using the following codes:

```{r}
Cluster_1$centers
```


You can describe these mean values and discuss each cluster's characteristics

Also, you can visualize this through a plot (see the example below) and discuss them by combining the plots and tables. Note: If your readings contains other types of visualization, please do them. Also, you can check with your instructor or TA on what type of visualization they are expecting. 


```{r}
ggplot(des_stat, aes(x=Cluster)) + 
  labs(y="Mean value of the Variable") +
  geom_point(aes(y = Calories), color = "red") + 
  geom_point(aes(y = Protein), color="blue") 

```
Another way of visualizing 

```{r}
df_plot <- gather(des_stat, key = "variable", value = "value", 2:3) #converting the second (Calories) and third (Protein) column into long format
df_plot
```
The long format was needed to create the following plot using ggplot2

```{r}
ggplot(df_plot, aes(x = Cluster, y = value)) + 
  geom_line(aes(color = variable, linetype = variable)) + 
  scale_color_manual(values = c("red", "blue")) +
  geom_text(aes(label=value, vjust=0.5)) # adding this line to add the values
```
The values are too big? Let's consider only three decimal places by using 'format' function.

```{r}
df_plot$value <- as.numeric(format(round(df_plot$value, 3))) # we also used 'as.numeric' function as this data need to be in numeric form to be plotted as values
df_plot
```

Now plot your visuals:
```{r}
ggplot(df_plot, aes(x = Cluster, y = value)) + 
  geom_text(aes(label=value)) +
  geom_line(aes(color = variable, linetype = variable)) + 
  scale_color_manual(values = c("red", "blue"))
   # adding this line to add the values
```

