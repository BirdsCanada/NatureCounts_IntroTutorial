# Data Manipulation {#Manip5}



You have successfully downloaded your NatureCounts dataset and are ready to explore and summarize the data. In this chapter we will demonstrate how to do some basic data manipulations and summaries. The possibilities are endless, so we try to focus on examples we think would be most valuable to users. We intend to develop `collection` and `protocol_id` specific data manipulation and analysis code in the future. If you have specific requests or would like to contribute your existing code, please contact dethier@birdscanada.org. 

## Basic data wrangling {#Manip5.1}

Recall in [Chapter 2](#Package2.2) you installed the [tidyverse](https://www.tidyverse.org/) package, which included dplyr for data manipulations. You are encouraged to learn more about this function by reviewing the [Data Transformations](https://r4ds.had.co.nz/transform.html) chapter in "R for Data Science" by Hadley Wickham and Garrett Grolemund. We also recommend you download a copy of the RStudio [Data Wrangling](https://rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf) cheat sheet as a reference document.


We are going to apply three key dplyr functions which will give you some of the basic tools needed to solve the vast majority of your data manipulation challenges. These functions include:

  - `select()`: Pick variables by their names 
  - `filter()`: Pick observations by their values 
  - `summarise()`: Collapse many values down to a single summary (often with `group_by()`)

Let's continue using the [Ontario Whip-poor-will](https://www.birdscanada.org/birdmon/default/datasets.jsp?code=WPWI) collection for this chapter.   


```r
WPWI <- nc_data_dl(collections = "WPWI", username = "sample", info = "tutorial example")
```

```
## Using filters: collections (WPWI); fields_set (BMDE2.00-min)
```

```
## Collecting available records...
```

```
##   access collection nrecords
## 1    yes       WPWI     3012
## Total records: 3,012
```

```
## 
## Downloading records for each collection:
```

```
##   WPWI
```

```
##     Records 1 to 3012 / 3012
```

### Select {#Manip5.1.1}

You will notice that NatureCounts datasets have many fields (i.e., columns) available. Generally, you will only want a few of these fields for your summary or analysis. Recall the complete version of the [BMDE](#Data3.1) includes 265 fields. The number of variables downloaded from the BMDE will depend on the `fields_set`, which is by default "minimum" (57 fields). You can narrow in on the variables you are interested in with the `select()` function, which allows you to subset the dataframe based on the names of the variables.

For example, we are going to `select()` a subset of variables from the WPWI dataset that we need for our summary: 


```r
WPWI_select <- select(WPWI, "ObservationCount", "SurveyAreaIdentifier", 
                      "RouteIdentifier", "species_id", "latitude", 
                      "longitude", "bcr", "survey_day", "survey_month", 
                      "survey_year")
WPWI_select
```

```
##   ObservationCount SurveyAreaIdentifier RouteIdentifier species_id latitude
## 1             <NA>                 2W-1              2W         NA 50.99415
## 2                0                 2W-2              2W       7870 50.99208
## 3             <NA>                 2W-3              2W       7870 50.98019
##   longitude bcr survey_day survey_month survey_year
## 1 -94.16117   8         24            6        2010
## 2 -94.13520   8         24            6        2010
## 3 -94.11525   8         24            6        2010
##  [ reached 'max' / getOption("max.print") -- omitted 3009 rows ]
```

### Filter {#Manip5.1.2}

Often there are observational records in a database that are not needed. We could have filtered these records out using the `nc_data_dl()` [filters](#Dowload4.2). However, we can also use the `dplyr::filter()` function, which allows us to subset observations based on their row values. Observations are selected using:

  - Comparison operators:`>`, `>=`, `<`, `<=`, `!=` (not equal), `==` (equal) 
  - Logical operators: `&` (and), `|` (or),  `!` (is not)

There are worked examples provided in the naturecounts article [Filtering data after download](https://birdstudiescanada.github.io/naturecounts/articles/filtering-data.html) to get you started with applying filters. These include:

  - Categorical filters
  - Numerical filters
  - Date filters

Here we provide a few additional examples for you to work with. First, lets apply a simple `filter()` that subsets the data based on a single survey month: 


```r
WPWI_June <- filter(WPWI_select, survey_month == 6)
WPWI_June
```

```
##   ObservationCount SurveyAreaIdentifier RouteIdentifier species_id latitude
## 1             <NA>                 2W-1              2W         NA 50.99415
## 2                0                 2W-2              2W       7870 50.99208
## 3             <NA>                 2W-3              2W       7870 50.98019
##   longitude bcr survey_day survey_month survey_year
## 1 -94.16117   8         24            6        2010
## 2 -94.13520   8         24            6        2010
## 3 -94.11525   8         24            6        2010
##  [ reached 'max' / getOption("max.print") -- omitted 2546 rows ]
```

Now lets try multiple survey months by adding a logical operator:

```r
WPWI_JJ <- filter(WPWI_select, survey_month == 6 | survey_month == 7)

#Alternatively this can be written as:
WPWI_JJ <- filter(WPWI_select, survey_month %in% c(6,7))
WPWI_JJ
```

```
##   ObservationCount SurveyAreaIdentifier RouteIdentifier species_id latitude
## 1             <NA>                 2W-1              2W         NA 50.99415
## 2                0                 2W-2              2W       7870 50.99208
## 3             <NA>                 2W-3              2W       7870 50.98019
##   longitude bcr survey_day survey_month survey_year
## 1 -94.16117   8         24            6        2010
## 2 -94.13520   8         24            6        2010
## 3 -94.11525   8         24            6        2010
##  [ reached 'max' / getOption("max.print") -- omitted 2602 rows ]
```

We can continue to add to the complexity of our filter:

```r
WPWI_multi <- filter(WPWI_select, 
                     survey_month %in% c(6,7) & bcr == 12 & 
                       survey_year >= 2010 & survey_year <= 2012)
WPWI_multi
```

```
##   ObservationCount SurveyAreaIdentifier RouteIdentifier species_id latitude
## 1             <NA>                  3-1               3         NA 44.79400
## 2             <NA>                  3-3               3         NA 44.80629
## 3             <NA>                  3-4               3         NA 44.82274
##   longitude bcr survey_day survey_month survey_year
## 1 -79.38787  12         24            6        2010
## 2 -79.34094  12         24            6        2010
## 3 -79.34187  12         24            6        2010
##  [ reached 'max' / getOption("max.print") -- omitted 1082 rows ]
```

### Summarise {#Manip5.1.3}

Now that we have selected the columns we need for our analysis and limited the data to the observation records we wish to use, we will want to summarise what we have left! The `summarise()` function is most useful if paired with the `group_by()` argument, because this changes the unit of analysis from the complete dataset to individual groups.

For example, say we want to determine how many point count stops were on each Route. We will want to group observations by `RouteIdentifier` and summarise the number of district `SurveyAreaIdentifier`. 


```r
WPWI_Route <- group_by(WPWI_multi, RouteIdentifier)
WPWI_Route <- summarise(WPWI_Route, Nstops = n_distinct(SurveyAreaIdentifier))
```

However, now that we're getting on to more complex operations, we'll introduce the 
use of the pipe `%>%`. This allows you to pass (pipe) the output of one line 
as input to the next line. Therefore, the previous code can be re-written as:


```r
WPWI_Route <- WPWI_multi %>% 
  group_by(RouteIdentifier) %>% 
  summarise(Nstops = n_distinct(SurveyAreaIdentifier))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```r
WPWI_Route
```

```
## # A tibble: 62 x 2
##    RouteIdentifier Nstops
##    <chr>            <int>
##  1 104                 10
##  2 11                  10
##  3 119                  3
##  4 124                 10
##  5 13                  10
##  6 137                 10
##  7 138                 10
##  8 143                 10
##  9 161                 10
## 10 165                 10
## # ... with 52 more rows
```

Note that there is a pipe (`%>%`) between each set of lines and that the data (`WPWI_multi`)
is only referred to once, at the very start. These two different codes achieve the 
exact same result.


Back to our example, we might also want to know how many routes where run in each year. 


```r
WPWI_Year <- WPWI_multi %>% 
  group_by(survey_year) %>% 
  summarise(Nroute = n_distinct(RouteIdentifier))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```r
WPWI_Year
```

```
## # A tibble: 3 x 2
##   survey_year Nroute
##         <int>  <int>
## 1        2010     40
## 2        2011     46
## 3        2012      9
```

Finally, lets look to see how many Observations (`ObservationCount`) of WPWI were made on each route in each year. First we need to replace any `NA`s with `0` and ensure this variable is numeric.


```r
WPWI_multi$ObservationCount[is.na(WPWI_multi$ObservationCount)] <- 0

WPWI_Obs <- WPWI_multi %>% 
  group_by(RouteIdentifier, survey_year) %>% 
  summarise(MeanObs = mean(as.numeric(ObservationCount)))
```

```
## `summarise()` regrouping output by 'RouteIdentifier' (override with `.groups` argument)
```

```r
WPWI_Obs
```

```
## # A tibble: 95 x 3
## # Groups:   RouteIdentifier [62]
##    RouteIdentifier survey_year MeanObs
##    <chr>                 <int>   <dbl>
##  1 104                    2010       0
##  2 104                    2011       0
##  3 11                     2010       0
##  4 11                     2011       0
##  5 119                    2010       0
##  6 119                    2011       0
##  7 119                    2012       0
##  8 124                    2010       0
##  9 124                    2011       0
## 10 13                     2010       0
## # ... with 85 more rows
```

No WIWP were detected in our subset of the data!! No wonder this species is listed as [threatened](https://www.ontario.ca/page/eastern-whip-poor-will) in the province. 

## Helper Functions {#Manip5.2}

There are a few additional helper functions built into the naturecounts R package that you may find useful. At present, they include: 

- [`format_dates()`](https://birdstudiescanada.github.io/naturecounts/reference/format_dates.html):Creates and adds date and day-of-year (doy) field/columns to data
- [`formate_zero_fill()`](https://birdstudiescanada.github.io/naturecounts/reference/format_zero_fill.html): Zero-fill the species presence data by adding zero observations counts (absences) to an existing NatureCounts dataset

The zero-fill function is particularly important if you want to ensure your dataset is complete!

## Exercies {#Manip5.3}

*Exercise 1*:  You (user "sample") are doing a research project using the fall migration monitoring data collected at [Vaseux Lake Bird Observatory](https://www.birdscanada.org/birdmon/default/datasets.jsp?code=CMMN-DET-VLBO), British Columbia. You request the open access data from 2017-2020. After you request this subset of the collection, you need to determine the number of unique days Gray catbirds were records in each year?

Answer: 
2017 =	54
2018 =	53

*Exercise 2*: Building off the same dataset used in Exercise 1, you now want to zero-fill the dataframe to ensure it is complete for your study on Gray catbirds. How many records (rows) are in the zero-fill Gray catbird dataframe?

Answer: 152

Answers to the exercises can be found in [Chapter 7](#Ans7.3).
