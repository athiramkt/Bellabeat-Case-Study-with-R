# Case Study: Unraveling User Behavior with Bellabeat Data

# Introduction

This repository presents a comprehensive case study on Bellabeat, a fictional wellness company. By leveraging the power of R programming, we delve into a dataset containing various user metrics such as activity levels, sleep patterns, and calories burned. The primary objective of this study is to gain valuable insights into user behavior and preferences, ultimately informing product development and marketing strategies.
## 📕 Table Of Contents
* [SCENARIO](#scenario)
* [ASK](#ask)
* [PREPARE](#prepare)
* [PROCESS](#process)
* [ANALYZE](#analyze)
* [ACT](#act)

## SCENARIO
Bellabeat, a high-tech company that manufactures health-focused smart products wants to analyse the usage of one of their products in order to gain insight into how people are already using their smart devices.Then, using this information, she would like high-level recommendations for how these trends can inform Bellabeat marketing strategy.

## ASK

**Business Tasks**

Analyze smart device usage data to understand consumer behavior with non-Bellabeat devices and apply these insights to enhance a Bellabeat product.

**Key stakeholders**

> 1. Urška Sršen: Bellabeat’s co founder and Chief Creative Officer;
> 2. Sando Mur: Mathematician and Bellabeat cofounder; key member of the Bellabeat executive team;
> 3. Bellabeat marketing analytics team: A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy.

> - Question to guide the analysis:
>    1.  What are some trends in smart device usage?
>    2.  How could these trends apply to Bellabeat customers?
>    3.  How could these trends help influence Bellabeat marketing strategy?

## PREPARE
**Evaluating Data Validity**

For this case study, we'll analyze public FitBit Fitness Tracker data sourced from Kaggle, comprising personal fitness information voluntarily shared by participants. The dataset presents a comprehensive snapshot of user activity and wellness metrics.
> 1. Reliability: The dataset includes data from 30 FitBit users, meeting the minimum sample size requirement for analysis.
> 2. Originality: The data was collected by a third party through Amazon Mechanical Turk surveys.
> 3. Comprehensiveness: It covers various metrics, including daily active time, activity minutes, sleep duration, step counts, and distance covered.
> 4. Current: The data spans one month, from 4/12/2016 to 5/12/2016.
> 5. Citations: The original source is unspecified as the data was collected by a third party.

``` r
#install packages 
library(tidyverse)
library(lubridate)
library(ggplot2)
library(dplyr)
library(knitr)
```
``` r
    ── Attaching core tidyverse packages ──────────────── tidyverse 2.0.0 ──
✔ dplyr     1.1.4     ✔ readr     2.1.5
✔ forcats   1.0.0     ✔ stringr   1.5.1
✔ ggplot2   3.5.1     ✔ tibble    3.2.1
✔ lubridate 1.9.3     ✔ tidyr     1.3.1
✔ purrr     1.0.2     
── Conflicts ────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
ℹ Use the conflicted package to force all conflicts to become errors
```

**Importing data**

We’re going to explore data across 18 CSV files. These files track different metrics at three distinct time intervals:
> 1. Daily summaries, 
> 2. Hourly breakdowns
> 3. Minute-by-minute records

The data follows a long format structure, where each session ID appears across multiple rows, with each row representing a single time point. We can identify individual reports using either their export ID or timestamp. 

By examining patterns across these different time scales, we aim to uncover insights that could benefit Bellabeat's customer base. Let’s start with loading our daily and hourly datasets. We’ll create dataframes for these now:

``` r
#Importing data
daily_activity <- read_csv("dailyActivity_merged.csv")
```
``` r
Rows: 940 Columns: 15                                                   
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr  (1): ActivityDate
dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActi...

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
Session restored from your saved work on 2024-Dec-05 18:37:10 UTC (1 hour ago)
```

``` r
daily_calories <- read_csv("dailyCalories_merged.csv")
```
``` r
Rows: 940 Columns: 3                                                    
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr (1): ActivityDay
dbl (2): Id, Calories

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
daily_intensities <- read_csv("dailyIntensities_merged.csv")
```
``` r
Rows: 940 Columns: 10                                                   
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr (1): ActivityDay
dbl (9): Id, SedentaryMinutes, LightlyActiveMinutes, FairlyActiveMin...

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
``` r
daily_steps <- read_csv("dailySteps_merged.csv")
```
``` r
Rows: 940 Columns: 3                                                    
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr (1): ActivityDay
dbl (2): Id, StepTotal

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
daily_sleep <- read_csv("sleepDay_merged.csv")
```
``` r
Rows: 413 Columns: 5                                                    
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr (1): SleepDay
dbl (4): Id, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
weight_log <- read_csv("weightLogInfo_merged.csv")
```
``` r
Rows: 67 Columns: 8                                                     
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr (1): Date
dbl (6): Id, WeightKg, WeightPounds, Fat, BMI, LogId
lgl (1): IsManualReport

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
hourly_calories <- read_csv("hourlyCalories_merged.csv")
```
``` r
Rows: 22099 Columns: 3                                                  
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr (1): ActivityHour
dbl (2): Id, Calories

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
hourly_intensities <- read_csv("hourlyIntensities_merged.csv")
```
``` r
Rows: 22099 Columns: 4                                                  
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr (1): ActivityHour
dbl (3): Id, TotalIntensity, AverageIntensity

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
``` r
hourly_steps <- read_csv("hourlySteps_merged.csv")
```
``` r
Rows: 22099 Columns: 3                                                  
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr (1): ActivityHour
dbl (2): Id, StepTotal

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
``` r
#Since the minute data is huge, we’re only going to import the smallest dataset:

minute_sleep <- read_csv("minuteSleep_merged.csv")
```
``` r
Rows: 188521 Columns: 4                                                 
── Column specification ────────────────────────────────────────────────
Delimiter: ","
chr (1): date
dbl (3): Id, value, logId

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
## Process

**Clean Data**: To ensure data quality and consistency, we'll clean and tidy the datasets. This involves handling missing values, outliers, and inconsistencies in data formats.

**Create tibbles** : Let’s take a closer look at the different dataframes. We’ll start by using the head() function to view the first few rows of each one:

**Daily dataframes**:
``` r
head(daily_activity)
```
``` r
# A tibble: 6 × 15
          Id ActivityDate TotalSteps TotalDistance TrackerDistance LoggedActivitiesDist…¹
       <dbl> <chr>             <dbl>         <dbl>           <dbl>                  <dbl>
1 1503960366 4/12/2016         13162          8.5             8.5                       0
2 1503960366 4/13/2016         10735          6.97            6.97                      0
3 1503960366 4/14/2016         10460          6.74            6.74                      0
4 1503960366 4/15/2016          9762          6.28            6.28                      0
5 1503960366 4/16/2016         12669          8.16            8.16                      0
6 1503960366 4/17/2016          9705          6.48            6.48                      0
# ℹ abbreviated name: ¹​LoggedActivitiesDistance
# ℹ 9 more variables: VeryActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
#   LightActiveDistance <dbl>, SedentaryActiveDistance <dbl>, VeryActiveMinutes <dbl>,
#   FairlyActiveMinutes <dbl>, LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>,
#   Calories <dbl>
```
``` r
head(daily_calories)
```
``` r
# A tibble: 6 × 3
          Id ActivityDay Calories
       <dbl> <chr>          <dbl>
1 1503960366 4/12/2016       1985
2 1503960366 4/13/2016       1797
3 1503960366 4/14/2016       1776
4 1503960366 4/15/2016       1745
5 1503960366 4/16/2016       1863
6 1503960366 4/17/2016       1728
```
``` r
head(daily_intensities)
```
``` r
# A tibble: 6 × 10
          Id ActivityDay SedentaryMinutes LightlyActiveMinutes FairlyActiveMinutes
       <dbl> <chr>                  <dbl>                <dbl>               <dbl>
1 1503960366 4/12/2016                728                  328                  13
2 1503960366 4/13/2016                776                  217                  19
3 1503960366 4/14/2016               1218                  181                  11
4 1503960366 4/15/2016                726                  209                  34
5 1503960366 4/16/2016                773                  221                  10
6 1503960366 4/17/2016                539                  164                  20
# ℹ 5 more variables: VeryActiveMinutes <dbl>, SedentaryActiveDistance <dbl>,
#   LightActiveDistance <dbl>, ModeratelyActiveDistance <dbl>, VeryActiveDistance <dbl>
```
``` r
head(daily_steps)
```
``` r
# A tibble: 6 × 3
          Id ActivityDay StepTotal
       <dbl> <chr>           <dbl>
1 1503960366 4/12/2016       13162
2 1503960366 4/13/2016       10735
3 1503960366 4/14/2016       10460
4 1503960366 4/15/2016        9762
5 1503960366 4/16/2016       12669
6 1503960366 4/17/2016        9705
```
``` r
head(daily_sleep)
```
``` r
# A tibble: 6 × 5
          Id SleepDay              TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
       <dbl> <chr>                             <dbl>              <dbl>          <dbl>
1 1503960366 4/12/2016 12:00:00 AM                 1                327            346
2 1503960366 4/13/2016 12:00:00 AM                 2                384            407
3 1503960366 4/15/2016 12:00:00 AM                 1                412            442
4 1503960366 4/16/2016 12:00:00 AM                 2                340            367
5 1503960366 4/17/2016 12:00:00 AM                 1                700            712
6 1503960366 4/19/2016 12:00:00 AM                 1                304            320
```
``` r
head(weight_log)
```
``` r
# A tibble: 6 × 8
          Id Date                WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
       <dbl> <chr>                  <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
1 1503960366 5/2/2016 11:59:59 …     52.6         116.    22  22.6 TRUE           1.46e12
2 1503960366 5/3/2016 11:59:59 …     52.6         116.    NA  22.6 TRUE           1.46e12
3 1927972279 4/13/2016 1:08:52 …    134.          294.    NA  47.5 FALSE          1.46e12
4 2873212765 4/21/2016 11:59:59…     56.7         125.    NA  21.5 TRUE           1.46e12
5 2873212765 5/12/2016 11:59:59…     57.3         126.    NA  21.7 TRUE           1.46e12
6 4319703577 4/17/2016 11:59:59…     72.4         160.    25  27.5 TRUE           1.46e12
```
``` r
head(weight_log)
```
``` r
# A tibble: 6 × 8
          Id Date                WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
       <dbl> <chr>                  <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
1 1503960366 5/2/2016 11:59:59 …     52.6         116.    22  22.6 TRUE           1.46e12
2 1503960366 5/3/2016 11:59:59 …     52.6         116.    NA  22.6 TRUE           1.46e12
3 1927972279 4/13/2016 1:08:52 …    134.          294.    NA  47.5 FALSE          1.46e12
4 2873212765 4/21/2016 11:59:59…     56.7         125.    NA  21.5 TRUE           1.46e12
5 2873212765 5/12/2016 11:59:59…     57.3         126.    NA  21.7 TRUE           1.46e12
6 4319703577 4/17/2016 11:59:59…     72.4         160.    25  27.5 TRUE           1.46e12
```
**Hourly dataframes**:
``` r
head(hourly_calories)
```
``` r
# A tibble: 6 × 3
          Id ActivityHour          Calories
       <dbl> <chr>                    <dbl>
1 1503960366 4/12/2016 12:00:00 AM       81
2 1503960366 4/12/2016 1:00:00 AM        61
3 1503960366 4/12/2016 2:00:00 AM        59
4 1503960366 4/12/2016 3:00:00 AM        47
5 1503960366 4/12/2016 4:00:00 AM        48
6 1503960366 4/12/2016 5:00:00 AM        48
```
``` r
head(hourly_intensities)
```
``` r
# A tibble: 6 × 4
          Id ActivityHour          TotalIntensity AverageIntensity
       <dbl> <chr>                          <dbl>            <dbl>
1 1503960366 4/12/2016 12:00:00 AM             20            0.333
2 1503960366 4/12/2016 1:00:00 AM               8            0.133
3 1503960366 4/12/2016 2:00:00 AM               7            0.117
4 1503960366 4/12/2016 3:00:00 AM               0            0    
5 1503960366 4/12/2016 4:00:00 AM               0            0    
6 1503960366 4/12/2016 5:00:00 AM               0            0  
```
``` r
head(hourly_steps)
```
``` r
# A tibble: 6 × 3
          Id ActivityHour          StepTotal
       <dbl> <chr>                     <dbl>
1 1503960366 4/12/2016 12:00:00 AM       373
2 1503960366 4/12/2016 1:00:00 AM        160
3 1503960366 4/12/2016 2:00:00 AM        151
4 1503960366 4/12/2016 3:00:00 AM          0
5 1503960366 4/12/2016 4:00:00 AM          0
6 1503960366 4/12/2016 5:00:00 AM          0
```
**Minute dataframe**:
``` r
head(minute_sleep)
```
``` r
# A tibble: 6 × 4
          Id date                 value       logId
       <dbl> <chr>                <dbl>       <dbl>
1 1503960366 4/12/2016 2:47:30 AM     3 11380564589
2 1503960366 4/12/2016 2:48:30 AM     2 11380564589
3 1503960366 4/12/2016 2:49:30 AM     1 11380564589
4 1503960366 4/12/2016 2:50:30 AM     1 11380564589
5 1503960366 4/12/2016 2:51:30 AM     1 11380564589
6 1503960366 4/12/2016 2:52:30 AM     1 11380564589
```
**View column names**: Now we’ll use the colnames() function to view the column names for each dataframe
**Daily dataframes**:
``` r
colnames(daily_activity)
```
``` r
[1] "Id"                       "ActivityDate"             "TotalSteps"              
 [4] "TotalDistance"            "TrackerDistance"          "LoggedActivitiesDistance"
 [7] "VeryActiveDistance"       "ModeratelyActiveDistance" "LightActiveDistance"     
[10] "SedentaryActiveDistance"  "VeryActiveMinutes"        "FairlyActiveMinutes"     
[13] "LightlyActiveMinutes"     "SedentaryMinutes"         "Calories"
```
``` r
colnames(daily_calories)
```
``` r
[1] "Id"          "ActivityDay" "Calories"  
```
``` r
colnames(daily_intensities)
```
``` r
 [1] "Id"                       "ActivityDay"              "SedentaryMinutes"        
 [4] "LightlyActiveMinutes"     "FairlyActiveMinutes"      "VeryActiveMinutes"       
 [7] "SedentaryActiveDistance"  "LightActiveDistance"      "ModeratelyActiveDistance"
[10] "VeryActiveDistance"
```
``` r
colnames(daily_steps)
```
``` r
[1] "Id"          "ActivityDay" "StepTotal" 
```
``` r
colnames(daily_sleep)
```
``` r
[1] "Id"                 "SleepDay"           "TotalSleepRecords"  "TotalMinutesAsleep"
[5] "TotalTimeInBed"
```
``` r
colnames(weight_log)
```
``` r
[1] "Id"             "Date"           "WeightKg"       "WeightPounds"   "Fat"           
[6] "BMI"            "IsManualReport" "LogId"
```
**Hourly dataframes**:
``` r
colnames(hourly_calories)
```
``` r
[1] "Id"           "ActivityHour" "Calories" 
```
``` r
colnames(hourly_intensities)
```
``` r
[1] "Id"               "ActivityHour"     "TotalIntensity"   "AverageIntensity"
```
``` r
colnames(hourly_steps)
```
``` r
[1] "Id"           "ActivityHour" "StepTotal"   
```
```
**Minute dataframes**:

``` r
colnames(minute_sleep)
```
``` r
[1] "Id"    "date"  "value" "logId"
```
**Viewing Variables**

Each dataframe contains an **Id field**, which can serve as a key to merge them into comprehensive datasets (e.g., daily or hourly dataframes). However, the formatting of the activity date/time variables appears inconsistent across some dataframes. To investigate further, we use the select() function to isolate and examine these variables more closely.
``` r
daily_activity %>%
  select(Id, ActivityDate)
```

``` r
# A tibble: 940 × 2
           Id ActivityDate
        <dbl> <chr>       
 1 1503960366 4/12/2016   
 2 1503960366 4/13/2016   
 3 1503960366 4/14/2016   
 4 1503960366 4/15/2016   
 5 1503960366 4/16/2016   
 6 1503960366 4/17/2016   
 7 1503960366 4/18/2016   
 8 1503960366 4/19/2016   
 9 1503960366 4/20/2016   
10 1503960366 4/21/2016   
# ℹ 930 more rows
# ℹ Use `print(n = ...)` to see more rows
```
``` r
daily_calories %>%
  select(Id, ActivityDay)
```
``` r
# A tibble: 940 × 2
           Id ActivityDay
        <dbl> <chr>      
 1 1503960366 4/12/2016  
 2 1503960366 4/13/2016  
 3 1503960366 4/14/2016  
 4 1503960366 4/15/2016  
 5 1503960366 4/16/2016  
 6 1503960366 4/17/2016  
 7 1503960366 4/18/2016  
 8 1503960366 4/19/2016  
 9 1503960366 4/20/2016  
10 1503960366 4/21/2016  
# ℹ 930 more rows
# ℹ Use `print(n = ...)` to see more rows
```
``` r
daily_intensities %>%
select(Id, ActivityDay)
```
``` r
# A tibble: 940 × 2
           Id ActivityDay
        <dbl> <chr>      
 1 1503960366 4/12/2016  
 2 1503960366 4/13/2016  
 3 1503960366 4/14/2016  
 4 1503960366 4/15/2016  
 5 1503960366 4/16/2016  
 6 1503960366 4/17/2016  
 7 1503960366 4/18/2016  
 8 1503960366 4/19/2016  
 9 1503960366 4/20/2016  
10 1503960366 4/21/2016  
# ℹ 930 more rows
# ℹ Use `print(n = ...)` to see more rows
```
``` r
daily_steps %>%
select(Id, ActivityDay)
```
``` r
# A tibble: 940 × 2
           Id ActivityDay
        <dbl> <chr>      
 1 1503960366 4/12/2016  
 2 1503960366 4/13/2016  
 3 1503960366 4/14/2016  
 4 1503960366 4/15/2016  
 5 1503960366 4/16/2016  
 6 1503960366 4/17/2016  
 7 1503960366 4/18/2016  
 8 1503960366 4/19/2016  
 9 1503960366 4/20/2016  
10 1503960366 4/21/2016  
# ℹ 930 more rows
# ℹ Use `print(n = ...)` to see more rows
```
``` r
daily_sleep %>%
select(Id, SleepDay)
```
``` r
# A tibble: 413 × 2
           Id SleepDay             
        <dbl> <chr>                
 1 1503960366 4/12/2016 12:00:00 AM
 2 1503960366 4/13/2016 12:00:00 AM
 3 1503960366 4/15/2016 12:00:00 AM
 4 1503960366 4/16/2016 12:00:00 AM
 5 1503960366 4/17/2016 12:00:00 AM
 6 1503960366 4/19/2016 12:00:00 AM
 7 1503960366 4/20/2016 12:00:00 AM
 8 1503960366 4/21/2016 12:00:00 AM
 9 1503960366 4/23/2016 12:00:00 AM
10 1503960366 4/24/2016 12:00:00 AM
# ℹ 403 more rows
# ℹ Use `print(n = ...)` to see more rows
```
``` r
weight_log %>%
select(Id, Date)

# A tibble: 67 × 2
           Id Date                 
        <dbl> <chr>                
 1 1503960366 5/2/2016 11:59:59 PM 
 2 1503960366 5/3/2016 11:59:59 PM 
 3 1927972279 4/13/2016 1:08:52 AM 
 4 2873212765 4/21/2016 11:59:59 PM
 5 2873212765 5/12/2016 11:59:59 PM
 6 4319703577 4/17/2016 11:59:59 PM
 7 4319703577 5/4/2016 11:59:59 PM 
 8 4558609924 4/18/2016 11:59:59 PM
 9 4558609924 4/25/2016 11:59:59 PM
10 4558609924 5/1/2016 11:59:59 PM 
# ℹ 57 more rows
# ℹ Use `print(n = ...)` to see more rows
```
**Findings** : The weight log and sleep records contain fewer entries compared to other datasets.
