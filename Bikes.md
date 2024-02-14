BIKES
================
By JOHN MWANGI
2024-02-14

![](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/image1_hH9B4gs.width-1600.format-webp.webp)

# Executive Summary

This analysis employs a data-driven approach to maximize annual
memberships for sustained growth. As a junior data analyst in Cyclistic
marketing analytics team, I will utilize R Studio for data cleaning and
analysis. The dataset that contains historical bike trip data will be
evaluated using the **Ask, Prepare, Process, Analyze, Share, and Act**
framework. The objective is to identify disparities in usage patterns
between casual riders and annual members, thereby informing a strategic
marketing initiative.

I will Insights generated from the analysis through professional data
visualizations using Tableau. These visualizations aim to provide
Cyclistic executives, including the detail-oriented executive team led
by Lily Moreno, with compelling and actionable data insights. The
emphasis lies in substantiating marketing recommendations through a
thorough understanding of user behavior. We hope to register more casual
riders into profitable annual membership program. The analysis and
visualizations will serve as a foundation for a targeted marketing
strategy, aligning with Cyclistic’s mission and future objectives.

# Introduction

Cyclistic is fictional bike-share program that boasts a fleet of 5,824
bicycles and 692 docking stations. While the majority of users engage in
leisurely rides, a notable 30% utilize the bikes for daily commutes. The
company offers three flexible pricing plans – single-ride passes,
full-day passes, and annual memberships, distinguishing between casual
riders and member riders accordingly.

# Scenario

As a Junior Data Analyst within Cyclistic’s marketing analyst team, the
focus is on strategic analysis. Cyclistic’s financial analysts have
underscored the profitability of annual memberships over casual
ridership. The Director of Marketing, recognizing the pivotal role of
annual memberships in the company’s future success, sets a goal to
maximize their numbers. The overarching marketing strategy centers on
converting casual riders into coveted annual members. The case study
will adhere to a systematic six-step data analysis process: **Ask,
Prepare, Process, Analyze, Share, and Act.**

# 1. Ask: Defining the Business Task

The business task at hand revolves around understanding the different
usage patterns of Cyclistic bikes between annual members and casual
riders. This analysis is important for the future marketing program
because it aims to maximize annual memberships. The insights derived
from the analysis will lay the foundation for the development of a
targeted marketing strategy. This strategy will help convert casual
riders into loyal annual members. THis plan is aligned with Cyclistic’s
goal of enhancing profitability through increased membership.

### 1.1 Identification of the Business Task

The primary question guiding this phase is: **How do annual members and
casual riders use Cyclistic bikes differently?** This question
necessitates a nuanced exploration of various aspects, including ride
duration, frequency, preferred routes, and temporal patterns. Unraveling
these distinctions is crucial for tailoring marketing initiatives that
specifically resonate with each user group, optimizing the likelihood of
conversion from casual riders to annual members.

### 1.2 Consideration of Key Stakeholders

The stakeholders involved in this business task include Lily Moreno, the
director of marketing, the Cyclistic marketing analytics team whcih I am
a part of. We are responsible for data analysis and generating insight
from the data. Finally, the Cyclistic executive team is tasked with
approving the marketing program. hence, addressing the user behavior
will empower the marketing team to make informed decisions. Besides, it
will provide a solid foundation for the subsequent phases of the
analysis process.

Therefore, the Ask phase sets the stage for a comprehensive examination
of the disparities in bike usage between annual members and casual
riders. This understanding is pivotal for generating actionable insights
that will inform strategic marketing decisions, ultimately contributing
to the attainment of Cyclistic’s business objectives.

# 2. Prepare: Data Acquisition and Organization

In the Prepare phase of the Cyclistic bike-share analysis, my focus is
on obtaining and organizing the historical trip data from Cyclistic to
facilitate a robust analysis of user behaviors. This involves addressing
key questions related to data location, organization, bias, credibility,
licensing, privacy, security, accessibility, data integrity, and its
relevance to answering the primary business question.

### 2.1 Data Location and Organization

[The historical trip
data](https://divvy-tripdata.s3.amazonaws.com/index.html) for the last
12 months has been made accessible by Motivate International Inc. The
datasets appropriate for addressing the business questions. The data is
downloadable, enabling the analysis of different customer types’
utilization of Cyclistic bikes. It is essential to note that due to
privacy constraints, personally identifiable information of riders
cannot be utilized, preventing the connection of pass purchases to
credit card details for specific user profiling. I selected data files
for the 12 months from January 2023 through December 2023. Each file was
downloaded, saved as a .csv file and named consistently to keep the
files organized and easy to identify.The dataset can be downloaded
[here](https://divvy-tripdata.s3.amazonaws.com/index.html). The codes
below were used to import the data into R:

``` r
library(tidyverse) #helps wrangle data
```

    ## Warning: package 'ggplot2' was built under R version 4.3.2

    ## Warning: package 'tibble' was built under R version 4.3.1

    ## Warning: package 'dplyr' was built under R version 4.3.2

``` r
# Use the conflicted package to manage conflicts
library(conflicted)
# Set dplyr::filter and dplyr::lag as the default choices
conflict_prefer("filter", "dplyr")
conflict_prefer("lag", "dplyr")

data1 <- read_csv("202301-divvy-tripdata.csv")
data2 <- read_csv("202302-divvy-tripdata.csv")
data3 <- read_csv("202303-divvy-tripdata.csv")
data4 <- read_csv("202304-divvy-tripdata.csv")
data5 <- read_csv("202305-divvy-tripdata.csv")
data6 <- read_csv("202306-divvy-tripdata.csv")
data7 <- read_csv("202307-divvy-tripdata.csv")
data8 <- read_csv("202308-divvy-tripdata.csv")
data9 <- read_csv("202309-divvy-tripdata.csv")
data10 <- read_csv("202310-divvy-tripdata.csv")
data11 <- read_csv("202311-divvy-tripdata.csv")
data12 <- read_csv("202312-divvy-tripdata.csv")

# I used colnames() on each new data frame to make sure all have the same 13 columns.
colnames(data1)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
#I then used rbind to combine the 12 dataframes into one dataframe and name it, bike_rides
bike_data <- rbind(data1, data2, data3, data4, data5, data6, data7, data8, data9, data10, data11, data12)
#After combining the 12 dataframes into one dataframe, I used the function rm() to remove the 12 individual dataframes from the environment to free up RAM
rm(data1, data2, data3, data4, data5, data6, data7, data8, data9, data10, data11, data12)
head(bike_data)
```

    ## # A tibble: 6 × 13
    ##   ride_id          rideable_type started_at          ended_at           
    ##   <chr>            <chr>         <dttm>              <dttm>             
    ## 1 F96D5A74A3E41399 electric_bike 2023-01-21 20:05:42 2023-01-21 20:16:33
    ## 2 13CB7EB698CEDB88 classic_bike  2023-01-10 15:37:36 2023-01-10 15:46:05
    ## 3 BD88A2E670661CE5 electric_bike 2023-01-02 07:51:57 2023-01-02 08:05:11
    ## 4 C90792D034FED968 classic_bike  2023-01-22 10:52:58 2023-01-22 11:01:44
    ## 5 3397017529188E8A classic_bike  2023-01-12 13:58:01 2023-01-12 14:13:20
    ## 6 58E68156DAE3E311 electric_bike 2023-01-31 07:18:03 2023-01-31 07:21:16
    ## # ℹ 9 more variables: start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

### 2.2 Bias, Credibility, and ROCCC Evaluation

In assessing the data, I prioritize considerations for bias and
credibility. The ROCCC framework— **Reliability, Objectivity, Currency,
Comprehensiveness, and Consistency** —is applied to ensure the data’s
quality and suitability for analysis. This involves examining the data
for potential biases, ensuring objectivity in its representation,
verifying its currency and comprehensiveness, and confirming consistency
across different dimensions. *Our data Rocks*.

### 2.3 Licensing, Privacy, Security, and Accessibility

A critical aspect of the Prepare phase is addressing licensing, privacy,
security, and accessibility concerns. The data, provided under Motivate
International Inc.’s license, must adhere to ethical and legal
standards. Privacy issues necessitate the exclusion of personally
identifiable information. Security measures are implemented to safeguard
the data, and efforts are made to ensure accessibility to all relevant
team members involved in the analysis.

### 2.4 Data Verification and Problem Identification

Data integrity is validated by assessing its accuracy and completeness.
The process involves sorting and filtering the data to identify any
anomalies or inconsistencies. Verification steps are crucial to ensuring
the reliability of subsequent analyses.

#### 2.4.1 Exploring the data

I’ll use class(), dim(), colnames(), and colSums(is.na()) to perform an
initial inspection.

``` r
#confirm dataset is a data frame
class(bike_data)
```

    ## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"

``` r
#confirm # of rows and columns
dim(bike_data)
```

    ## [1] 5719877      13

``` r
#confirm column names
colnames(bike_data)
```

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

``` r
#confirm blank data fields
colSums(is.na(bike_data))
```

    ##            ride_id      rideable_type         started_at           ended_at 
    ##                  0                  0                  0                  0 
    ## start_station_name   start_station_id   end_station_name     end_station_id 
    ##             875716             875848             929202             929343 
    ##          start_lat          start_lng            end_lat            end_lng 
    ##                  0                  0               6990               6990 
    ##      member_casual 
    ##                  0

``` r
select(bike_data)
```

    ## # A tibble: 5,719,877 × 0

``` r
nrow(bike_data)
```

    ## [1] 5719877

``` r
ncol(bike_data)
```

    ## [1] 13

``` r
length(bike_data)
```

    ## [1] 13

#### 2.4.2 Identify missing data, limitations and other data problems

I will employ the is.na() function on every variable or column within my
data frame to gain insights into the occurrences of missing data. I will
explore the data to identify variables with missing data. Upon
scrutinizing the dataset, I uncovered several limitations and
challenges. Due to stringent user privacy measures, crucial details such
as user demographics, the distinction between local users and visitors,
and the total number of customers remain undisclosed. Additionally, the
absence of mapping between bike trips and individual customers hinders
comprehensive customer-centric analysis. The classification of casual
rider bike trips in relation to single-ride or full-day passes is
indeterminable, posing further analytical obstacles. Furthermore,
anomalies such as bike trips associated with “testing” and
inconsistencies in the decimal points of latitude and longitude values
introduce complexities. Moreover, specific data columns, including start
and end station names, station IDs, and latitude-longitude coordinates,
exhibit missing values, demanding meticulous attention in subsequent
stages of the analytical process. These identified challenges underscore
the need for a thorough and nuanced approach to address the limitations
and ensure the reliability of the forthcoming data analysis.

These data columns have missing values which we’ll address further along
in the process:

*start_station_name *start_station_id *end_station_name *end_station_id
\*end_lat, end_lng

``` r
attach(bike_data)
bike_data[is.na(start_station_name),]
```

    ## # A tibble: 875,716 × 13
    ##    ride_id          rideable_type started_at          ended_at           
    ##    <chr>            <chr>         <dttm>              <dttm>             
    ##  1 3F624CAD11ADC36B electric_bike 2023-01-24 19:15:35 2023-01-24 19:21:59
    ##  2 7F4991C08F87A20F electric_bike 2023-01-27 12:36:53 2023-01-27 13:02:30
    ##  3 F3AD17CF04B88EE9 electric_bike 2023-01-20 00:37:00 2023-01-20 00:46:09
    ##  4 CA3677FEF8FD11B6 electric_bike 2023-01-27 02:13:40 2023-01-27 02:18:22
    ##  5 6FFD201EBB80C87C electric_bike 2023-01-16 01:43:52 2023-01-16 01:52:02
    ##  6 1CBF19453B2A188A electric_bike 2023-01-03 18:00:00 2023-01-03 18:21:49
    ##  7 5E4D17D116F8992E electric_bike 2023-01-25 20:10:52 2023-01-25 20:13:17
    ##  8 48D7B0517B33E979 electric_bike 2023-01-15 16:49:26 2023-01-15 16:55:08
    ##  9 0038D9BAAB896034 electric_bike 2023-01-30 19:36:11 2023-01-30 19:48:17
    ## 10 03233477CBB0DB29 electric_bike 2023-01-11 12:27:08 2023-01-11 12:36:11
    ## # ℹ 875,706 more rows
    ## # ℹ 9 more variables: start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

``` r
bike_data[is.na(start_station_id),]
```

    ## # A tibble: 875,848 × 13
    ##    ride_id          rideable_type started_at          ended_at           
    ##    <chr>            <chr>         <dttm>              <dttm>             
    ##  1 3F624CAD11ADC36B electric_bike 2023-01-24 19:15:35 2023-01-24 19:21:59
    ##  2 7F4991C08F87A20F electric_bike 2023-01-27 12:36:53 2023-01-27 13:02:30
    ##  3 F3AD17CF04B88EE9 electric_bike 2023-01-20 00:37:00 2023-01-20 00:46:09
    ##  4 CA3677FEF8FD11B6 electric_bike 2023-01-27 02:13:40 2023-01-27 02:18:22
    ##  5 6FFD201EBB80C87C electric_bike 2023-01-16 01:43:52 2023-01-16 01:52:02
    ##  6 1CBF19453B2A188A electric_bike 2023-01-03 18:00:00 2023-01-03 18:21:49
    ##  7 5E4D17D116F8992E electric_bike 2023-01-25 20:10:52 2023-01-25 20:13:17
    ##  8 48D7B0517B33E979 electric_bike 2023-01-15 16:49:26 2023-01-15 16:55:08
    ##  9 0038D9BAAB896034 electric_bike 2023-01-30 19:36:11 2023-01-30 19:48:17
    ## 10 03233477CBB0DB29 electric_bike 2023-01-11 12:27:08 2023-01-11 12:36:11
    ## # ℹ 875,838 more rows
    ## # ℹ 9 more variables: start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

``` r
bike_data[is.na(end_station_name),]
```

    ## # A tibble: 929,202 × 13
    ##    ride_id          rideable_type started_at          ended_at           
    ##    <chr>            <chr>         <dttm>              <dttm>             
    ##  1 98563E8CECC44A5B electric_bike 2023-01-06 13:12:53 2023-01-06 13:18:54
    ##  2 3F625414353F2C07 electric_bike 2023-01-24 07:01:34 2023-01-24 07:06:32
    ##  3 0A1832AB46BA959E electric_bike 2023-01-22 13:09:13 2023-01-22 13:14:17
    ##  4 4B4C428B94A39EEC electric_bike 2023-01-16 10:26:17 2023-01-16 10:28:08
    ##  5 E001B905A293D938 electric_bike 2023-01-31 05:27:52 2023-01-31 05:33:27
    ##  6 E9764CD7AB7E133B electric_bike 2023-01-25 19:08:59 2023-01-25 19:14:26
    ##  7 C641C434FA650F1D electric_bike 2023-01-11 12:24:18 2023-01-11 12:36:15
    ##  8 5611BB00F0F6B8FB electric_bike 2023-01-15 08:57:16 2023-01-15 09:02:32
    ##  9 B4A88F389369C392 electric_bike 2023-01-15 14:43:13 2023-01-15 15:18:56
    ## 10 67C3A736ED3BC098 electric_bike 2023-01-15 20:49:27 2023-01-15 20:52:04
    ## # ℹ 929,192 more rows
    ## # ℹ 9 more variables: start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

``` r
bike_data[is.na(end_station_id),]
```

    ## # A tibble: 929,343 × 13
    ##    ride_id          rideable_type started_at          ended_at           
    ##    <chr>            <chr>         <dttm>              <dttm>             
    ##  1 98563E8CECC44A5B electric_bike 2023-01-06 13:12:53 2023-01-06 13:18:54
    ##  2 3F625414353F2C07 electric_bike 2023-01-24 07:01:34 2023-01-24 07:06:32
    ##  3 0A1832AB46BA959E electric_bike 2023-01-22 13:09:13 2023-01-22 13:14:17
    ##  4 4B4C428B94A39EEC electric_bike 2023-01-16 10:26:17 2023-01-16 10:28:08
    ##  5 E001B905A293D938 electric_bike 2023-01-31 05:27:52 2023-01-31 05:33:27
    ##  6 E9764CD7AB7E133B electric_bike 2023-01-25 19:08:59 2023-01-25 19:14:26
    ##  7 C641C434FA650F1D electric_bike 2023-01-11 12:24:18 2023-01-11 12:36:15
    ##  8 5611BB00F0F6B8FB electric_bike 2023-01-15 08:57:16 2023-01-15 09:02:32
    ##  9 B4A88F389369C392 electric_bike 2023-01-15 14:43:13 2023-01-15 15:18:56
    ## 10 67C3A736ED3BC098 electric_bike 2023-01-15 20:49:27 2023-01-15 20:52:04
    ## # ℹ 929,333 more rows
    ## # ℹ 9 more variables: start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

``` r
bike_data[is.na(start_lat),]
```

    ## # A tibble: 0 × 13
    ## # ℹ 13 variables: ride_id <chr>, rideable_type <chr>, started_at <dttm>,
    ## #   ended_at <dttm>, start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

``` r
bike_data[is.na(start_lng),]
```

    ## # A tibble: 0 × 13
    ## # ℹ 13 variables: ride_id <chr>, rideable_type <chr>, started_at <dttm>,
    ## #   ended_at <dttm>, start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

``` r
bike_data[is.na(end_lat),]
```

    ## # A tibble: 6,990 × 13
    ##    ride_id          rideable_type started_at          ended_at           
    ##    <chr>            <chr>         <dttm>              <dttm>             
    ##  1 1FB8FE3600279846 classic_bike  2023-01-01 04:45:39 2023-01-02 05:45:28
    ##  2 08D63AE5147A8A12 docked_bike   2023-01-09 14:20:41 2023-01-15 04:19:09
    ##  3 BEEB2B851275BBEE classic_bike  2023-01-30 13:24:22 2023-01-31 14:24:09
    ##  4 758F82A4444D0DF3 classic_bike  2023-01-31 09:21:10 2023-02-01 10:21:02
    ##  5 45125F6E88AD0535 docked_bike   2023-01-07 12:52:32 2023-01-08 06:47:21
    ##  6 CFD822F52941BDFF classic_bike  2023-01-19 08:30:10 2023-01-20 09:30:04
    ##  7 F4BEA5EBCB4A75B2 classic_bike  2023-01-08 09:40:16 2023-01-09 10:40:05
    ##  8 B0EBF078DBC98ED0 docked_bike   2023-01-10 14:49:33 2023-01-11 17:54:00
    ##  9 3B05E012D9448676 classic_bike  2023-01-19 08:25:17 2023-01-20 09:25:13
    ## 10 D9873617FD530D40 classic_bike  2023-01-06 16:12:58 2023-01-07 17:12:49
    ## # ℹ 6,980 more rows
    ## # ℹ 9 more variables: start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

``` r
bike_data[is.na(end_lng),]
```

    ## # A tibble: 6,990 × 13
    ##    ride_id          rideable_type started_at          ended_at           
    ##    <chr>            <chr>         <dttm>              <dttm>             
    ##  1 1FB8FE3600279846 classic_bike  2023-01-01 04:45:39 2023-01-02 05:45:28
    ##  2 08D63AE5147A8A12 docked_bike   2023-01-09 14:20:41 2023-01-15 04:19:09
    ##  3 BEEB2B851275BBEE classic_bike  2023-01-30 13:24:22 2023-01-31 14:24:09
    ##  4 758F82A4444D0DF3 classic_bike  2023-01-31 09:21:10 2023-02-01 10:21:02
    ##  5 45125F6E88AD0535 docked_bike   2023-01-07 12:52:32 2023-01-08 06:47:21
    ##  6 CFD822F52941BDFF classic_bike  2023-01-19 08:30:10 2023-01-20 09:30:04
    ##  7 F4BEA5EBCB4A75B2 classic_bike  2023-01-08 09:40:16 2023-01-09 10:40:05
    ##  8 B0EBF078DBC98ED0 docked_bike   2023-01-10 14:49:33 2023-01-11 17:54:00
    ##  9 3B05E012D9448676 classic_bike  2023-01-19 08:25:17 2023-01-20 09:25:13
    ## 10 D9873617FD530D40 classic_bike  2023-01-06 16:12:58 2023-01-07 17:12:49
    ## # ℹ 6,980 more rows
    ## # ℹ 9 more variables: start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

``` r
bike_data[is.na(member_casual),]
```

    ## # A tibble: 0 × 13
    ## # ℹ 13 variables: ride_id <chr>, rideable_type <chr>, started_at <dttm>,
    ## #   ended_at <dttm>, start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

As such, the Prepare phase establishes the foundation for my forthcoming
analysis, ensuring that the acquired data is appropriately sourced,
organized, and evaluated for bias, credibility, and adherence to ethical
considerations. This meticulous preparation is vital for generating
meaningful insights in response to the business questions posed by
Cyclistic’s marketing team.

# 3. Process: Data Transformation and Cleaning

In the Process phase of the Cyclistic bike-share analysis, the emphasis
is on transforming and cleaning the acquired data to make it conducive
for in-depth analysis. This phase involves a series of tasks guided by
key questions related to tool selection, data integrity, cleanliness,
verification, and documentation of the cleaning process.

### 3.1 Tool Selection and Data Integrity

The choice of tools for data processing is a crucial decision that
impacts the efficiency and accuracy of the analysis. Selection is based
on the nature of the data and the analytical requirements. Tools such as
Python, R, or specialized data analysis platforms are chosen for their
versatility and ability to handle the dataset effectively. Before
proceeding, a comprehensive check for data integrity is conducted to
identify any anomalies or discrepancies that may affect the accuracy of
the analysis.

### 3.2 Data Cleaning Steps

The next critical task involves ensuring the cleanliness of the data.
This process begins with a thorough data wrangling. I implemented
several cleaning steps to address data issues and ensure that the
dataset is free from discrepancies that could distort the analysis
results.

### 3.3 Verification and Documentation

Verification of data cleanliness is an ongoing process throughout the
cleaning phase. Rigorous checks are performed to validate that errors
have been appropriately addressed, and the data is ready for analysis.
Documentation of the cleaning process is paramount, ensuring
transparency and reproducibility of the results. This documentation
includes detailed records of steps taken, transformations applied, and
any decisions made during the cleaning process.

### 3.4 Data Transformation

To facilitate effective analysis, the data is transformed into a format
that is conducive to the analytical tasks at hand. This involves
restructuring, aggregating, or deriving new variables to extract
meaningful insights. The transformed dataset is prepared for subsequent
stages of the analysis, aligning with the specific requirements of
addressing the business question regarding the differential usage of
Cyclistic bikes by annual members and casual riders.

#### 3.4.1 Renaming columns

``` r
bike_data <- rename(bike_data, "bike_type" = "rideable_type", "user_type" = "member_casual")
```

#### 3.4.2 Formating data and time

``` r
bike_data$started_at <- ymd_hms(bike_data$started_at)
```

    ## Warning: 23 failed to parse.

``` r
bike_data$ended_at <- ymd_hms(bike_data$ended_at)
```

    ## Warning: 26 failed to parse.

#### 3.4.3 Adding new columns

``` r
#Adding a new column called, “ride_length_min”.
bike_data$ride_length_min <- round(as.numeric(difftime(bike_data$ended_at, bike_data$started_at, units = "mins")), 2)
#Adding columns for: date, month, day, year, day_of_week, and hour
bike_data$date <- as.Date(bike_data$started_at)   
bike_data$month <- format(as.Date(bike_data$date), "%B")
bike_data$day <- format(as.Date(bike_data$date), "%d")
bike_data$year <- format(as.Date(bike_data$date), "%Y")
bike_data$day_of_week <- format(as.Date(bike_data$date), "%A")
bike_data$hour <- lubridate::hour(bike_data$started_at)

#Adding a column for season

bike_data <- bike_data %>% mutate(season = recode(month,
                        December = "Winter",
                        January = "Winter",
                        February = "Winter",
                        March = "Spring",
                        April = "Spring",
                        May = "Spring",
                        June = "Summer",
                        July = "Summer",
                        August = "Summer",
                        September = "Fall",
                        October = "Fall",
                        November = "Fall"))

#Adding a column for “time_of_day” by using the case_when() function

bike_data <- bike_data %>% mutate(time_of_day = case_when(
                        hour >= 6 & hour < 9 ~ "Early Morning",
                        hour >= 9 & hour < 12 ~ "Mid Morning",
                        hour >= 12 & hour < 18  ~ "Afternoon",
                        hour >= 18 & hour <= 23  ~ "Evening",
                        hour >= 0 & hour < 3  ~ "Early Night",
                        hour >= 3 & hour < 6  ~ "Late Night"))
```

#### 3.4.4 Address missing values

``` r
#Creating four new columns to show start_lat, start_lng, end_lat & end_lng all rounded to 2 decimal places.

bike_data <- bike_data %>% 
  mutate(start_lat_round = round(start_lat, digits = 2),
        start_lng_round = round(start_lng, digits = 2),
        end_lat_round = round(end_lat, digits = 2),
        end_lng_round = round(end_lng, digits = 2))
#Impute missing start station names
bike_data <- bike_data %>% 
  group_by(start_lat_round, start_lng_round) %>% 
  tidyr::fill(start_station_name, .direction = "downup") %>% 
  ungroup()

#Impute missing end station names
bike_data <- bike_data %>% 
  group_by(end_lat_round, end_lng_round) %>% 
  tidyr::fill(end_station_name, .direction = "downup") %>% 
  ungroup()
 
#Impute missing start_station_id
bike_data <- bike_data %>% 
  group_by(start_station_name) %>% 
  tidyr::fill(start_station_id, .direction = "downup") %>% 
  ungroup()
 
#Impute missing end_station_id
bike_data <- bike_data %>% 
  group_by(end_station_name) %>% 
  tidyr::fill(end_station_id, .direction = "downup") %>% 
  ungroup()
 
#Now that we’ve imputed alot of the missing data, let’s check missing values by column, again, and see what’s still missing.
colSums(is.na(bike_data))
```

    ##            ride_id          bike_type         started_at           ended_at 
    ##                  0                  0                 23                 26 
    ## start_station_name   start_station_id   end_station_name     end_station_id 
    ##              10646              10646              33105              33105 
    ##          start_lat          start_lng            end_lat            end_lng 
    ##                  0                  0               6990               6990 
    ##          user_type    ride_length_min               date              month 
    ##                  0                 49                 23                 23 
    ##                day               year        day_of_week               hour 
    ##                 23                 23                 23                 23 
    ##             season        time_of_day    start_lat_round    start_lng_round 
    ##                 23                 23                  0                  0 
    ##      end_lat_round      end_lng_round 
    ##               6990               6990

``` r
#Where end_lat & end_lng are missing, we don’t have an end_station_name or end_station_id so the missing data cannot be imputed using those fields. These are all 1 minute rides with the exception of one 2 minute ride. Since these are 1 minute rides with missing end-lat & end_lng I’m assuming they are flukes and I am going to remove them. Percentage-wise 5,835 of missing data is considered immaterial and will not impact the integrity of my dataset.

bike_data %>% filter(is.na(end_lat)) %>% 
  count(end_station_name, end_station_id, end_lat, end_lng, bike_type)
```

    ## # A tibble: 9 × 6
    ##   end_station_name           end_station_id end_lat end_lng bike_type        n
    ##   <chr>                      <chr>            <dbl>   <dbl> <chr>        <int>
    ## 1 Drexel Ave & 60th St       22005               NA      NA classic_bike  2673
    ## 2 Drexel Ave & 60th St       22005               NA      NA docked_bike    373
    ## 3 Elizabeth St & Randolph St 23001               NA      NA classic_bike    46
    ## 4 Elizabeth St & Randolph St 23001               NA      NA docked_bike     56
    ## 5 Halsted St & Fulton St     23003               NA      NA classic_bike  1442
    ## 6 Halsted St & Fulton St     23003               NA      NA docked_bike   1178
    ## 7 Lincoln Ave & Byron St     23002               NA      NA docked_bike     14
    ## 8 Stony Island Ave & 63rd St 653B                NA      NA classic_bike   666
    ## 9 Stony Island Ave & 63rd St 653B                NA      NA docked_bike    542

``` r
#Where start_station_name is missing, we don’t have a start_station_id to impute the missing data. I should be able to impute from data with matching end_lat & end_lng coordinates. That would be trickier because, we may have multiple station names where the lat & lng coordinates only go out 2 decimal places. For timesake and because the amount of data is immaterial, I’m going to leave as is and move on. I could remove this data as it is immaterial. Adding the View() function will provide a table that you can then also filter on.

bike_data %>% filter(is.na(start_station_name)) %>% 
  count(start_station_name, start_station_id, start_lat, start_lng, bike_type)
```

    ## # A tibble: 159 × 6
    ##    start_station_name start_station_id start_lat start_lng bike_type         n
    ##    <chr>              <chr>                <dbl>     <dbl> <chr>         <int>
    ##  1 <NA>               <NA>                  41.6     -87.5 electric_bike     1
    ##  2 <NA>               <NA>                  41.6     -87.5 electric_bike     2
    ##  3 <NA>               <NA>                  41.6     -87.6 electric_bike    17
    ##  4 <NA>               <NA>                  41.6     -87.6 electric_bike     3
    ##  5 <NA>               <NA>                  41.6     -87.5 electric_bike    17
    ##  6 <NA>               <NA>                  41.7     -87.6 electric_bike    19
    ##  7 <NA>               <NA>                  41.7     -87.6 electric_bike     5
    ##  8 <NA>               <NA>                  41.7     -87.6 electric_bike     1
    ##  9 <NA>               <NA>                  41.7     -87.6 electric_bike     5
    ## 10 <NA>               <NA>                  41.7     -87.7 electric_bike     7
    ## # ℹ 149 more rows

``` r
#Where end_station_name is missing, we don’t have a end_station_id to impute the missing data, but I should be able to impute from data with matching end_lat & end_lng coordinates. That would be trickier because, we may have multiple station names where the lat & lng coordinates only go out 2 decimal places. For timesake and because the amount of data is immaterial, I’m going to leave as is and move on. I could remove this data as it is immaterial. Adding the View() function will provide a table that you can then also filter on.

bike_data %>% filter(is.na(end_station_name)) %>% 
  count(end_station_name, end_station_id, end_lat, end_lng, bike_type)
```

    ## # A tibble: 332 × 6
    ##    end_station_name end_station_id end_lat end_lng bike_type         n
    ##    <chr>            <chr>            <dbl>   <dbl> <chr>         <int>
    ##  1 <NA>             <NA>              41.6   -87.5 electric_bike     1
    ##  2 <NA>             <NA>              41.6   -87.6 electric_bike     1
    ##  3 <NA>             <NA>              41.6   -87.6 electric_bike     1
    ##  4 <NA>             <NA>              41.6   -87.6 electric_bike     1
    ##  5 <NA>             <NA>              41.6   -87.5 electric_bike     1
    ##  6 <NA>             <NA>              41.6   -87.6 electric_bike     1
    ##  7 <NA>             <NA>              41.6   -87.5 electric_bike     1
    ##  8 <NA>             <NA>              41.6   -87.5 electric_bike     1
    ##  9 <NA>             <NA>              41.6   -87.7 electric_bike     2
    ## 10 <NA>             <NA>              41.6   -87.7 electric_bike     1
    ## # ℹ 322 more rows

#### 3.4.5 Dropping unwanted data

After a thorough review of the data, I’ll remove the following:

*Rides less than 60 seconds in length as they are potentially false
starts or users trying to re-dock a bike to ensure it was secure per
Divvy website: Divvy System Data *Rides with a negative ride_length are
considered invalid since the trip start time cannot be greater than the
trip end time *Rides with a ride_length greater than 24 hrs are
considered invalid outliers for purposes of this project *Rides where
end_lat & end_lng are both missing, in these cases we don’t have an
end_station_name or end_station_id so the missing data cannot be
imputed. Percentage-wise 5,835 of missing data is considered immaterial.
These are all 1 minute rides with the exception of one 2 minute ride.
Since they have no end-lat & end_lng I’m assuming they are flukes.
\*Rides where start_station_id or end_station_id are related to
“testing” as identified earlier

I’ll use the select() function to create a new data frame with only
selected columns.

``` r
bike_data2 <- select(bike_data, c(1,2,5, 6:16, 13:16, 18:22))
```

Remove rides less than 60 seconds (or 1 minute) and greater than 24 hrs
(or 1440 minutes) in length. This will remove all rides with a negative
ride_length

``` r
bike_data2 <- bike_data2 %>% 
  filter(ride_length_min >= 1 & ride_length_min <= 1440)
```

Remove rides where end_lat & end_lng are both missing

``` r
bike_data2 <- bike_data2 %>%
  filter(!is.na(end_lat) & !is.na(end_lng))
```

Remove rides related to test/repair stations

``` r
bike_data2 <- bike_data2 %>% 
  filter(!start_station_id %in% c("DIVVY 001", "DIVVY 001 - Warehouse test station", "Hubbard Bike-checking (LBS-WH-TEST)", "Pawel Bialowas - Test- PBSC charging station", "DIVVY CASSETTE REPAIR MOBILE STATION", "2059 Hastings Warehouse Station", "Hastings WH 2", "Throop/Hastings Mobile Station"))
bike_data2 <- bike_data2 %>%
  filter(!end_station_id %in% c("DIVVY 001", "DIVVY 001 - Warehouse test station", "Hubbard Bike-checking (LBS-WH-TEST)", "Pawel Bialowas - Test- PBSC charging station", "DIVVY CASSETTE REPAIR MOBILE STATION", "2059 Hastings Warehouse Station", "Hastings WH 2", "Throop/Hastings Mobile Station"))
```

Finally, I’ll run through a quick check off to verify the data is now
clean, accurate, consistent and complete.

``` r
glimpse(bike_data2)
```

    ## Rows: 5,562,449
    ## Columns: 19
    ## $ ride_id            <chr> "F96D5A74A3E41399", "13CB7EB698CEDB88", "BD88A2E670…
    ## $ bike_type          <chr> "electric_bike", "classic_bike", "electric_bike", "…
    ## $ start_station_name <chr> "Lincoln Ave & Fullerton Ave", "Kimbark Ave & 53rd …
    ## $ start_station_id   <chr> "TA1309000058", "TA1309000037", "RP-005", "TA130900…
    ## $ end_station_name   <chr> "Hampden Ct & Diversey Ave", "Greenwood Ave & 47th …
    ## $ end_station_id     <chr> "202480.0", "TA1308000002", "599", "TA1308000002", …
    ## $ start_lat          <dbl> 41.92407, 41.79957, 42.00857, 41.79957, 41.79957, 4…
    ## $ start_lng          <dbl> -87.64628, -87.59475, -87.69048, -87.59475, -87.594…
    ## $ end_lat            <dbl> 41.93000, 41.80983, 42.03974, 41.80983, 41.80983, 4…
    ## $ end_lng            <dbl> -87.64000, -87.59938, -87.69941, -87.59938, -87.599…
    ## $ user_type          <chr> "member", "member", "casual", "member", "member", "…
    ## $ ride_length_min    <dbl> 10.85, 8.48, 13.23, 8.77, 15.32, 3.22, 14.00, 9.35,…
    ## $ date               <date> 2023-01-21, 2023-01-10, 2023-01-02, 2023-01-22, 20…
    ## $ month              <chr> "January", "January", "January", "January", "Januar…
    ## $ year               <chr> "2023", "2023", "2023", "2023", "2023", "2023", "20…
    ## $ day_of_week        <chr> "Saturday", "Tuesday", "Monday", "Sunday", "Thursda…
    ## $ hour               <int> 20, 15, 7, 10, 13, 7, 21, 10, 20, 16, 17, 17, 19, 2…
    ## $ season             <chr> "Winter", "Winter", "Winter", "Winter", "Winter", "…
    ## $ time_of_day        <chr> "Evening", "Afternoon", "Early Morning", "Mid Morni…

``` r
head(bike_data2)
```

    ## # A tibble: 6 × 19
    ##   ride_id         bike_type start_station_name start_station_id end_station_name
    ##   <chr>           <chr>     <chr>              <chr>            <chr>           
    ## 1 F96D5A74A3E413… electric… Lincoln Ave & Ful… TA1309000058     Hampden Ct & Di…
    ## 2 13CB7EB698CEDB… classic_… Kimbark Ave & 53r… TA1309000037     Greenwood Ave &…
    ## 3 BD88A2E670661C… electric… Western Ave & Lun… RP-005           Valli Produce -…
    ## 4 C90792D034FED9… classic_… Kimbark Ave & 53r… TA1309000037     Greenwood Ave &…
    ## 5 3397017529188E… classic_… Kimbark Ave & 53r… TA1309000037     Greenwood Ave &…
    ## 6 58E68156DAE3E3… electric… Lakeview Ave & Fu… TA1309000019     Hampden Ct & Di…
    ## # ℹ 14 more variables: end_station_id <chr>, start_lat <dbl>, start_lng <dbl>,
    ## #   end_lat <dbl>, end_lng <dbl>, user_type <chr>, ride_length_min <dbl>,
    ## #   date <date>, month <chr>, year <chr>, day_of_week <chr>, hour <int>,
    ## #   season <chr>, time_of_day <chr>

The Process phase ensures that the data is refined, cleaned, and
transformed, laying the groundwork for a robust analysis. The careful
selection of tools, thorough data integrity checks, meticulous cleaning
steps, and comprehensive documentation contribute to the reliability and
accuracy of the subsequent analytical efforts. This phase is integral to
producing actionable insights that will inform the development of
targeted marketing strategies for Cyclistic.

# 4. Analyze: Extracting Insights from the Data

With the data appropriately stored and prepared, the Analyze phase
focuses on extracting meaningful insights that address the business
questions outlined in the Cyclistic bike-share case study. This phase is
crucial for identifying trends, relationships, and surprises within the
dataset. At this step, I’ll work with the data to analyze and look at it
in different ways. I’ll use functions to help me explore relationships
and trends while being mindful to not stray from my objective which is
to analyze how annual member riders and casual riders use Cyclistic
bikes differently.

### 4.1 Data Organization and Formatting

The first task involves organizing the data for effective analysis. This
includes structuring the dataset in a way that facilitates exploration
of key variables and relationships. Ensuring proper formatting is
essential, enabling seamless application of analytical techniques. Any
surprises or anomalies discovered during this process are documented for
a comprehensive understanding of the dataset.

### 4.2 Aggregation and Calculation

To derive actionable insights, the data is aggregated to reveal patterns
and trends. Calculations are performed to quantify key metrics relevant
to the business question, such as average ride duration, frequency of
rides, and peak usage times. These aggregated results provide a clearer
picture of how annual members and casual riders differ in their
utilization of Cyclistic bikes.

### 4.3 Identifying Trends and Relationships

The heart of the Analyze phase lies in uncovering trends and
relationships within the data. This involves employing statistical
methods, visualizations, and exploratory data analysis techniques to
discern patterns. The objective is to understand the nuances of how
annual members and casual riders distinctly engage with Cyclistic bikes.
Discovering trends allows for informed decision-making, guiding the
subsequent stages of the analysis.

``` r
#Save the data
write.csv(bike_data2, file = 'Cleaned_bike_data.csv')
```

#### Summarizing Data based on user types

``` r
summary(bike_data2)
```

    ##    ride_id           bike_type         start_station_name start_station_id  
    ##  Length:5562449     Length:5562449     Length:5562449     Length:5562449    
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##  end_station_name   end_station_id       start_lat       start_lng     
    ##  Length:5562449     Length:5562449     Min.   :41.63   Min.   :-87.94  
    ##  Class :character   Class :character   1st Qu.:41.88   1st Qu.:-87.66  
    ##  Mode  :character   Mode  :character   Median :41.90   Median :-87.64  
    ##                                        Mean   :41.90   Mean   :-87.65  
    ##                                        3rd Qu.:41.93   3rd Qu.:-87.63  
    ##                                        Max.   :42.07   Max.   :-87.46  
    ##     end_lat         end_lng        user_type         ride_length_min  
    ##  Min.   : 0.00   Min.   :-88.16   Length:5562449     Min.   :   1.00  
    ##  1st Qu.:41.88   1st Qu.:-87.66   Class :character   1st Qu.:   5.70  
    ##  Median :41.90   Median :-87.64   Mode  :character   Median :   9.78  
    ##  Mean   :41.90   Mean   :-87.65                      Mean   :  15.49  
    ##  3rd Qu.:41.93   3rd Qu.:-87.63                      3rd Qu.:  17.18  
    ##  Max.   :42.18   Max.   :  0.00                      Max.   :1439.87  
    ##       date               month               year           day_of_week       
    ##  Min.   :2023-01-01   Length:5562449     Length:5562449     Length:5562449    
    ##  1st Qu.:2023-05-21   Class :character   Class :character   Class :character  
    ##  Median :2023-07-21   Mode  :character   Mode  :character   Mode  :character  
    ##  Mean   :2023-07-16                                                           
    ##  3rd Qu.:2023-09-17                                                           
    ##  Max.   :2023-12-31                                                           
    ##       hour          season          time_of_day       
    ##  Min.   : 0.00   Length:5562449     Length:5562449    
    ##  1st Qu.:11.00   Class :character   Class :character  
    ##  Median :15.00   Mode  :character   Mode  :character  
    ##  Mean   :14.09                                        
    ##  3rd Qu.:18.00                                        
    ##  Max.   :23.00

``` r
#Let’s get a count and percentage breakdown of our two user types: the casual rider vs the member rider
bike_data2 %>% 
  group_by(user_type) %>% 
  summarise(count = n(), Percentage = n()/nrow(bike_data2)*100)
```

    ## # A tibble: 2 × 3
    ##   user_type   count Percentage
    ##   <chr>       <int>      <dbl>
    ## 1 casual    1999509       35.9
    ## 2 member    3562940       64.1

``` r
#Total rides by user type and bike type
bike_data2 %>% 
  group_by(user_type, bike_type) %>% 
  summarise(count = n(), Percentage = n()/nrow(bike_data2)*100)
```

    ## `summarise()` has grouped output by 'user_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 5 × 4
    ## # Groups:   user_type [2]
    ##   user_type bike_type       count Percentage
    ##   <chr>     <chr>           <int>      <dbl>
    ## 1 casual    classic_bike   860697      15.5 
    ## 2 casual    docked_bike     75372       1.36
    ## 3 casual    electric_bike 1063440      19.1 
    ## 4 member    classic_bike  1788959      32.2 
    ## 5 member    electric_bike 1773981      31.9

#### Analyzing Ride Length

Let’s look at the length of each trip (in minutes)

``` r
#summary
summary(bike_data2$ride_length_min)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    1.00    5.70    9.78   15.49   17.18 1439.87

``` r
#Average ride length of each trip (in minutes) by user type
aggregate(bike_data2$ride_length_min ~ bike_data2$user_type, FUN = mean)
```

    ##   bike_data2$user_type bike_data2$ride_length_min
    ## 1               casual                   21.05655
    ## 2               member                   12.36407

``` r
#Summarize the rides based on number of minutes
bike_data2 %>% 
   group_by(user_type) %>% 
  summarize("<12 min" = sum(ride_length_min <11.99),
            "12-20 min" = sum(ride_length_min >=12 & ride_length_min <=20.99),
            "21-30 min" = sum(ride_length_min >=21 & ride_length_min <=30.99),
            "31-60 min" = sum(ride_length_min >=31 & ride_length_min <=60),
            "61-120 min" = sum(ride_length_min >=60.01 & ride_length_min <=120.99),
            "121-240 min" = sum(ride_length_min >=121 & ride_length_min <=240.99),
            "241+min" = sum(ride_length_min >=241))  
```

    ## # A tibble: 2 × 8
    ##   user_type `<12 min` `12-20 min` `21-30 min` `31-60 min` `61-120 min`
    ##   <chr>         <int>       <int>       <int>       <int>        <int>
    ## 1 casual       987061      466167      230592      206619        83732
    ## 2 member      2340286      750620      285832      161299        17181
    ## # ℹ 2 more variables: `121-240 min` <int>, `241+min` <int>

``` r
#Adding a column to create ride length categories to get a better visual in R
bike_data2 <- bike_data2 %>% mutate(ride_length_cat = case_when(
                        ride_length_min <11.99 ~ "< 12 min",
                        ride_length_min >=12 & ride_length_min <=20.99 ~ "12-20 min",
                        ride_length_min >=21 & ride_length_min <=30.99  ~ "21-30 min",
                        ride_length_min >=31 & ride_length_min <=60.99  ~ "31-60 min",
                        ride_length_min >=60 & ride_length_min <=120.99  ~ "61-120 min",
                        ride_length_min >=121 & ride_length_min <=240.99  ~ "121-240 min",
                        ride_length_min >=241  ~ "241+ min"))
#Total rides by user type and ride length category
bike_data2 %>% 
  group_by(user_type, ride_length_cat) %>% 
  summarise(count = n(), Percentage = n()/nrow(bike_data2)*100) 
```

    ## `summarise()` has grouped output by 'user_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 14 × 4
    ## # Groups:   user_type [2]
    ##    user_type ride_length_cat   count Percentage
    ##    <chr>     <chr>             <int>      <dbl>
    ##  1 casual    12-20 min        466167     8.38  
    ##  2 casual    121-240 min       19519     0.351 
    ##  3 casual    21-30 min        230592     4.15  
    ##  4 casual    241+ min           5819     0.105 
    ##  5 casual    31-60 min        209886     3.77  
    ##  6 casual    61-120 min        80465     1.45  
    ##  7 casual    < 12 min         987061    17.7   
    ##  8 member    12-20 min        750620    13.5   
    ##  9 member    121-240 min        4561     0.0820
    ## 10 member    21-30 min        285832     5.14  
    ## 11 member    241+ min           3161     0.0568
    ## 12 member    31-60 min        162163     2.92  
    ## 13 member    61-120 min        16317     0.293 
    ## 14 member    < 12 min        2340286    42.1

``` r
#Average ride length of each trip (in minutes) by user type and time of day
axis_labels <- c("Early Morning \n6am-9am", "Mid Morning \n9am-12pm", "Afternoon \n12pm-6pm", "Evening \n6pm-11pm", "Early Night \n11pm-3am", "Wee Night \n3am-6am")

bike_data2 %>% 
  group_by(user_type, time_of_day) %>% 
  summarise(count = n(), average_ride_length=mean(ride_length_min)) 
```

    ## `summarise()` has grouped output by 'user_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 12 × 4
    ## # Groups:   user_type [2]
    ##    user_type time_of_day     count average_ride_length
    ##    <chr>     <chr>           <int>               <dbl>
    ##  1 casual    Afternoon      923888                22.7
    ##  2 casual    Early Morning  150023                15.1
    ##  3 casual    Early Night     72592                18.6
    ##  4 casual    Evening        568524                19.3
    ##  5 casual    Late Night      24557                15.7
    ##  6 casual    Mid Morning    259925                23.8
    ##  7 member    Afternoon     1524379                12.8
    ##  8 member    Early Morning  530912                11.1
    ##  9 member    Early Night     66851                11.9
    ## 10 member    Evening        914106                12.6
    ## 11 member    Late Night      49469                10.9
    ## 12 member    Mid Morning    477223                12.2

``` r
#Average ride length of each trip (in minutes) by user type and day of the week note
bike_data2 %>% 
  group_by(user_type, day_of_week) %>% 
  summarise(count = n(), average_ride_length=mean(ride_length_min)) 
```

    ## `summarise()` has grouped output by 'user_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 14 × 4
    ## # Groups:   user_type [2]
    ##    user_type day_of_week  count average_ride_length
    ##    <chr>     <chr>        <int>               <dbl>
    ##  1 casual    Friday      302959                20.4
    ##  2 casual    Monday      228167                20.7
    ##  3 casual    Saturday    398524                23.8
    ##  4 casual    Sunday      325580                24.5
    ##  5 casual    Thursday    262899                18.4
    ##  6 casual    Tuesday     239257                18.8
    ##  7 casual    Wednesday   242123                18.0
    ##  8 member    Friday      517038                12.3
    ##  9 member    Monday      481741                11.7
    ## 10 member    Saturday    459649                13.8
    ## 11 member    Sunday      397849                13.8
    ## 12 member    Thursday    573573                11.9
    ## 13 member    Tuesday     561732                11.9
    ## 14 member    Wednesday   571358                11.8

#### Other Analysis

``` r
#Total rides by user type and by hour of the day
bike_data2 %>% 
  group_by(user_type, hour) %>% 
  summarise(count = n()) %>%  
  arrange(user_type, hour) 
```

    ## `summarise()` has grouped output by 'user_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 48 × 3
    ## # Groups:   user_type [2]
    ##    user_type  hour count
    ##    <chr>     <int> <int>
    ##  1 casual        0 35590
    ##  2 casual        1 23074
    ##  3 casual        2 13928
    ##  4 casual        3  7658
    ##  5 casual        4  5764
    ##  6 casual        5 11135
    ##  7 casual        6 29377
    ##  8 casual        7 51688
    ##  9 casual        8 68958
    ## 10 casual        9 68142
    ## # ℹ 38 more rows

``` r
#Total rides by user type and by time of day
bike_data2 %>% 
  group_by(user_type, time_of_day) %>% 
  summarise(count = n()) 
```

    ## `summarise()` has grouped output by 'user_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 12 × 3
    ## # Groups:   user_type [2]
    ##    user_type time_of_day     count
    ##    <chr>     <chr>           <int>
    ##  1 casual    Afternoon      923888
    ##  2 casual    Early Morning  150023
    ##  3 casual    Early Night     72592
    ##  4 casual    Evening        568524
    ##  5 casual    Late Night      24557
    ##  6 casual    Mid Morning    259925
    ##  7 member    Afternoon     1524379
    ##  8 member    Early Morning  530912
    ##  9 member    Early Night     66851
    ## 10 member    Evening        914106
    ## 11 member    Late Night      49469
    ## 12 member    Mid Morning    477223

``` r
#Total rides by user type and day of the week
bike_data2 %>% 
  group_by(user_type, day_of_week) %>% 
  summarise(count = n()) 
```

    ## `summarise()` has grouped output by 'user_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 14 × 3
    ## # Groups:   user_type [2]
    ##    user_type day_of_week  count
    ##    <chr>     <chr>        <int>
    ##  1 casual    Friday      302959
    ##  2 casual    Monday      228167
    ##  3 casual    Saturday    398524
    ##  4 casual    Sunday      325580
    ##  5 casual    Thursday    262899
    ##  6 casual    Tuesday     239257
    ##  7 casual    Wednesday   242123
    ##  8 member    Friday      517038
    ##  9 member    Monday      481741
    ## 10 member    Saturday    459649
    ## 11 member    Sunday      397849
    ## 12 member    Thursday    573573
    ## 13 member    Tuesday     561732
    ## 14 member    Wednesday   571358

``` r
#Total rides by user type and day of the week
bike_data2 %>% 
  group_by(user_type, day_of_week) %>% 
  summarise(count = n()) 
```

    ## `summarise()` has grouped output by 'user_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 14 × 3
    ## # Groups:   user_type [2]
    ##    user_type day_of_week  count
    ##    <chr>     <chr>        <int>
    ##  1 casual    Friday      302959
    ##  2 casual    Monday      228167
    ##  3 casual    Saturday    398524
    ##  4 casual    Sunday      325580
    ##  5 casual    Thursday    262899
    ##  6 casual    Tuesday     239257
    ##  7 casual    Wednesday   242123
    ##  8 member    Friday      517038
    ##  9 member    Monday      481741
    ## 10 member    Saturday    459649
    ## 11 member    Sunday      397849
    ## 12 member    Thursday    573573
    ## 13 member    Tuesday     561732
    ## 14 member    Wednesday   571358

``` r
#Total rides by user type and season
bike_data2 %>% 
  group_by(user_type, season) %>% 
  summarise(count = n()) 
```

    ## `summarise()` has grouped output by 'user_type'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 8 × 3
    ## # Groups:   user_type [2]
    ##   user_type season   count
    ##   <chr>     <chr>    <int>
    ## 1 casual    Fall    523458
    ## 2 casual    Spring  428932
    ## 3 casual    Summer  916285
    ## 4 casual    Winter  130834
    ## 5 member    Fall   1007070
    ## 6 member    Spring  817661
    ## 7 member    Summer 1281817
    ## 8 member    Winter  456392

``` r
#Top five starting stations for casual riders. 
bike_data2 %>% 
  filter(!(is.na(start_station_name))) %>% 
  filter(user_type == "casual") %>% 
  group_by(start_station_name) %>% 
  summarize(count=n()) %>% 
  arrange(-count) %>% 
  top_n(5) %>% 
  mutate(start_station_name= fct_reorder(start_station_name, count)) 
```

    ## Selecting by count

    ## # A tibble: 5 × 2
    ##   start_station_name                 count
    ##   <fct>                              <int>
    ## 1 Streeter Dr & Grand Ave            47942
    ## 2 DuSable Lake Shore Dr & Monroe St  30622
    ## 3 DuSable Lake Shore Dr & North Blvd 23721
    ## 4 Michigan Ave & Oak St              22936
    ## 5 Millennium Park                    20475

``` r
#Top five ending stations for casual riders
bike_data2 %>% 
  filter(!(is.na(end_station_name))) %>% 
  filter(user_type == "casual") %>% 
  group_by(end_station_name) %>% 
  summarize(count=n()) %>% 
  arrange(-count) %>% 
  top_n(5)
```

    ## Selecting by count

    ## # A tibble: 5 × 2
    ##   end_station_name                   count
    ##   <chr>                              <int>
    ## 1 Streeter Dr & Grand Ave            53413
    ## 2 DuSable Lake Shore Dr & Monroe St  27093
    ## 3 Millennium Park                    25269
    ## 4 DuSable Lake Shore Dr & North Blvd 23531
    ## 5 Michigan Ave & Oak St              23525

``` r
#Top five starting stations for member riders
bike_data2 %>% 
  filter(!(is.na(start_station_name))) %>% 
  filter(user_type == "member") %>% 
  group_by(start_station_name) %>% 
  summarize(count=n()) %>% 
  arrange(-count) %>% 
  top_n(5)
```

    ## Selecting by count

    ## # A tibble: 5 × 2
    ##   start_station_name           count
    ##   <chr>                        <int>
    ## 1 Clinton St & Washington Blvd 27678
    ## 2 Kingsbury St & Kinzie St     26454
    ## 3 Clark St & Elm St            25850
    ## 4 Wells St & Concord Ln        22885
    ## 5 Wells St & Elm St            22210

I then Visualized some of the Analysis in Tableau.

### 4.4 Insights for Business Questions

The analysis of the bike sharing data provides valuable insights into
user behavior, ride characteristics, and station popularity. The
findings are presented in APA format below:

#### 4.4.1 User Types and Ride Distribution

The dataset comprises a total of 5,562,449 rides, with 35.9% identified
as casual riders and 64.1% as member riders. Further exploration reveals
that among casual riders, electric bikes (19.1%) are more popular than
classic bikes (15.5%) and docked bikes (1.36%). On the other hand,
member riders exhibit a preference for both classic bikes (32.2%) and
electric bikes (31.9%).

#### 4.4.2 Ride Length Analysis

The average ride length for casual riders is notably higher (21.06
minutes) compared to member riders (12.36 minutes). Categorizing rides
by duration reveals that a significant portion of casual rides falls
within the 12-20 minute range (8.38%), while member rides are more
evenly distributed across various durations.

##### 4.4.3 Temporal Trends

Analyzing ride lengths across different times of the day indicates that
casual riders tend to have longer rides in the afternoon (average of
22.7 minutes), whereas member riders exhibit a more consistent ride
length throughout the day. Additionally, both user types show higher
average ride lengths on weekends compared to weekdays.

#### 4.4.4 Day of the Week Analysis

Casual riders demonstrate a preference for weekend rides, with the
highest average ride lengths recorded on Saturday (23.8 minutes) and
Sunday (24.5 minutes). In contrast, member riders display more
consistent ride lengths across all days of the week.

#### 4.4.5 Station Popularity

The top five starting stations for casual riders are Streeter Dr & Grand
Ave, DuSable Lake Shore Dr & Monroe St, DuSable Lake Shore Dr & North
Blvd, Michigan Ave & Oak St, and Millennium Park. Member riders, on the
other hand, frequently initiate rides from stations such as Clinton St &
Washington Blvd, Kingsbury St & Kinzie St, Clark St & Elm St, Wells St &
Concord Ln, and Wells St & Elm St.

These findings offer a comprehensive understanding of user behavior and
preferences, enabling stakeholders to optimize bike-sharing services for
both casual and member riders.

# 5. Share: Knitting to HTML and Visualizing on Tableau for Effective Communication

For the Share phase, the visualization of data in Tableau emerged as a
powerful and effective strategy. The key task involved determining the
best way to convey the insights gained from the Cyclistic bike-share
analysis to the executive team. Leveraging Tableau’s capabilities,
sophisticated and polished visualizations were crafted, adhering to
Moreno’s emphasis on clarity and effectiveness.

The visualizations, hosted on Tableau Public, serve as a centralized
platform for sharing findings. This approach ensures accessibility to
the executive team, enabling them to interact with and explore the
visualizations at their convenience. The data tells a compelling story
about how annual members and casual riders differ in their utilization
of Cyclistic bikes, directly addressing the original question posed in
the Ask phase.

By choosing Tableau Public, the presentation of findings becomes not
only visually engaging but also easily accessible to a diverse audience.
The interactive nature of Tableau allows for a dynamic exploration of
trends, relationships, and key metrics, enhancing the understanding of
the nuanced behaviors observed in the data. This aligns seamlessly with
the goal of effectively communicating the insights to the executive team
and facilitating informed decision-making based on the comprehensive
analysis of Cyclistic’s bike-share data.

### Top Three Recommendations

1.  **Targeted Marketing Campaigns:** Craft marketing initiatives
    tailored to the specific preferences and behaviors of casual riders.
    Emphasize the benefits of annual memberships, aligning with their
    distinct usage patterns.

2.  **Promotional Incentives:** Introduce promotional incentives, such
    as discounted annual memberships for first-time users or loyalty
    programs. These incentives aim to encourage casual riders to
    transition to annual memberships.

3.  **Digital Media Engagement:** Leverage digital media platforms to
    actively engage with casual riders. Utilize targeted advertisements
    and social media channels to communicate the advantages of annual
    memberships and encourage user conversion.

### Portfolio Development

To showcase the analysis, recommendations, and insights, I created an
online portfolio on LinkedIn. This portfolio becomes a visual
representation of the case study, providing a comprehensive overview of
the analytical process and the value derived from it. The portfolio
serves as a professional showcase of skills, methodologies, and
outcomes.

### Practice Presentation

Preparation for presenting the case study is essential. I will
practicing the presentation with either with a friend or family member
to ensure a confident and articulate delivery. The presentation aims to
effectively convey the significance of the analysis, the derived
insights, and the actionable recommendations to stakeholders.

# Conclusion

In conclusion, the Cyclistic bike-share case study unveils valuable
insights into the diverse usage patterns of annual members and casual
riders. Through meticulous analysis and sophisticated visualizations,
the study provides actionable recommendations to enhance marketing
strategies. The journey from data exploration to polished HTML
presentation exemplifies the power of informed decision-making. This
case study showcases the intersection of analytical prowess, effective
communication, and strategic thinking, reinforcing the significance of
data-driven approaches in guiding business success for Cyclistic.
