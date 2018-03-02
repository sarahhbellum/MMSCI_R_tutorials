Intro to Data Science in R: Part 2
================
Sarah McGough
7/24/2017

Part 2: R Basics – Transforming, Subsetting, and Merging Data
-------------------------------------------------------------

We have now successfully cleaned the data and prepped the variables for analysis. Here, we will learn how to slice and dice our data several different ways, which is useful when you want to isolate particular subsets of variables, such as only individuals who experience back pain.

We have provided you with a clean dataset for use in this part of the tutorial. Let’s load and inspect it.

``` r
clean_data <- read.csv("clean_data.csv")
dim(clean_data)
```

    ## [1] 54 14

``` r
head(clean_data)
```

    ##   id visit_number visit_date back_pain back_pain_date gi_bleed_outcome
    ## 1  1            1    4/26/08        No           <NA>               No
    ## 2  1            2     5/1/08        No           <NA>               No
    ## 3  1            3   10/19/08       Yes        10/3/08               No
    ## 4  1            4    10/4/09        No           <NA>               No
    ## 5  1            5    11/4/09        No           <NA>              Yes
    ## 6  1            6   12/23/09        No           <NA>               No
    ##   gi_bleed_date medication_analgesic analgesic_start_date
    ## 1          <NA>                 <NA>                 <NA>
    ## 2          <NA>                 <NA>                 <NA>
    ## 3          <NA>                 <NA>                 <NA>
    ## 4          <NA>                coxib              4/26/09
    ## 5      10/29/09                 <NA>                 <NA>
    ## 6          <NA>                 <NA>                 <NA>
    ##   medication_warfarin warfarin_start_date age    sex  race
    ## 1                <NA>                <NA>  72 female white
    ## 2                <NA>                <NA>  72 female white
    ## 3                <NA>                <NA>  72 female white
    ## 4                <NA>                <NA>  72 female white
    ## 5                <NA>                <NA>  72 female white
    ## 6                <NA>                <NA>  72 female white

In this section, we will perform each command two ways: first, using base R syntax, and second, using the **dplyr** **%&gt;%** pipe.

``` r
library(dplyr)
```

**Goal:** Subset back pain data to make it usable for our sub-analysis.

We will isolate only the individuals with back pain and save it into a new data frame called back\_pain. Note that the back\_pain variable has only two values: “Yes” and “No.” Since in R, these are character formats (meaning they are words), we will always enclose them in quotation marks. In base R, the command is subset, while in dplyr, we tell R to filter all of the observations with back\_pain==“Yes”.

``` r
back_pain <- subset(clean_data, back_pain=="Yes") # base R
back_pain <- clean_data %>% filter(back_pain=="Yes") # dplyr
```

Let’s check it out. Did it work?

``` r
head(back_pain)
```

    ##   id visit_number visit_date back_pain back_pain_date gi_bleed_outcome
    ## 1  1            3   10/19/08       Yes        10/3/08               No
    ## 2  2            3    6/26/10       Yes        5/17/10               No
    ## 3  4            3    4/26/10       Yes         4/2/10               No
    ## 4  5            3    5/31/11       Yes        4/29/11               No
    ## 5  6            3    5/11/09       Yes         5/5/09               No
    ## 6  8            2     3/6/09       Yes         1/2/09               No
    ##   gi_bleed_date medication_analgesic analgesic_start_date
    ## 1          <NA>                 <NA>                 <NA>
    ## 2          <NA>                 <NA>                 <NA>
    ## 3          <NA>                 <NA>                 <NA>
    ## 4          <NA>                 <NA>                 <NA>
    ## 5          <NA>                 <NA>                 <NA>
    ## 6          <NA>                 <NA>                 <NA>
    ##   medication_warfarin warfarin_start_date age    sex  race
    ## 1                <NA>                <NA>  72 female white
    ## 2                <NA>                <NA>  79 female black
    ## 3                <NA>                <NA>  73   male white
    ## 4                <NA>                <NA>  70 female white
    ## 5                <NA>                <NA>  82   male white
    ## 6                <NA>                <NA>  82 female black

We have a ton of variables left in the data, but we probably don’t need all of them. Let’s restrict to 2 relevant variables: ID and back pain date. Here, with dplyr, we use select to choose the columns. In both cases, we select multiple columns using c(). Using **base R** (first example), we must enclose the variable names in quotes to indicate that we are specifying objects (columns) within back\_pain. However, in dplyr (second example), the **%&gt;%** allows us to forgo quotes because R already knows that you are “looking inside” the back\_pain data.

``` r
back_pain_subset <- back_pain[,c("id", "back_pain_date")]
#or
back_pain_subset <- back_pain %>% select(c(id,back_pain_date))
```

**Review: How could you do this using only the column numbers?**

Let’s check the data again:

``` r
head(back_pain)
```

    ##   id visit_number visit_date back_pain back_pain_date gi_bleed_outcome
    ## 1  1            3   10/19/08       Yes        10/3/08               No
    ## 2  2            3    6/26/10       Yes        5/17/10               No
    ## 3  4            3    4/26/10       Yes         4/2/10               No
    ## 4  5            3    5/31/11       Yes        4/29/11               No
    ## 5  6            3    5/11/09       Yes         5/5/09               No
    ## 6  8            2     3/6/09       Yes         1/2/09               No
    ##   gi_bleed_date medication_analgesic analgesic_start_date
    ## 1          <NA>                 <NA>                 <NA>
    ## 2          <NA>                 <NA>                 <NA>
    ## 3          <NA>                 <NA>                 <NA>
    ## 4          <NA>                 <NA>                 <NA>
    ## 5          <NA>                 <NA>                 <NA>
    ## 6          <NA>                 <NA>                 <NA>
    ##   medication_warfarin warfarin_start_date age    sex  race
    ## 1                <NA>                <NA>  72 female white
    ## 2                <NA>                <NA>  79 female black
    ## 3                <NA>                <NA>  73   male white
    ## 4                <NA>                <NA>  70 female white
    ## 5                <NA>                <NA>  82   male white
    ## 6                <NA>                <NA>  82 female black

``` r
back_pain[1:5,]
```

    ##   id visit_number visit_date back_pain back_pain_date gi_bleed_outcome
    ## 1  1            3   10/19/08       Yes        10/3/08               No
    ## 2  2            3    6/26/10       Yes        5/17/10               No
    ## 3  4            3    4/26/10       Yes         4/2/10               No
    ## 4  5            3    5/31/11       Yes        4/29/11               No
    ## 5  6            3    5/11/09       Yes         5/5/09               No
    ##   gi_bleed_date medication_analgesic analgesic_start_date
    ## 1          <NA>                 <NA>                 <NA>
    ## 2          <NA>                 <NA>                 <NA>
    ## 3          <NA>                 <NA>                 <NA>
    ## 4          <NA>                 <NA>                 <NA>
    ## 5          <NA>                 <NA>                 <NA>
    ##   medication_warfarin warfarin_start_date age    sex  race
    ## 1                <NA>                <NA>  72 female white
    ## 2                <NA>                <NA>  79 female black
    ## 3                <NA>                <NA>  73   male white
    ## 4                <NA>                <NA>  70 female white
    ## 5                <NA>                <NA>  82   male white

Say we are interested in looking at new use of coxib or NSAID analgesic medication among people who have back pain. We’ve already restricted our data to those with back\_pain==“Yes”, so we can circle back to our clean\_data and extract new use of these medicines: once again, we will subset. Then, we will perform a merge of the medicine data with the back pain data.

``` r
new_users <- subset(clean_data, medication_analgesic=="nsaid" | medication_analgesic=="coxib") # base R way
#or
new_users <- clean_data %>% filter((medication_analgesic=="nsaid") | (medication_analgesic=="coxib")) # dplyr way
```

In the new\_users data, we really only care about 3 variables: id, medication\_analgesic, and analgesic\_start\_date. Let’s keep these 3 variables.

``` r
new_users_subset <- new_users[,c("id", "medication_analgesic", "analgesic_start_date")]  # base R way
#or
new_users_subset <- new_users %>% select(c(id,medication_analgesic,analgesic_start_date)) # dplyr way
```

Now, we can finally perform a merge of our 2 datasets: back\_pain\_subset and new\_users\_subset. What this means is we will combine the information on back pain with the information on the medicine use and medicine start date. How does R know how to combine the information? It needs a unique identifier by which the data will be merged. This is why we kept the “id” variable in each data frame: we want the right information to be assigned to the right people / observations. For each individual person (given by their ID number), R will join the correct back pain and medicines data.

To perform a merge, base R uses the command merge. However, you can also use the equivalent in dplyr, inner\_join, which has almost identical syntax, but is about 43x faster than merge. This makes a difference when working with huge datasets. You tell R which variable(s) to join the information on using by=“variable name”. In our case, we are merging on the common variable “id.”

``` r
full_data <- merge(back_pain_subset, new_users_subset, by="id", all=FALSE)
#or
full_data <- inner_join(back_pain_subset, new_users_subset, by="id")
```

If you want to merge by 2 or more variables, you would specify *by=c(“var1”,“var2”,“etc”)*, where var1 and var2 and so forth are your different variable names.

Here we will discover the utility of the *as.Date* format by subtracting the date of back pain and the medication start date.

First, let’s convert all dates to R date format:

``` r
full_data$back_pain_date <- as.Date(full_data$back_pain_date, "%m/%d/%y") 
full_data$analgesic_start_date <- as.Date(full_data$analgesic_start_date, "%m/%d/%y") 
```

Now we can subtract the dates to find out how much time elapsed between symptoms (back pain) and medication use:

``` r
full_data$time_elapsed <- full_data$analgesic_start_date - full_data$back_pain_date

head(full_data)
```

    ##   id back_pain_date medication_analgesic analgesic_start_date time_elapsed
    ## 1  1     2008-10-03                coxib           2009-04-26     205 days
    ## 2  2     2010-05-17                coxib           2010-09-10     116 days
    ## 3  4     2010-04-02                coxib           2010-07-27     116 days
    ## 4  5     2011-04-29                nsaid           2011-10-23     177 days
    ## 5  6     2009-05-05                nsaid           2009-10-21     169 days
    ## 6  8     2009-01-02                nsaid           2009-09-07     248 days

Lastly, we can extract demographic data from our original dataset (data\_clean) and merge it with this data.

It makes sense to obtain information on age, sex, and race from each individuals’ first clinical visit.

``` r
first_visit <- subset(clean_data,visit_number==1)
#or
first_visit <- clean_data %>% filter(visit_number==1)
```

We can save the variables of interest, and remember to include “id” because we will later be merging the data based on ID number.

``` r
dem_vars <- first_visit[,c("id","age","sex","race")]
#or
dem_vars <- clean_data %>% filter(visit_number==1) %>% select(c(id,age,sex,race))

dem_vars
```

    ##   id age    sex  race
    ## 1  1  72 female white
    ## 2  2  79 female black
    ## 3  3  79 female white
    ## 4  4  73   male white
    ## 5  5  70 female white
    ## 6  6  82   male white
    ## 7  8  82 female black
    ## 8  9  67 female white
    ## 9 10  73   male white

Finally, we’ll perform a merge of the data:

``` r
full_data2 <- merge(full_data, dem_vars, by="id", all=F)
full_data2 <- inner_join(full_data, dem_vars, by="id", all=F)
```

Inspect to see if it worked:

``` r
full_data2
```

    ##   id back_pain_date medication_analgesic analgesic_start_date time_elapsed
    ## 1  1     2008-10-03                coxib           2009-04-26     205 days
    ## 2  2     2010-05-17                coxib           2010-09-10     116 days
    ## 3  4     2010-04-02                coxib           2010-07-27     116 days
    ## 4  5     2011-04-29                nsaid           2011-10-23     177 days
    ## 5  6     2009-05-05                nsaid           2009-10-21     169 days
    ## 6  8     2009-01-02                nsaid           2009-09-07     248 days
    ##   age    sex  race
    ## 1  72 female white
    ## 2  79 female black
    ## 3  73   male white
    ## 4  70 female white
    ## 5  82   male white
    ## 6  82 female black

Remember that your working directory can be used to load data as well as store data. So, we can save our new data frame to the working directory on the computer – this is the only way that you can save a “hard copy” of the transformed data. If you quit R without doing this, you will lose all of the data manipulation of the last 2 hours.

We use *write.csv(data.frame.in.R, “filename.csv”)* to save our data frame in R to the working directory.

``` r
write.csv(full_data2, “full_data2.csv”)
```

Congratulations! You have now performed an assortment of commands to deal with missing data, outliers, and dates, as well as transform data frames by subsetting and merging relevant information for analysis. These steps should help give you ideas of how to properly format and prep your own data for your own analysis.

R Command Cheat Sheet
---------------------

**Missing variables**

-   is.na()

-   sum(is.na)

-   na.omit()

-   complete.cases()

**Dates**

-   as.Date(data.frame$date.column, “input date format”)

**Subset data to specific values**

-   data.frame\[c(rows.I.want), c(columns.I.want)\]

-   subset(data.frame, “criteria I want fulfilled for selection”)

-   data.frame %&gt;% filter(“criteria I want fulfilled for selection)

**Select columns**

-   data.frame\[,c(“columns I want”)\]

-   data.frame %&gt;% select(c(“columns I want”))

**Merge 2 data frames**

-   merge(data.frame.1, data.frame.2, by=“1 common variable”)

-   merge(data.frame.1, data.frame.2, by=c(“2+ common variables”))

**Logical operators**

-   **==** is equal to

-   **!=** not equal to

-   **|** or

-   **&** and

-   **&lt;** less than

-   **&lt;=** less than or equal to

-   **&gt;** greater than

-   **&gt;=** greater than or equal to

**Saving data to the working directory**

-   write.csv(data.frame.in.R, “filename.csv”)

Help me!

**?** will return the R help documentation for a given function. This is useful if you already know the name of the function for which you would like assistance. For instance,

-   ?is.na

-   ?subset

**??** acts as a search across all installed packages and base R. For instance,

**??filter** will return search results for help pages related to “filter” - including the dplyr command that you used today - and a base R command called “Filter.”
