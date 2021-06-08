---
title: "Harris Lab R Training Module"
author: "Jeremy Lowe"
date: "6/7/2021"
output:
  html_document:
    toc: yes
    toc_float: yes
    number_sections: yes
    keep_md: yes
  github_document:
    toc: yes
    number_sections: yes
    
---

<style type="text/css">
.code-style {
  background-color: snow;
  border: 1px solid black;
  font-weight: bold
}
</style>




<center>
![Image credit: Allison Horst](C:/Users/jerem/Desktop/Harris Research/_Lab_Photos_JL/horst_rstudio_air.png)
Image Credit: Allison Horst
</center>

***
# Introduction

This training module was created for the Harris Lab to aid in developing publication-ready figures, tables, and more. It assumes you have a basic knowledge of R including importing data, 'tidying' it, and basic analyses. It is by no means meant to reinvent the wheel when it comes to more detailed trainings (like the ones found in [the R for Data Science textbook](https://r4ds.had.co.nz/index.html).) More general resources for the lab can also be found in the [Harris Lab R Resources document](https://docs.google.com/document/d/1F6vYfGr8zeFSx2Wo0bD8fEipSpl7VPpqFNFkbbOOw6I/edit#heading=h.ft1w0w4x8r7h).

For this small module, we will be using data from the *Iris* dataset, which is included in the Tidyverse package. The dataset actually contains data from a study conducted in 1936 by Ronald Fisher, so if you are interested in learning more, you can view it [here](https://onlinelibrary.wiley.com/doi/epdf/10.1111/j.1469-1809.1936.tb02137.x). It contains measurements from three different irises that characterizes different dimensions for each. The image below gives you an idea of the data we're looking at:

  &nbsp;
  
<center>
![](C:/Users/jerem/Desktop/Harris Research/_Lab_Photos_JL/iris_sepal_petal.png)

</center>

  &nbsp;

And the code chunk below demonstrates how to load the data and other relevant packages into the environment:

  &nbsp;
  


```{.r .code-style}
# Loading in relevant packages, some of these will be used later
library(tidyverse)
library(table1)
library(kableExtra)
library(patchwork)


# Loading data into the environment
# You will likely use the read_csv() function in Tidyverse to load other datasets.
data(iris)

# Take a peek at the data to see what we're working with!
head(iris)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
## 6          5.4         3.9          1.7         0.4  setosa
```

  &nbsp;


***
# R Markdown

R Markdown is a literate programming file within RStudio that can take advantage of html formatting and more to produce amazing tables, visualizations, dashboards, reports, and more. It was used to create the document you are viewing right now as well! This tutorial relies on packages that make use of the html formatting in R Markdown, so it is recommended you use an R Markdown file when following along. More information on R Markdown can be found within [Chapter 27](https://r4ds.had.co.nz/r-markdown.html) of the R for Data Science textbook linked in the previous section. Once you are comfortable with the functions of R Markdown and have an R Markdown file created, you can continue on.

***
# Tables

For any data analysis, being able to calculate descriptive statistics and represent them in a table is a must. This section will walk you through the [Table1](https://benjaminrich.github.io/table1/vignettes/table1-examples.html) package, a useful tool in representing descriptive statistics across several variables, as well as [Kable](https://cran.r-project.org/web/packages/kableExtra/vignettes/awesome_table_in_html.html#Installation), included in the [Knitr](https://yihui.org/knitr/) package, which offers abilities to transform dataframes/tibbles into well-formatted and meaningful tables. Just to reiterate, you should be working within an R Markdown file to create an html output; regular R source files will not do the trick. 

First, with our *Iris* dataset, we're interested to learn about some of the measures of central tendency (mean, median, standard deviation, etc.) of the provided data. We have variables representing the sepal length and width, petal length and width, and the species of Iris.

  &nbsp;
  
## Table1
  
To get a quick view at descriptive statistics, we'll use the following code and the Table1 package. 



```{.r .code-style}
# First, I need to ensure my variables are the right data type.
str(iris)
```

```
## 'data.frame':	150 obs. of  5 variables:
##  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```

```{.r .code-style}
# Based on the 'structure' function above, I see I have 4 numeric variables and 1 factor variable, which is exactly what I need.

# Often, categorical data is represented as a character data type, but table1 prefers factor data types
# To change characters to factors, I could use the following code:
# iris$Species <- factor(iris$Species)




# Creating basic table using table1 function
# Note: The variables I'm interested in are included in a 'formula' input:
# ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width
# All variables you want to calculate descriptive stats for are used in the table1() function this way
table1(~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width + Species, data = iris)
```

```{=html}
<div class="Rtable1"><table class="Rtable1">
<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=150)</span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td class='rowlabel firstrow'>Sepal.Length</td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>5.84 (0.828)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>5.80 [4.30, 7.90]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Sepal.Width</td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>3.06 (0.436)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>3.00 [2.00, 4.40]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Petal.Length</td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>3.76 (1.77)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>4.35 [1.00, 6.90]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Petal.Width</td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>1.20 (0.762)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>1.30 [0.100, 2.50]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Species</td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>setosa</td>
<td>50.0 (33.3%)</td>
</tr>
<tr>
<td class='rowlabel'>versicolor</td>
<td>50.0 (33.3%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>virginica</td>
<td class='lastrow'>50.0 (33.3%)</td>
</tr>
</tbody>
</table>
</div>
```


  &nbsp;

Looks good! However, there are many more ways to customize the table1 output. For one, to gain some more meaningful insights, I could stratify my numerical data over the 3 species of Irises. Let's say I am also interested in making this more publication ready, which means I need to change how my variable names appear in the table. Doing so would use the following code:

  &nbsp;
  

```{.r .code-style}
# Adjusting the display of variable names
label(iris$Sepal.Length) <- "Sepal Length"
label(iris$Sepal.Width) <- "Sepal Width"
label(iris$Petal.Length) <- "Petal Length"
label(iris$Petal.Width) <- "Petal Width"

# Adjusting factor display names for each species
iris$Species <- fct_recode(iris$Species,
  "Setosa" = "setosa",
  "Versicolor" = "versicolor",
  "Virginica" = "virginica"
)


# table1 output stratified by Iris species
table1(~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width | Species, data = iris)
```

```{=html}
<div class="Rtable1"><table class="Rtable1">
<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>Setosa<br><span class='stratn'>(N=50)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Versicolor<br><span class='stratn'>(N=50)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Virginica<br><span class='stratn'>(N=50)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=150)</span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td class='rowlabel firstrow'>Sepal Length</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>5.01 (0.352)</td>
<td>5.94 (0.516)</td>
<td>6.59 (0.636)</td>
<td>5.84 (0.828)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>5.00 [4.30, 5.80]</td>
<td class='lastrow'>5.90 [4.90, 7.00]</td>
<td class='lastrow'>6.50 [4.90, 7.90]</td>
<td class='lastrow'>5.80 [4.30, 7.90]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Sepal Width</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>3.43 (0.379)</td>
<td>2.77 (0.314)</td>
<td>2.97 (0.322)</td>
<td>3.06 (0.436)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>3.40 [2.30, 4.40]</td>
<td class='lastrow'>2.80 [2.00, 3.40]</td>
<td class='lastrow'>3.00 [2.20, 3.80]</td>
<td class='lastrow'>3.00 [2.00, 4.40]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Petal Length</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>1.46 (0.174)</td>
<td>4.26 (0.470)</td>
<td>5.55 (0.552)</td>
<td>3.76 (1.77)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>1.50 [1.00, 1.90]</td>
<td class='lastrow'>4.35 [3.00, 5.10]</td>
<td class='lastrow'>5.55 [4.50, 6.90]</td>
<td class='lastrow'>4.35 [1.00, 6.90]</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Petal Width</td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Mean (SD)</td>
<td>0.246 (0.105)</td>
<td>1.33 (0.198)</td>
<td>2.03 (0.275)</td>
<td>1.20 (0.762)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Median [Min, Max]</td>
<td class='lastrow'>0.200 [0.100, 0.600]</td>
<td class='lastrow'>1.30 [1.00, 1.80]</td>
<td class='lastrow'>2.00 [1.40, 2.50]</td>
<td class='lastrow'>1.30 [0.100, 2.50]</td>
</tr>
</tbody>
</table>
</div>
```


  &nbsp;
  
Nice! We can begin to see some of the differences across the different types of species and the table looks great. What I've shown here just scratches the surface of [Table1](https://benjaminrich.github.io/table1/vignettes/table1-examples.html), and I recommend checking out the documentation to see more of what it is capable of.

  &nbsp;
  
## Kable

The kbl() function is included in the knitr package and provides the ability to transform dataframes/tibbles into html formatted tables. It is perfect for developing tables that are easily readable for publications, presentations, reports, etc.

Say we want to represent the descriptive statistics again, but this time using kbl(). To do so, we could use the following code:


```{.r .code-style}
# Creating descriptive statistics
iris_kable <- group_by(iris, Species) %>%
  summarise(
    Count = n(),
    "Sepal Width Mean" = mean(Sepal.Width, na.rm = TRUE),
    "Sepal Width SD" = sd(Sepal.Width, na.rm = TRUE),
    "Petal Width Mean" = mean(Petal.Width, na.rm = TRUE),
    "Petal Width SD" = sd(Petal.Width, na.rm = TRUE),
    "Sepal Length Mean" = mean(Sepal.Length, na.rm = TRUE),
    "Sepal Length SD" = sd(Sepal.Length, na.rm = TRUE),
    "Petal Length Mean" = mean(Petal.Length, na.rm = TRUE),
    "Petal Length SD" = sd(Petal.Length, na.rm = TRUE),
  )

# Representing in a Kable
iris_kable %>%
  kbl() %>%
  add_header_above(c("Descriptive Statistics of Iris Species (N = 150)" = 10)) %>%
  kable_styling(bootstrap_options = c("striped", "condensed"), full_width = F, font_size = 12)
```

<table class="table table-striped table-condensed" style="font-size: 12px; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
<tr><th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="10"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Descriptive Statistics of Iris Species (N = 150)</div></th></tr>
  <tr>
   <th style="text-align:left;"> Species </th>
   <th style="text-align:right;"> Count </th>
   <th style="text-align:right;"> Sepal Width Mean </th>
   <th style="text-align:right;"> Sepal Width SD </th>
   <th style="text-align:right;"> Petal Width Mean </th>
   <th style="text-align:right;"> Petal Width SD </th>
   <th style="text-align:right;"> Sepal Length Mean </th>
   <th style="text-align:right;"> Sepal Length SD </th>
   <th style="text-align:right;"> Petal Length Mean </th>
   <th style="text-align:right;"> Petal Length SD </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Setosa </td>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:right;"> 3.428 </td>
   <td style="text-align:right;"> 0.3790644 </td>
   <td style="text-align:right;"> 0.246 </td>
   <td style="text-align:right;"> 0.1053856 </td>
   <td style="text-align:right;"> 5.006 </td>
   <td style="text-align:right;"> 0.3524897 </td>
   <td style="text-align:right;"> 1.462 </td>
   <td style="text-align:right;"> 0.1736640 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Versicolor </td>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:right;"> 2.770 </td>
   <td style="text-align:right;"> 0.3137983 </td>
   <td style="text-align:right;"> 1.326 </td>
   <td style="text-align:right;"> 0.1977527 </td>
   <td style="text-align:right;"> 5.936 </td>
   <td style="text-align:right;"> 0.5161711 </td>
   <td style="text-align:right;"> 4.260 </td>
   <td style="text-align:right;"> 0.4699110 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Virginica </td>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:right;"> 2.974 </td>
   <td style="text-align:right;"> 0.3224966 </td>
   <td style="text-align:right;"> 2.026 </td>
   <td style="text-align:right;"> 0.2746501 </td>
   <td style="text-align:right;"> 6.588 </td>
   <td style="text-align:right;"> 0.6358796 </td>
   <td style="text-align:right;"> 5.552 </td>
   <td style="text-align:right;"> 0.5518947 </td>
  </tr>
</tbody>
</table>


  &nbsp;

As you can see, we are able to represent the descriptive statistics in a well-formatted table. Table1 still might be the better package for descrptive statistics in this case, but this serves as a good example of what Kable is capable of!

Another table you will likely find yourself needing to create are contingency tables (also known as cross-tabulations). In environmental health research, we are often wanting to compare different groups of participants based on outcomes or characteristics. Kable offers a quick way to create cross-tabulations across several variables!

For our example, we will create a new binary variable to represent if the iris has a sepal width that is below (coded as 0) or above (coded as 1) the median value overall across the 150 samples. After this new variable is created, we will create a cross-tabulation to gain understanding into which species has the greatest number with an above-median sepal width. This could be a quick way to infer how species correlates to sepal width. 


```{.r .code-style}
# Creating new binary variable to represent if an iris has an above/below median sepal width
sepal_width_median <- median(iris$Sepal.Width)
iris <- mutate(iris, Sepal.Width.Median = if_else(iris$Sepal.Width < sepal_width_median, "Below Median", "Above Median"))

# Creating a cross-tab representation of the species of irises and if they have an above or below median sepal width
#First, creating a small dataframe with the variables of interest
iris_kable <- iris%>%
  select(Species, Sepal.Width.Median)
#Changing the variable names to be more meaningful
names(iris_kable) <- c("Species", "Above/Below Median Sepal Width")
#Next, creating the cross-tab using kable
iris_kable%>%
  group_by(Species, `Above/Below Median Sepal Width`)%>%
  summarize(n=n())%>%
  spread(Species, n)%>%
  kable()%>%
  add_header_above(c(" " = 1, "Species" = 3))%>%
  add_header_above(c("Cross-tabulation of Iris Species and sepal Width (N = 150)" = 4))%>%
  kable_styling(bootstrap_options = c("condensed"), full_width = F, font_size = 12)
```

<table class="table table-condensed" style="font-size: 12px; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
<tr><th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="4"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Cross-tabulation of Iris Species and sepal Width (N = 150)</div></th></tr>
<tr>
<th style="empty-cells: hide;border-bottom:hidden;" colspan="1"></th>
<th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="3"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Species</div></th>
</tr>
  <tr>
   <th style="text-align:left;"> Above/Below Median Sepal Width </th>
   <th style="text-align:right;"> Setosa </th>
   <th style="text-align:right;"> Versicolor </th>
   <th style="text-align:right;"> Virginica </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Above Median </td>
   <td style="text-align:right;"> 48 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 29 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Below Median </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 21 </td>
  </tr>
</tbody>
</table>

  &nbsp;
  
From the cross-tabulation, we can see that the Setosa species has the greatest number of irises with a sepal width greater than the median. This cross-tabulation could help inform future statistical analyses to determine if there are any relationships between species and iris size.
  
Everything I have shown you thus far is a practical application of how to create tables in R Markdown. There is *much* more you can do than this and I encourage you to explore! Kable has many more opportunities to create custom tables through the [KableExtra](https://cran.r-project.org/web/packages/kableExtra/vignettes/awesome_table_in_html.html#Overview) package. Any time you have results from analyses (i.e. regression models, etc.), consider representing them in a Kable!

***

# Data Visualization with ggplot2

R offers countless ways to create unique data visualizations. For our Iris dataset, it would be interesting to be able to expand upon our previous cross-tabulation to get a sense of how all of the dimensions of each species compares to one another. A straightforward way to do this would be to look at the distributions of dimensions for each species of Iris through box plots.

The following code makes use of the [ggplot2](https://ggplot2-book.org/index.html) package that is included in the *Tidyverse*. The ggplot2 package offers incredibly useful tools to create data visualizations that go way beyond what basic R is capable of. An additional resource that contains more specific information on ggplot2 can be found [here](https://r-graphics.org/).

When working with ggplot2, you can imagine working with a recipe. Each ingredient, or layer, that you add to your plot will create something new until you have your final product. The code belows demonstrates a variety of ways to use these layers to create a beautiful final product!


```{.r .code-style}
#For this first step, we are simply sending our dataset to a ggplot2 object
plot_1 <- iris%>%
  ggplot() +
#Above, you will notice I used the + symbol. 
#The + symbol is how we add each layer to another!
#Now, we will create our boxplot layer
  geom_boxplot(aes(x = Species, y=Sepal.Width)) +
#Next, we will add labels to each axis
  labs(x = "Species",
       y = "Sepal Width (cm)") +
#Finally, we want to make it look pretty, so we'll add a theme
  theme_bw()



#We will now replicate the same plot for our other 3 measurements. 

plot_2 <- iris%>%
  ggplot() +
  geom_boxplot(aes(x = Species, y=Sepal.Length)) +
  labs(x = "Species",
       y = "Sepal Length (cm)") +
  theme_bw()


plot_3 <- iris%>%
  ggplot() +
  geom_boxplot(aes(x = Species, y=Petal.Width)) +
  labs(x = "Species",
       y = "Petal Width (cm)") +
  theme_bw()

plot_4 <- iris%>%
  ggplot() +
  geom_boxplot(aes(x = Species, y=Petal.Length)) +
  labs(x = "Species",
       y = "Petal Length (cm)") +
  theme_bw()


#Instead of having 4 individual figures, it'd be more helpful to represent them in 1. 
#We will do so using the patchwork package. 
#The patchwork package is helpful for combining ggplot2 objects into 1!

library(patchwork)

#Patchwork can be expanded upon to create some really unique visualizations,
#so be sure to look more into it!


plot_1 + plot_2 + plot_3 + plot_4 + plot_annotation(title = "Petal and Sepal Characteristics of Irises") 
```

<img src="R_Training_Module_Harris_Lab_files/figure-html/Comparing distributions through a ggplot2 visualization-1.png" style="display: block; margin: auto;" />


  &nbsp;

  
Again, there are various geoms (or plot types) that can be used with ggplot2; it is all a matter of finding the right one to represent your data! 





  


***

# Bivariate Data Analysis


Now say we are interested in determining if there is a difference in size of the sepal among different species. To do so, we will be using a one-way ANOVA. More information on how to choose the right statistical test can be found [here](https://stats.idre.ucla.edu/other/mult-pkg/whatstat/). The additional key assumptions for a one-way ANOVA are that the samples are independent of each other and randomly chosen, and that the dependent variable (in our case, sepal width or height) is normally distributed. At a basic level, a histogram can allow us to 'eyeball' it, but there are more rigorous statistical tests out there to determine normality. See the code below for how to create a basic histogram using the ggplot2 package:


```{.r .code-style}
# Creating histogram of sepal width variable
plot_1 <- iris %>%
  ggplot() +
  geom_histogram(aes(y = Sepal.Width), binwidth = 0.1) +
  coord_flip() +
  labs(
    title = "Sepal Width",
    y = "Sepal Width (cm)",
    x = "Frequency"
  ) +
  theme_bw()

plot_1
```

<img src="R_Training_Module_Harris_Lab_files/figure-html/Checking for normality of iris data-1.png" style="display: block; margin: auto;" />


  &nbsp;
  
As we can see, the data are fairly normally distributed, so we can feel more confident about using the one-way ANOVA to compare sepal width across species. Upon obtaining results though, we will want to represent them in a formatted table using the power of Kable so it can be shared to an audience. See the following code for how to do so:


```{.r .code-style}
# Conducting one-way ANOVA and storing model for later formatting

anova_results <- aov(data = iris, Sepal.Width ~ Species)

# The summary() function creates a crude output
summary(anova_results)
```

```
##              Df Sum Sq Mean Sq F value Pr(>F)    
## Species       2  11.35   5.672   49.16 <2e-16 ***
## Residuals   147  16.96   0.115                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```



    
As we can see from the ANOVA results, there is a statistically significant (P < 0.05) relationship between Iris species and sepal width! Using tools from Kable or ggplot2, we can decide to represent these results further to better communicate this relationship and its magnitude of effect.


***
# Conclusion

This small training module was meant to introduce you to helpful tools in R to create tables and visualizations, as well as walking through a small bivariate analysis. However, this module only touches the surface of everything R is capable of. There are vast resources available, and it is recommended you check out all of the many resources that have been linked throughout this document!

Have fun! Best of luck!
