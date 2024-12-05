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

     ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
     ✔ dplyr     1.1.1     ✔ readr     2.1.4
     ✔ forcats   1.0.0     ✔ stringr   1.5.0
     ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
     ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
     ✔ purrr     1.0.1     
     ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
     ✖ dplyr::filter() masks stats::filter()
     ✖ dplyr::lag()    masks stats::lag()
     ℹ Use the ]8;;http://conflicted.r-lib.org/conflicted package]8;; to force all conflicts to become errors

