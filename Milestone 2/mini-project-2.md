Mini Data Analysis Milestone 2
================
Eully Ao

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

- Making summary tables and graphs
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
library(broom)
library(here)
```

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  **Are there trends in the species of trees planted over the years?
    For example, are certain species planted more in certain
    neighbourhoods, and did certain species only get planted in the
    earlier years?**
2.  **Is there a relationship between the species of tree versus where
    it’s planted?**
3.  **What are the average traits for each species (diameter, height
    range)?**
4.  **Are there trends in the species of trees that are planted along
    roads versus clustered in parks?**

<!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

**First, I will load the dataset as a dataframe.**

``` r
# load as a dataframe into the environment
dataset <- vancouver_trees
```

### Question 1

**My first research question is: Are there trends in the species of
trees planted over the years? For example, are certain species planted
more in certain neighbourhoods, and did certain species only get planted
in the earlier years? This is a rather broad question, but because I am
interested in the species of the trees, I can first determine how many
species there are in the dataset and how many trees there are of each
species.**

**Reference: [dplyr](https://dplyr.tidyverse.org/reference/index.html)**

``` r
# summarizing how many species there are in this dataset and how many trees there are for each species
# n_distinct counts the unique observations, using it with dataset$species_name because I want to know the number of unique observations in the column "species"
species_number <- n_distinct(dataset$species_name)
species_number
```

    ## [1] 283

``` r
# determining how many trees there are of each species
species_counts <- dataset %>%
  select(tree_id, species_name) %>% # subset to make a new dataframe with only species and tree id
  group_by(species_name) %>% # group observations by species
  summarize(number = n()) %>% # and summarize to know how many trees are in each species
  arrange(desc(number)) # reordering in descending order 

head(species_counts)
```

    ## # A tibble: 6 × 2
    ##   species_name number
    ##   <chr>         <int>
    ## 1 SERRULATA     13357
    ## 2 CERASIFERA    12031
    ## 3 PLATANOIDES   11963
    ## 4 RUBRUM         8467
    ## 5 AMERICANA      5515
    ## 6 SYLVATICA      5285

**The work above results in a dataframe where there are counts for the
283 species of trees. This is helpful because even though I do not know
yet how many of each species were planted in each year, I can still get
a sense of the data I am working with. I can then also create a graph to
visualize the spread of the data.**

**References: [dplyr](https://dplyr.tidyverse.org/reference/index.html),
[ggplot labs](https://ggplot2.tidyverse.org/reference/labs.html),
[reordering
bars](https://stackoverflow.com/questions/25664007/reorder-bars-in-geom-bar-ggplot2-by-value),
[rotating
labels](https://www.geeksforgeeks.org/rotating-and-spacing-axis-labels-in-ggplot2-in-r/)**

``` r
# using ggplot to visualize the number of trees per species 
ggplot(species_counts, aes(x = reorder(species_name, -number), y = number)) +
         geom_bar(stat = "identity") +
         labs(x = "Species", y = "Number of trees", title = "Number of trees per species") + #labelling axis and title
         scale_y_log10() + # log10 to scale 
         theme(axis.text.x = element_text(angle = 45, hjust = 1)) # rotate axis
```

![](mini-project-2_files/figure-gfm/Q1.2.1%20plot-1.png)<!-- -->

**The result looks like a nightmare! 283 species in one plot is too
many, so I will plot a random 50 instead.**

``` r
# the result is a nightmare - 283 species is way too many! will plot a random 50 instead.

set.seed(1)  # set seed so this random list can be reproduced
random_50 <- sample(unique(dataset$species_name), 50)
species_50 <- species_counts %>%
  filter(species_name %in% random_50)

# view the random list
head(species_50)
```

    ## # A tibble: 6 × 2
    ##   species_name  number
    ##   <chr>          <int>
    ## 1 RUBRUM          8467
    ## 2 FREEMANI   X    4164
    ## 3 HIPPOCASTANUM   2277
    ## 4 PENNSYLVANICA   1612
    ## 5 ROBUR           1521
    ## 6 RETICULATA      1294

``` r
# plot number of trees per species
ggplot(species_50, aes(x = reorder(species_name, -number), y = number)) +
  geom_bar(stat = "identity") +
  labs(x = "Species", y = "Number of trees", title = "Number of trees per species") +
  scale_y_log10() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1, size = 7))
```

![](mini-project-2_files/figure-gfm/Q1.2.1%20random%20sample%20plot-1.png)<!-- -->

**The second plot plots a random 50 species from the first plot, so it’s
slightly more useful since we can see the names of the species, however,
seeing all the species in one plot would have been more useful.**

### Question 2

**My next question is: Is there a relationship between the species of
tree versus where it is planted? For the first task, I can summarize how
many trees per species are planted in each area (indicated by the
observations in the variable ’plant_area”).**

``` r
# subsetting the data needed
plant_area_df <- dataset %>%
  select(tree_id, species_name, plant_area) 

#view subsetted data
head(plant_area_df)
```

    ## # A tibble: 6 × 3
    ##   tree_id species_name plant_area
    ##     <dbl> <chr>        <chr>     
    ## 1  149556 AMERICANA    N         
    ## 2  149563 SERRATA      N         
    ## 3  149579 JAPONICA     4         
    ## 4  149590 AMERICANA    4         
    ## 5  149604 CAMPESTRE    4         
    ## 6  149616 CALLERYANA   B

``` r
# seeing what the different observations are for plant_area
areas <- unique(plant_area_df$plant_area)
areas
```

    ##  [1] "N"  "4"  "B"  "6"  "3"  "5"  "2"  NA   "10" "C"  "7"  "8"  "12" "25" "40"
    ## [16] "9"  "17" "1"  "24" "11" "20" "13" "15" "16" "G"  "18" "b"  "14" "30" "c" 
    ## [31] "L"  "P"  "50" "34" "60" "M"  "21" "35" "n"  "75" "45" "19" "0"  "g"  "22"
    ## [46] "y"  "27" "32" "26"

**We can see that there are 49 different observations. The
[description](https://rdrr.io/github/UBC-MDS/datateachr/man/vancouver_trees.html)
of this dataset states that for plant area, B = behind sidewalk, G = in
tree gate, N = no sidewalk, C = cutout, and a number indicates boulevard
width in feet. There are observations such as “G”, “P”, “L”, “M”, and
also missing observations (NA), which I will remove because there is no
description for those. Observations such as “b” and “c” will be kept,
assuming they are the same as “B” and “C”. Numbers will be renamed to
“V” to indicate the tree being planted in a boulevard.**

**References: [lower to
uppercase](https://stackoverflow.com/questions/16516593/convert-from-lowercase-to-uppercase-all-values-in-all-character-variables-in-dat#),
[renaming
characters](https://dplyr.tidyverse.org/reference/rename.html),
[stringr](https://cran.r-project.org/web/packages/stringr/vignettes/regular-expressions.html),
[grepl](https://stackoverflow.com/questions/59354928/regular-expressions-and-substring-in-r)**

``` r
# keeping only the rows in which the observations for plant_area are not NA, L, P, M, or y (aka keeping only B, G, N, C, b, g, n, c, and numbers)
plant_area_filtered <- plant_area_df %>%
  filter(!plant_area %in% c(NA, "L", "P", "M", "y")) %>%
  mutate(plant_area = str_to_upper(plant_area)) %>% # changing lowercase to uppercase
  mutate(plant_area = ifelse(grepl("^\\d+$", plant_area), "V", plant_area)) # changing numbers to be represented by the observation V
head(plant_area_filtered)
```

    ## # A tibble: 6 × 3
    ##   tree_id species_name plant_area
    ##     <dbl> <chr>        <chr>     
    ## 1  149556 AMERICANA    N         
    ## 2  149563 SERRATA      N         
    ## 3  149579 JAPONICA     V         
    ## 4  149590 AMERICANA    V         
    ## 5  149604 CAMPESTRE    V         
    ## 6  149616 CALLERYANA   B

``` r
# checking what observations there are now in the filtered dataset
areas_filtered <- unique(plant_area_filtered$plant_area)
areas_filtered # now only has B, G, N, C, and V
```

    ## [1] "N" "V" "B" "C" "G"

``` r
# summarizing how many trees in each species is in each area
plant_area_counts <- plant_area_filtered %>%
  group_by(species_name, plant_area) %>%
  summarize(number = n())
```

    ## `summarise()` has grouped output by 'species_name'. You can override using the
    ## `.groups` argument.

``` r
# calculating the percentage of trees per species in each plant area
plant_area_counts <- plant_area_counts %>%
  mutate(percent = number / sum(number) * 100)

# view final dataframe
head(plant_area_counts)
```

    ## # A tibble: 6 × 4
    ## # Groups:   species_name [2]
    ##   species_name   plant_area number percent
    ##   <chr>          <chr>       <int>   <dbl>
    ## 1 ABIES          B              49  37.1  
    ## 2 ABIES          C               1   0.758
    ## 3 ABIES          N              35  26.5  
    ## 4 ABIES          V              47  35.6  
    ## 5 ACERIFOLIA   X B             135   7.84 
    ## 6 ACERIFOLIA   X C             115   6.68

**Now, I can make a plot that shows the percentage of trees per species
planted in each area. Again, I will just plot the same random 50 species
as before.**

**Reference: [grouped bar
plot](https://r-graph-gallery.com/48-grouped-barplot-with-ggplot2)**

``` r
# subset dataset to only have the random 50 species
areas_50 <- plant_area_counts %>%
  filter(species_name %in% random_50)
head(areas_50)
```

    ## # A tibble: 6 × 4
    ## # Groups:   species_name [2]
    ##   species_name plant_area number percent
    ##   <chr>        <chr>       <int>   <dbl>
    ## 1 ACUTISSIMA   B              27    5.15
    ## 2 ACUTISSIMA   C              50    9.54
    ## 3 ACUTISSIMA   G               8    1.53
    ## 4 ACUTISSIMA   N              51    9.73
    ## 5 ACUTISSIMA   V             388   74.0 
    ## 6 AMURENSE     N               2   40

``` r
# creating stacked bar plot
ggplot(areas_50, aes(x = species_name, y = percent, fill = plant_area )) +
  geom_bar(position = "fill", stat = "identity") +
  labs(x = "Species", y = "Percentage of Trees", title = "Proportion of trees per species in each area", fill = "Area") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1, size = 6)) +
  theme(legend.position = "right")
```

![](mini-project-2_files/figure-gfm/Q1.2.2%20plot-1.png)<!-- -->

**This plot is useful because we can see that some species are planted
only in one area, whereas some are planted in all 4 areas, however, it
should be kept in mind that this plot shows proportions. For example,
100% of Spectabilis trees (6 in total) are not on sidewalks (N), while
100% of Strobus are in a boulevard, but there’s only 1 tree.**

### **Question 3**

**My next question is: What are the average traits for each species
(diameter, height range)? As a task, I can find the range, mean, median,
and standard deviation of the diameter within each species. I will also
be removing the trees with a diameter of 0.00.**

``` r
# subset dataset and create a new dataframe
diameter_df <- dataset %>%
  select(tree_id, species_name, diameter) %>%
  filter(diameter != 0.00) # remove the trees with a diameter of 0.00
head(diameter_df)
```

    ## # A tibble: 6 × 3
    ##   tree_id species_name diameter
    ##     <dbl> <chr>           <dbl>
    ## 1  149556 AMERICANA          10
    ## 2  149563 SERRATA            10
    ## 3  149579 JAPONICA            4
    ## 4  149590 AMERICANA          18
    ## 5  149604 CAMPESTRE           9
    ## 6  149616 CALLERYANA          5

``` r
# calculate the summary statistics 
diameter_stats <- diameter_df %>%
  group_by(species_name) %>%
  summarise(
    range = max(diameter) - min(diameter),
    mean = mean(diameter),
    median = median(diameter),
    sd = sd(diameter))

# view final dataframe
head(diameter_stats)
```

    ## # A tibble: 6 × 5
    ##   species_name   range  mean median    sd
    ##   <chr>          <dbl> <dbl>  <dbl> <dbl>
    ## 1 ABIES           33   12.9    12    7.51
    ## 2 ACERIFOLIA   X  55   20.8    19   11.8 
    ## 3 ACUMINATA       26   10.9     7    9.91
    ## 4 ACUTISSIMA      34    8.87    8.5  4.33
    ## 5 AILANTHIFOLIA   16   32      34    6.44
    ## 6 ALBA            38.5 19.4    20.2 14.4

**The table nicely summarizes these statistics and will be useful for
creating a plot.**

``` r
# subset the random 50 species
diameter_stats_50 <- diameter_stats %>%
  filter(species_name %in% random_50)
head(diameter_stats_50) # this isn't tidy
```

    ## # A tibble: 6 × 5
    ##   species_name range  mean median    sd
    ##   <chr>        <dbl> <dbl>  <dbl> <dbl>
    ## 1 ACUTISSIMA    34    8.87   8.5   4.33
    ## 2 AMURENSE      12   18.1   19     5.03
    ## 3 AQUIFOLIUM    40   12.6   12     6.73
    ## 4 ARBOREUM       2.5  3      3     1.77
    ## 5 ATLANTICA     45   16.5    9.62 14.9 
    ## 6 BABYLONICA    37.5 23.6   23     9.68

``` r
# use pivot_longer to make the data frame tidy, with a new column for the name of the statistic
diameter_stats_50_tidy <- diameter_stats_50 %>%
  pivot_longer(
    cols = -species_name, # not changing species column
    names_to = "statistic", # new column called statistic
    values_to = "value") %>% # column for values
  filter(value >= 1)

#view tidied dataframe
head(diameter_stats_50_tidy)
```

    ## # A tibble: 6 × 3
    ##   species_name statistic value
    ##   <chr>        <chr>     <dbl>
    ## 1 ACUTISSIMA   range     34   
    ## 2 ACUTISSIMA   mean       8.87
    ## 3 ACUTISSIMA   median     8.5 
    ## 4 ACUTISSIMA   sd         4.33
    ## 5 AMURENSE     range     12   
    ## 6 AMURENSE     mean      18.1

``` r
# can now do a grouped bar plot
ggplot(diameter_stats_50_tidy, aes(fill = statistic, y = value, x = species_name)) +
  geom_bar(position = "dodge", stat = "identity") +
  labs(x = "Species", y = "Value", title = "Summary statistics comparison between species") +
  scale_y_log10() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1, size = 5))
```

![](mini-project-2_files/figure-gfm/Q1.2.3%20plot-1.png)<!-- -->

**This plot is still difficult to read with 50 species, but does allow
the visualization of the summary statistics, and we can see trends like
how most species have a large range in diameter.**

### **Question 4**

**My last question is: Are there trends in the species of trees that are
planted along roads versus clustered in parks? While this may be
interesting to investigate, this question is difficult to answer without
somehow mapping the longitude and latitude coordinates to determine
whether the trees are in parks. I will change the question to look at
the what percentage of trees within each species is planted on the
curb.**

``` r
# subset dataset and create a new dataframe
curb_df <- dataset %>%
  select(tree_id, species_name, curb)
head(curb_df)
```

    ## # A tibble: 6 × 3
    ##   tree_id species_name curb 
    ##     <dbl> <chr>        <chr>
    ## 1  149556 AMERICANA    N    
    ## 2  149563 SERRATA      N    
    ## 3  149579 JAPONICA     Y    
    ## 4  149590 AMERICANA    Y    
    ## 5  149604 CAMPESTRE    Y    
    ## 6  149616 CALLERYANA   Y

``` r
# summarizing how many trees in each species are on the curb
curb_counts <- curb_df %>%
  group_by(species_name, curb) %>%
  summarize(number = n())
```

    ## `summarise()` has grouped output by 'species_name'. You can override using the
    ## `.groups` argument.

``` r
head(curb_counts)
```

    ## # A tibble: 6 × 3
    ## # Groups:   species_name [4]
    ##   species_name   curb  number
    ##   <chr>          <chr>  <int>
    ## 1 ABIES          N         26
    ## 2 ABIES          Y        113
    ## 3 ACERIFOLIA   X N        176
    ## 4 ACERIFOLIA   X Y       1548
    ## 5 ACUMINATA      Y          7
    ## 6 ACUTISSIMA     N          4

``` r
# calculating the percentage of trees per species that are on the curb or not
curb_counts <- curb_counts %>%
  mutate(percent = number / sum(number) * 100)

# view final dataframe
head(curb_counts)
```

    ## # A tibble: 6 × 4
    ## # Groups:   species_name [4]
    ##   species_name   curb  number percent
    ##   <chr>          <chr>  <int>   <dbl>
    ## 1 ABIES          N         26  18.7  
    ## 2 ABIES          Y        113  81.3  
    ## 3 ACERIFOLIA   X N        176  10.2  
    ## 4 ACERIFOLIA   X Y       1548  89.8  
    ## 5 ACUMINATA      Y          7 100    
    ## 6 ACUTISSIMA     N          4   0.760

**Again, I will plot 50 of the species in a stacked bar plot.**

``` r
# subset dataset to only have the random 50 species
curb_50 <- curb_counts %>%
  filter(species_name %in% random_50)
head(curb_50)
```

    ## # A tibble: 6 × 4
    ## # Groups:   species_name [4]
    ##   species_name curb  number percent
    ##   <chr>        <chr>  <int>   <dbl>
    ## 1 ACUTISSIMA   N          4   0.760
    ## 2 ACUTISSIMA   Y        522  99.2  
    ## 3 AMURENSE     Y          5 100    
    ## 4 AQUIFOLIUM   N         33  21.3  
    ## 5 AQUIFOLIUM   Y        122  78.7  
    ## 6 ARBOREUM     N          1  50

``` r
# creating stacked bar plot
ggplot(curb_50, aes(x = species_name, y = percent, fill = curb )) +
  geom_bar(position = "fill", stat = "identity") +
  labs(x = "Species", y = "Percentage of Trees", title = "Proportion of trees per species planted on a curb", fill = "Curb") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1, size = 6)) +
  theme(legend.position = "right")
```

![](mini-project-2_files/figure-gfm/Q1.2.4%20plot-1.png)<!-- -->

**This plot makes it easier to visualize that most most species are
planted both on and off the curb, with more being planted on the curb.**

<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

**I think having the subsetted dataframes help with answering the
research questions, plus the graphs had some interested results. For
example, the plot for the third question shows that many trees have a
large range in diameter, and the plot for the second question shows that
most species are planted in all 4 areas, the majority being in
boulevards. However, because all my questions were related to using the
species variable, the datasets are still quite large since there are 283
species. I think using a smaller dataset and investigating a more
specific question would be an improvement. For example, I can narrow it
down to one species and see if there is a correlation between two
variables, such diameter and planted area. Or, I can use genus rather
than species. Plus, rather than randomly picking a certain number of
species, I can also filter out the categories that do not fit certain
criteria (ex. only keep the species that exceed a certain proportion
threshold in a neighbourhood).**

<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

**The dataset has more than 8 variables, so I will be selecting 8.**

``` r
# subset dataset and create a new dataframe
untidy <- dataset %>%
  select(1:8)

head(untidy)
```

    ## # A tibble: 6 × 8
    ##   tree_id civic_number std_street genus_name species_name cultivar_name  
    ##     <dbl>        <dbl> <chr>      <chr>      <chr>        <chr>          
    ## 1  149556          494 W 58TH AV  ULMUS      AMERICANA    BRANDON        
    ## 2  149563          450 W 58TH AV  ZELKOVA    SERRATA      <NA>           
    ## 3  149579         4994 WINDSOR ST STYRAX     JAPONICA     <NA>           
    ## 4  149590          858 E 39TH AV  FRAXINUS   AMERICANA    AUTUMN APPLAUSE
    ## 5  149604         5032 WINDSOR ST ACER       CAMPESTRE    <NA>           
    ## 6  149616          585 W 61ST AV  PYRUS      CALLERYANA   CHANTICLEER    
    ## # ℹ 2 more variables: common_name <chr>, assigned <chr>

``` r
colnames(untidy)
```

    ## [1] "tree_id"       "civic_number"  "std_street"    "genus_name"   
    ## [5] "species_name"  "cultivar_name" "common_name"   "assigned"

**The 8 variables are `tree_id`, `civic_number`, `std_street`,
`genus_name`, `species_name`, `cultivar_name`, `common_name`, and
`assigned`. `tree_id` is the ID given to each tree, `civic_number` is
the street address of the site at which the tree is associated with,
`std_street` is the street name of the site at which the tree is
associated with, `assigned` indicates whether the address is made up to
associate the tree with a nearby lot, and the rest are the genus,
species, cultivar, and common names of the tree. Each row contains
multiple observations of each tree (each of the variables). Therefore,
this dataset is not tidy.**

<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

**The data is untidy, so I will tidy it such that each row represents an
observation. To do so, I will use `pivot_longer` to create a new column
that has variables indicating which observation (ex. genus name, species
name) is in that row. In other words, a row would be split into
multiple, so there will be multiple rows for the same tree.**

**Before tidying:**

``` r
head(untidy)
```

    ## # A tibble: 6 × 8
    ##   tree_id civic_number std_street genus_name species_name cultivar_name  
    ##     <dbl>        <dbl> <chr>      <chr>      <chr>        <chr>          
    ## 1  149556          494 W 58TH AV  ULMUS      AMERICANA    BRANDON        
    ## 2  149563          450 W 58TH AV  ZELKOVA    SERRATA      <NA>           
    ## 3  149579         4994 WINDSOR ST STYRAX     JAPONICA     <NA>           
    ## 4  149590          858 E 39TH AV  FRAXINUS   AMERICANA    AUTUMN APPLAUSE
    ## 5  149604         5032 WINDSOR ST ACER       CAMPESTRE    <NA>           
    ## 6  149616          585 W 61ST AV  PYRUS      CALLERYANA   CHANTICLEER    
    ## # ℹ 2 more variables: common_name <chr>, assigned <chr>

**After tidying:**

``` r
tidy <- untidy %>%
  mutate(civic_number = as.character(civic_number)) %>% # civic_number is numeric, so changing it to character so all the columns to be pivoted are the same data type
  pivot_longer(
    cols = -tree_id, # not changing id column
    names_to = "descriptor", # new column called descriptor to store all the variables from the old dataset
    values_to = "data") # column to store the data

head(tidy)
```

    ## # A tibble: 6 × 3
    ##   tree_id descriptor    data       
    ##     <dbl> <chr>         <chr>      
    ## 1  149556 civic_number  494        
    ## 2  149556 std_street    W 58TH AV  
    ## 3  149556 genus_name    ULMUS      
    ## 4  149556 species_name  AMERICANA  
    ## 5  149556 cultivar_name BRANDON    
    ## 6  149556 common_name   BRANDON ELM

<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  **What is the relationship between height and diameter of genera
    with over 1000 trees planted?**
2.  **Are there any trends in the tree genera with more than 1000 trees
    planted in neighborhoods?**

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

1.  **Originally, I wanted to determine average traits for each species
    (diameter, height range), however, this is more of a task rather
    exploring whether there is a relationship between the two variables
    (thus being a more interesting question). So, I can investigate
    whether there is a relationship between the traits diameter and
    height. Plus, because there are so many species, using genus instead
    could still yield interesting results while decreasing the number of
    categories, in addition to selecting only the top few genera.**
2.  **I think it would still be interesting to look at whether there are
    preferences for certain trees in each neighbourhood, but because
    there are so many species, using genus instead would decrease the
    number of categories and increase the number of trees in each
    category.**

<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidying,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

### **Question 1**

**What is the relationship between height and diameter of genera with
over 1000 trees planted?**

``` r
# find the number of trees in each genus
genus_counts <- dataset %>%
  group_by(genus_name) %>%
  summarize(number = n()) %>%
  arrange(desc(number))

# keep the genera with more than 1000 trees
genera_1000 <- genus_counts %>%
  filter(number >= 1000)

# subset dataset and create a new dataframe
height_diameter <- dataset %>%
  filter(genus_name %in% genera_1000$genus_name) %>%
  group_by(genus_name) %>%
  summarize(mean_height = mean(height_range_id), 
            mean_diameter = mean(diameter))

# view final dataframe
head(height_diameter)
```

    ## # A tibble: 6 × 3
    ##   genus_name     mean_height mean_diameter
    ##   <chr>                <dbl>         <dbl>
    ## 1 ACER                  2.73         10.6 
    ## 2 AESCULUS              4.60         23.7 
    ## 3 BETULA                4.48         17.9 
    ## 4 CARPINUS              2.27          9.33
    ## 5 CERCIDIPHYLLUM        1.88          6.90
    ## 6 CORNUS                1.80          8.02

``` r
# plot to see relationship
ggplot(height_diameter, aes(x = mean_height, y = mean_diameter, color = genus_name)) +
  geom_point(size = 4, alpha = 0.5) +
  labs(x = "Mean height", y = "Mean diameter", title = "Relationship between mean height and mean diameter for genera with >1000 trees")
```

![](mini-project-2_files/figure-gfm/Q2.3.1%20relationship%20between%20height%20and%20diameter-1.png)<!-- -->

### **Question 2**

**Are there any trends in the tree genera (those with more than 1000
trees) planted in neighborhoods?**

``` r
# subset, keeping the trees whose genus they belong are in the list previously created
genus_nh <- dataset %>%
  filter(genus_name %in% genera_1000$genus_name)

# counts per genus in each neighborhood
genus_nh<- genus_nh %>%
  count(neighbourhood_name, genus_name)

# view final dataframe
head(genus_nh)
```

    ## # A tibble: 6 × 3
    ##   neighbourhood_name genus_name         n
    ##   <chr>              <chr>          <int>
    ## 1 ARBUTUS-RIDGE      ACER             867
    ## 2 ARBUTUS-RIDGE      AESCULUS         134
    ## 3 ARBUTUS-RIDGE      BETULA            77
    ## 4 ARBUTUS-RIDGE      CARPINUS         167
    ## 5 ARBUTUS-RIDGE      CERCIDIPHYLLUM   100
    ## 6 ARBUTUS-RIDGE      CORNUS            58

``` r
ggplot(genus_nh, aes(x = neighbourhood_name, y = n, fill = genus_name)) +
  geom_bar(stat = "identity") +
  labs(x = "Neighbourhood", y = "Trees (n)", fill = "Genus", title = "Trends in Genera with >1000 Trees Planted") +
  theme(
    axis.text.x = element_text(angle = 65, hjust = 1, size = 6),  # Adjust the text size
    legend.text = element_text(size = 6))
```

![](mini-project-2_files/figure-gfm/Q2.3.2%20trends%20in%20the%20tree%20genera%20planted%20in%20neighbourhoods-1.png)<!-- -->

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: **What is the relationship between height and
diameter of genera with over 1000 trees planted?**

**Variable of interest**: **height**

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

``` r
# fit model using lm()
height_diameter_lm <- lm(mean_height ~ mean_diameter, data = height_diameter)

# print output
print(height_diameter_lm)
```

    ## 
    ## Call:
    ## lm(formula = mean_height ~ mean_diameter, data = height_diameter)
    ## 
    ## Coefficients:
    ##   (Intercept)  mean_diameter  
    ##        0.6565         0.1848

**The model states that for every 1 unit change in diameter (1 inch),
height increases by approx 0.18 units (1 unit = 10 feet, so 0.18 unit =
1.8 feet = 21.6 inches). 1 unit of diamater is 1 inch at breast height,
while 1 unit of height is 10 feet (it should be noted that the
measurement is a range - ex. a height of “2” isn’t 20 feet, it’s 20-30
feet.); 0.18 of 10 feet is 1.8 feet or 21.6 inches. So, the model
suggests that mean height is expected to change 21.6 inches (again, an
approximate because height is reported as a range) for every 1 inch of
change in mean diameter. This makes sense because trees are typically
grow taller faster than they increase in diameter.**

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

**I will produce a p-value to determine whether mean diameter is a
significant predictor of mean height.**

``` r
# produce tibble using tidy()
lm_tidy <- broom::tidy(height_diameter_lm)

# print result, the column of interest is the p.value column.
print(lm_tidy)
```

    ## # A tibble: 2 × 5
    ##   term          estimate std.error statistic  p.value
    ##   <chr>            <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)      0.657    0.139       4.73 1.02e- 4
    ## 2 mean_diameter    0.185    0.0113     16.3  9.33e-14

**Using the `broom` package’s function `tidy()`, a p.value of 9.3e-14
for the term mean_diameter is produced. Because it is a very small
value, this suggests that `mean_diameter` is a significant predictor of
`mean_height` for those genera.**

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
# specifying the path for the folder "output"
folder_path <- here("output")
folder_path
```

    ## [1] "/Users/eully/Desktop/courses/STAT 545/mda_eullyao/output"

``` r
# create folder
dir.create(folder_path)
```

    ## Warning in dir.create(folder_path): '/Users/eully/Desktop/courses/STAT
    ## 545/mda_eullyao/output' already exists

``` r
# write .csv file in that folder
csv_path <- here("output", "plant_area_counts.csv")
write_csv(plant_area_counts, file = csv_path)
```

**The code above specifies a path for the .csv file
`plant_area_counts`** **to be saved in the “output” folder that was
created.**

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
# specify path for the model object
model_path <- here("output", "lm.rds" )

# write as RDS
saveRDS(height_diameter_lm, file = model_path)

# read RDS
lm_rds <- readRDS(model_path)
lm_rds
```

    ## 
    ## Call:
    ## lm(formula = mean_height ~ mean_diameter, data = height_diameter)
    ## 
    ## Coefficients:
    ##   (Intercept)  mean_diameter  
    ##        0.6565         0.1848

``` r
# checking if the RDS is the same as the model object
identical(lm_rds, height_diameter_lm)
```

    ## [1] TRUE

**The code above specifies a path for the RDS `lm_rds`** **to be saved
in the “output” folder. `identical()`** **was used to check if the
object, when read using `readRDS`, produces the same object.**

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
