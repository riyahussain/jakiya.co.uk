---
title: "Google Capstone Project: Cyclistic Bike Share 🚴"
draft: false
---

## Introduction

As part of the Google Data Analytics Professional Certificate, this case study was chosen for my Capstone Project. It will consist of the following processes as taught in the programme; ******************************Ask, Prepare, Process, Analyse, Share and Act.****************************** Due to the large nature of the dataset, R was used in order to clean, manipulate, analyse and visualise. Tableau was used for creating the visualisations, you can access my dashboard [here](https://public.tableau.com/app/profile/jakiya/viz/CyclistBikeShare_16749151981160/CyclistBikeShare)!

### Scenario

Based in Chicago, Cyclistic is a fictional bike share company. Lily Moreno, the marketing director, believes maximising the number of annual memberships will lead to the company’s future success. Hence, the data analytics team wants to understand how casual riders and annual member riders use Cyclistic bikes differently. From these insights, a new marketing strategy will be designed to convert casual riders into member riders. 

### About the company

Launched in 2016, Cyclistic features more than 5,800 geo-tracked bicycles and almost 700 docking stations. Cyclistic sets itself apart by offering reclining bikes, hand tricycles and cargo bikes, ensuring bike share is more inclusive to people with disabilities. These bikes can be unlocked at one station and returned to another at anytime. 

The company has flexible pricing plans: single ride passes, full day passes, and annual memberships. ****************************Casual riders**************************** are those who purchase single ride or full day passes whilst ******Member riders****** are those who purchase annual memberships. 

Cyclistic’s finance analysts have concluded that member riders are more profitable than casual riders. Although the pricing flexibility helps the company attract more customers, Moreno believes that maximising the number of member riders will be key to future growth. She also believes there is a very good chance to convert casual riders into member riders, instead of focusing on creating a marketing campaign that targets all new customers. 

### Business task

Analyse the company’s historical bike trip data to identify trends into how casual riders and member riders use Cyclistic’s bikes differently.

### Stakeholders

- ************************Lily Moreno:************************ Cyclistic’s marketing director, responsible for the development of campaigns and initiatives to promote the bike share programme.
- **************************************************************Cyclistic’s marketing analytics team:************************************************************** A team of data analysts who are responsible for collecting, analysing and reporting data that helps guide the marketing strategy.
- ****************Cyclistic’s executive team:**************** Responsible for deciding whether to approve the recommended marketing programme or not.

## Ask

The following three questions will guide the analysis:

1. How do casual riders and member riders use Cyclistic bikes differently?
2. Why would casual riders buy a Cyclistic annual membership?
3. How can Cyclistic use digital media to influence casual riders to become member riders?

## Prepare

The data used in the project was made readily available by Motivate International Inc. under this [licence](https://ride.divvybikes.com/data-license-agreement). The datasets are named different as Cyclistic is fictional company. ***Divvy***, the name on the files, is a real bike share system operating in Chicago with similar numbers of bicycles and docking stations. Therefore, the data is appropriate and will allow us to explore how both customer types are using Cyclistic bikes. 

### Data organisation

Twelve CSV files consisting of the data sets between “202110-divvy-tripdata.zip” and “202209-divvy-tripdata.zip” were downloaded. This data corresponds to the period between **November 2021 and October 2022**. Various data can be found on the data sets such as available bike types for rental, the date and time of each rental bike and different start and finish stations can be found, alongside more information. The data set can be found [here](https://divvy-tripdata.s3.amazonaws.com/index.html).

Due to data privacy issues, it is imperative to note that it prevents us to have access to the rider’s personally identifiable information. This means that it will not be possible to connect pass purchases to credit card numbers to determine whether casual riders are from the Cyclistic service area, or if they have purchased many single ride passes.

### Data integrity

The data was collected first hand by Cyclistic, thus making it trusted. This then makes the data free from bias and would reflect the overall population. Moreover, the data is consistent as it’s provided in comma separated format (CSV) files and organised into rows and columns. 

### Data security

As encryption allows rider’s personally identifiable information to be protected, a copy of the downloaded files is backed up.

### Load required R packages

```r
library(tidyverse)
library(data.table)
library(readr)
library(lubridate)
library(janitor)
library(skimr)
library(dplyr)
library(tidyr)
```

### Import and load the data sets

```r
trip_1 <- read_csv("Documents/Cyclist_Bike_Share /Nov_Trip_Data.csv")
trip_2 <- read_csv("Documents/Cyclist_Bike_Share /Dec_Trip_Data.csv")
trip_3 <- read_csv("Documents/Cyclist_Bike_Share /Jan_Trip_Data.csv")
trip_4 <- read_csv("Documents/Cyclist_Bike_Share /Feb_Trip_Data.csv")
trip_5 <- read_csv("Documents/Cyclist_Bike_Share /Mar_Trip_Data.csv")
trip_6 <- read_csv("Documents/Cyclist_Bike_Share /Apr_Trip_Data.csv")
trip_7 <- read_csv("Documents/Cyclist_Bike_Share /May_Trip_Data.csv")
trip_8 <- read_csv("Documents/Cyclist_Bike_Share /Jun_Trip_Data.csv")
trip_9 <- read_csv("Documents/Cyclist_Bike_Share /Jul_Trip_Data.csv")
trip_10 <- read_csv("Documents/Cyclist_Bike_Share /Aug_Trip_Data.csv")
trip_11 <- read_csv("Documents/Cyclist_Bike_Share /Sep_Trip_Data.csv")
trip_12 <- read_csv("Documents/Cyclist_Bike_Share /Oct_Trip_Data.csv")
```

**Console output from loading data sets** 

```r
> trip_1 <- read_csv("Documents/Cyclist_Bike_Share /Nov_Trip_Data.csv")
Rows: 359978 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_2 <- read_csv("Documents/Cyclist_Bike_Share /Dec_Trip_Data.csv")
Rows: 247540 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_3 <- read_csv("Documents/Cyclist_Bike_Share /Jan_Trip_Data.csv")
Rows: 103770 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_4 <- read_csv("Documents/Cyclist_Bike_Share /Feb_Trip_Data.csv")
Rows: 115609 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_5 <- read_csv("Documents/Cyclist_Bike_Share /Mar_Trip_Data.csv")
Rows: 284042 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_6 <- read_csv("Documents/Cyclist_Bike_Share /Apr_Trip_Data.csv")
Rows: 371249 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_7 <- read_csv("Documents/Cyclist_Bike_Share /May_Trip_Data.csv")
Rows: 634858 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_8 <- read_csv("Documents/Cyclist_Bike_Share /Jun_Trip_Data.csv")
Rows: 769204 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_9 <- read_csv("Documents/Cyclist_Bike_Share /Jul_Trip_Data.csv")
Rows: 823488 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_10 <- read_csv("Documents/Cyclist_Bike_Share /Aug_Trip_Data.csv")
Rows: 785932 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_11 <- read_csv("Documents/Cyclist_Bike_Share /Sep_Trip_Data.csv")
Rows: 701339 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

> trip_12 <- read_csv("Documents/Cyclist_Bike_Share /Oct_Trip_Data.csv")
Rows: 558685 Columns: 13                                                                                                                                     
── Column specification ──────────────────────────────────────────────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_station_name, end_station_id, member_casual
dbl  (4): start_lat, start_lng, end_lat, end_lng
dttm (2): started_at, ended_at
```

### Merge all data sets into a single data frame

```r
all_trips <-
  rbind(trip_1, trip_2, trip_3, trip_4, trip_5, trip_6, trip_7, trip_8, trip_9, trip_10, trip_11, trip_12)
```

## Process

In order to process the data sets, R programming language will be used as aforementioned. In this section, the data will get cleaned and manipulated for further analysis. 

### Identify and view data set using functions

```r
head(all_trips)
```

******Console output******

```r
# A tibble: 6 × 13
  **ride_id**          **rideable_type started_at          ended_at            start_station_name    start_station_id end_station_name end_station_id start_lat start_lng end_lat end_lng membe…¹**
  <chr>            <chr>         <dttm>              <dttm>              <chr>                 <chr>            <chr>            <chr>              <dbl>     <dbl>   <dbl>   <dbl> <chr>  
1 7C00A93E10556E47 electric_bike 2021-11-27 13:27:38 2021-11-27 13:46:38 NA                    NA               NA               NA                  41.9     -87.7    42.0   -87.7 casual 
2 90854840DFD508BA electric_bike 2021-11-27 13:38:25 2021-11-27 13:56:10 NA                    NA               NA               NA                  42.0     -87.7    41.9   -87.7 casual 
3 0A7D10CDD144061C electric_bike 2021-11-26 22:03:34 2021-11-26 22:05:56 NA                    NA               NA               NA                  42.0     -87.7    42.0   -87.7 casual 
4 2F3BE33085BCFF02 electric_bike 2021-11-27 09:56:49 2021-11-27 10:01:50 NA                    NA               NA               NA                  41.9     -87.8    41.9   -87.8 casual 
5 D67B4781A19928D4 electric_bike 2021-11-26 19:09:28 2021-11-26 19:30:41 NA                    NA               NA               NA                  41.9     -87.6    41.9   -87.6 casual 
6 02F85C2C3C5F7D46 electric_bike 2021-11-26 18:34:07 2021-11-26 18:52:49 Michigan Ave & Oak St 13042            NA               NA                  41.9     -87.6    41.9   -87.6 casual 
# … with abbreviated variable name ¹member_casu
```

```r
tail(all_trips)
```

****************************Console output****************************

```r
# A tibble: 6 × 13
  **ride_id          rideable_type started_at          ended_at            start_station_name         start_station_id end_station_name       end_s…¹ start…² start…³ end_lat end_lng membe…⁴**
  <chr>            <chr>         <dttm>              <dttm>              <chr>                      <chr>            <chr>                  <chr>     <dbl>   <dbl>   <dbl>   <dbl> <chr>  
1 DA551F0A9C0DBCA2 classic_bike  2022-10-24 17:45:38 2022-10-24 17:48:02 Sedgwick St & North Ave    TA1307000038     Lincoln Ave & Roscoe … chargi…    41.9   -87.6    41.9   -87.7 member 
2 BC3BFA659C9AB6F1 classic_bike  2022-10-30 01:41:29 2022-10-30 01:57:16 Clifton Ave & Armitage Ave TA1307000163     Lincoln Ave & Roscoe … chargi…    41.9   -87.7    41.9   -87.7 casual 
3 ACD65450291CF95F classic_bike  2022-10-30 01:41:54 2022-10-30 01:57:09 Clifton Ave & Armitage Ave TA1307000163     Lincoln Ave & Roscoe … chargi…    41.9   -87.7    41.9   -87.7 casual 
4 4AAC03D1438E97CA classic_bike  2022-10-15 09:34:11 2022-10-15 10:03:21 Sedgwick St & North Ave    TA1307000038     Wabash Ave & Grand Ave TA1307…    41.9   -87.6    41.9   -87.6 casual 
5 8E6F3F29785E5D40 classic_bike  2022-10-09 10:21:34 2022-10-09 10:43:45 Sedgwick St & North Ave    TA1307000038     Damen Ave & Clybourn … 13271      41.9   -87.6    41.9   -87.7 member 
6 8D14CBE672431920 docked_bike   2022-10-22 13:17:13 2022-10-22 13:46:14 Clark St & Armitage Ave    13146            Wabash Ave & Grand Ave TA1307…    41.9   -87.6    41.9   -87.6 casual 
# … with abbreviated variable names ¹end_station_id, ²start_lat, ³start_lng, ⁴member_casua
```

```r
glimpse(all_trips)
```

****************************Console output****************************

```r
Rows: 5,755,694
Columns: 13
$ ride_id            <chr> "7C00A93E10556E47", "90854840DFD508BA", "0A7D10CDD144061C", "2F3BE33085BCFF02", "D67B4781A19928D4", "02F85C2C3C5F7D46", "EF780B807EF7835A", "17069CC749126036"…
$ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "elec…
$ started_at         <dttm> 2021-11-27 13:27:38, 2021-11-27 13:38:25, 2021-11-26 22:03:34, 2021-11-27 09:56:49, 2021-11-26 19:09:28, 2021-11-26 18:34:07, 2021-11-27 13:31:12, 2021-11-27…
$ ended_at           <dttm> 2021-11-27 13:46:38, 2021-11-27 13:56:10, 2021-11-26 22:05:56, 2021-11-27 10:01:50, 2021-11-26 19:30:41, 2021-11-26 18:52:49, 2021-11-27 13:37:12, 2021-11-27…
$ start_station_name <chr> NA, NA, NA, NA, NA, "Michigan Ave & Oak St", NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
$ start_station_id   <chr> NA, NA, NA, NA, NA, "13042", NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
$ end_station_name   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, "Clark St …
$ end_station_id     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, "TA1309000…
$ start_lat          <dbl> 41.93000, 41.96000, 41.96000, 41.94000, 41.90000, 41.90086, 41.81000, 41.95000, 41.80000, 41.78000, 41.91000, 41.92000, 41.92000, 41.92000, 41.90000, 41.78000…
$ start_lng          <dbl> -87.72000, -87.70000, -87.70000, -87.79000, -87.63000, -87.62379, -87.60000, -87.66000, -87.60000, -87.60000, -87.72000, -87.71000, -87.71000, -87.78000, -87.…
$ end_lat            <dbl> 41.96000, 41.92000, 41.96000, 41.93000, 41.88000, 41.90000, 41.80000, 41.95000, 41.79000, 41.80000, 41.92000, 41.97000, 41.91000, 41.92000, 41.88000, 41.80000…
$ end_lng            <dbl> -87.73000, -87.70000, -87.70000, -87.79000, -87.62000, -87.63000, -87.60000, -87.66000, -87.60000, -87.59000, -87.71000, -87.68000, -87.72000, -87.78000, -87.…
$ member_casual      <chr> "casual", "casual", "casual", "casual", "casual", "casual", "casual", "casual", "casual", "casual", "casual", "casual", "casual", "casual", "casual", "casual"…
```

```r
colnames(all_trips)
```

****************************Console output****************************

```r
[1] "ride_id"  [2] "rideable_type"  [3] "started_at" [4] "ended_at"  [5] "start_station_name". [6] "start_station_id"  [7] "end_station_name"  [8] "end_station_id"    
[9] "start_lat"  [10] "start_lng"  [11] "end_lat"  [12] "end_lng"  [13] "member_casual"
```

```r
nrow(all_trips)
```

****************************Console output****************************

```r
5755694
```

```r
dim(all_trips)
```

****************************Console output****************************

```r
[1] 5755694 [2] 13
```

```r
summary(all_trips)
```

****************************Console output****************************

```r
ride_id          rideable_type        started_at                        ended_at                      start_station_name start_station_id   end_station_name   end_station_id    
 Length:5755694     Length:5755694     Min.   :2021-11-01 00:00:14.00   Min.   :2021-11-01 00:04:06.00   Length:5755694     Length:5755694     Length:5755694     Length:5755694    
 Class :character   Class :character   1st Qu.:2022-04-27 16:40:09.00   1st Qu.:2022-04-27 16:51:40.25   Class :character   Class :character   Class :character   Class :character  
 Mode  :character   Mode  :character   Median :2022-06-30 18:31:03.00   Median :2022-06-30 18:49:28.00   Mode  :character   Mode  :character   Mode  :character   Mode  :character  
                                       Mean   :2022-06-13 23:04:32.59   Mean   :2022-06-13 23:23:58.99                                                                             
                                       3rd Qu.:2022-08-24 19:52:19.75   3rd Qu.:2022-08-24 20:10:05.75                                                                              
                                       Max.   :2022-10-31 23:59:33.00   Max.   :2022-11-07 04:53:58.00                                                                             
                                                                                                                                                                                    
   start_lat       start_lng         end_lat         end_lng       member_casual     
 Min.   :41.64   Min.   :-87.84   Min.   :41.39   Min.   :-88.97   Length:5755694    
 1st Qu.:41.88   1st Qu.:-87.66   1st Qu.:41.88   1st Qu.:-87.66   Class :character  
 Median :41.90   Median :-87.64   Median :41.90   Median :-87.64   Mode  :character  
 Mean   :41.90   Mean   :-87.65   Mean   :41.90   Mean   :-87.65                     
 3rd Qu.:41.93   3rd Qu.:-87.63   3rd Qu.:41.93   3rd Qu.:-87.63                     
 Max.   :45.64   Max.   :-73.80   Max.   :42.37   Max.   :-87.30                     
                                  NA's   :5835    NA's   :5835
```

```r
str(all_trips)
```

****************************Console output****************************

```r
spc_tbl_ [5,755,694 × 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ ride_id           : chr [1:5755694] "7C00A93E10556E47" "90854840DFD508BA" "0A7D10CDD144061C" "2F3BE33085BCFF02" ...
 $ rideable_type     : chr [1:5755694] "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
 $ started_at        : POSIXct[1:5755694], format: "2021-11-27 13:27:38" "2021-11-27 13:38:25" "2021-11-26 22:03:34" "2021-11-27 09:56:49" ...
 $ ended_at          : POSIXct[1:5755694], format: "2021-11-27 13:46:38" "2021-11-27 13:56:10" "2021-11-26 22:05:56" "2021-11-27 10:01:50" ...
 $ start_station_name: chr [1:5755694] NA NA NA NA ...
 $ start_station_id  : chr [1:5755694] NA NA NA NA ...
 $ end_station_name  : chr [1:5755694] NA NA NA NA ...
 $ end_station_id    : chr [1:5755694] NA NA NA NA ...
 $ start_lat         : num [1:5755694] 41.9 42 42 41.9 41.9 ...
 $ start_lng         : num [1:5755694] -87.7 -87.7 -87.7 -87.8 -87.6 ...
 $ end_lat           : num [1:5755694] 42 41.9 42 41.9 41.9 ...
 $ end_lng           : num [1:5755694] -87.7 -87.7 -87.7 -87.8 -87.6 ...
 $ member_casual     : chr [1:5755694] "casual" "casual" "casual" "casual" ...
 - attr(*, "spec")=
  .. cols(
  ..   ride_id = col_character(),
  ..   rideable_type = col_character(),
  ..   started_at = col_datetime(format = ""),
  ..   ended_at = col_datetime(format = ""),
  ..   start_station_name = col_character(),
  ..   start_station_id = col_character(),
  ..   end_station_name = col_character(),
  ..   end_station_id = col_character(),
  ..   start_lat = col_double(),
  ..   start_lng = col_double(),
  ..   end_lat = col_double(),
  ..   end_lng = col_double(),
  ..   member_casual = col_character()
  .. )
 - attr(*, "problems")=<externalptr>
```

### Data cleaning

The data set had multiple null values which needed to be sorted and filtered. This was achieved by running the following code.

### Remove N/A values

```r
is.na(all_trips)
colSums(is.na(all_trips))
```

****************************Console output****************************

```r
**ride_id rideable_type started_at ended_at start_station_name start_station_id end_station_name end_station_id start_lat start_lng end_lat end_lng member_casual**
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE               TRUE             TRUE             TRUE           TRUE     FALSE     FALSE   FALSE   FALSE         FALSE
 [ reached getOption("max.print") -- omitted 5755618 rows ]

> colSums(is.na(all_trips))
           ride_id      rideable_type         started_at           ended_at           start_station_name   start_station_id   end_station_name     end_station_id          start_lat 
                 0                  0                  0                  0                       878177             878177             940010             940010                  0 
         start_lng            end_lat            end_lng      member_casual 
                 0               5835               5835                  0
```

```r
all_trips <- all_trips[rowSums(is.na(all_trips))==0,]
is.na(all_trips)
```

******************Console output******************

```r
**ride_id  rideable_type started_at ended_at start_station_name start_station_id end_station_name end_station_id start_lat start_lng end_lat end_lng member_casual**
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
FALSE         FALSE      FALSE    FALSE              FALSE            FALSE            FALSE          FALSE     FALSE     FALSE   FALSE   FALSE         FALSE
 [ reached getOption("max.print") -- omitted 4410362 rows ]
```

```r
glimpse(all_trips)
```

****************************Console output****************************

```r
Rows: 4,410,438
Columns: 13
$ ride_id            <chr> "4CA9676997DAFFF6", "F3E84A230AF2D676", "A1F2C92308007968", "9B871C3B14E9BEC4", "2A81E957DD24A3DC", "A57F8F504A01004E", "C79A70DFD07E75D9", "41E7F8FB5640B521"…
$ rideable_type      <chr> "classic_bike", "classic_bike", "electric_bike", "classic_bike", "classic_bike", "electric_bike", "electric_bike", "electric_bike", "electric_bike", "electric…
$ started_at         <dttm> 2021-11-26 10:27:28, 2021-11-15 09:35:03, 2021-11-10 16:27:02, 2021-11-09 19:51:36, 2021-11-06 19:14:10, 2021-11-18 11:58:24, 2021-11-23 22:14:11, 2021-11-05…
$ ended_at           <dttm> 2021-11-26 11:22:13, 2021-11-15 09:42:08, 2021-11-10 17:04:28, 2021-11-09 20:11:17, 2021-11-06 19:33:19, 2021-11-18 12:08:35, 2021-11-23 22:44:01, 2021-11-05…
$ start_station_name <chr> "Michigan Ave & Oak St", "Clark St & Grace St", "Leamington Ave & Hirsch St", "Desplaines St & Kinzie St", "Larrabee St & Armitage Ave", "Michigan Ave & Oak S…
$ start_station_id   <chr> "13042", "TA1307000127", "307", "TA1306000003", "TA1309000006", "13042", "604", "TA1307000127", "TA1306000003", "KA1503000043", "TA1306000003", "KA1503000043"…
$ end_station_name   <chr> "Michigan Ave & Oak St", "Clark St & Leland Ave", "Leamington Ave & Hirsch St", "Desplaines St & Kinzie St", "Michigan Ave & Oak St", "Michigan Ave & Oak St",…
$ end_station_id     <chr> "13042", "TA1309000014", "307", "TA1306000003", "13042", "13042", "604", "TA1309000014", "TA1305000006", "TA1306000003", "KA1503000043", "KA1503000043", "TA13…
$ start_lat          <dbl> 41.90096, 41.95078, 41.91000, 41.88872, 41.91808, 41.90114, 42.05820, 41.95086, 41.88863, 41.88915, 41.88872, 41.88918, 41.88918, 41.88132, 41.90096, 41.95097…
$ start_lng          <dbl> -87.62378, -87.65917, -87.75000, -87.64445, -87.64375, -87.62353, -87.67755, -87.65913, -87.64437, -87.63849, -87.64445, -87.63851, -87.63851, -87.62952, -87.…
$ end_lat            <dbl> 41.90096, 41.96710, 41.91000, 41.88872, 41.90096, 41.90100, 42.05825, 41.96707, 41.88160, 41.88872, 41.88918, 41.88918, 41.88132, 41.88132, 41.90096, 41.96712…
$ end_lng            <dbl> -87.62378, -87.66743, -87.75000, -87.64445, -87.62378, -87.62375, -87.67752, -87.66745, -87.63013, -87.64427, -87.63851, -87.63851, -87.62952, -87.62952, -87.…
$ member_casual      <chr> "casual", "casual", "casual", "casual", "casual", "casual", "casual", "member", "member", "member", "member", "member", "member", "member", "casual", "member"…
```

The null values have now been viewed and removed correspondingly. This will then enable ease for data manipulation. 

### Data manipulation

At this stage the data was altered for easier use and to make calculations.

### Create “ride_distance” column to calculate rider distances

```r
all_trips <- all_trips %>%
  mutate(ride_distance = distHaversine(cbind(start_lng, start_lat),cbind(end_lng,end_lat)))

head(all_trips)
```

****************************Console output****************************

```r
# A tibble: 6 × 14
  **ride_id          rideable_type started_at          ended_at            start_station_name         start_station_id end_station_…¹ end_s…² start…³ start…⁴ end_lat end_lng membe…⁵ ride_…⁶**
  <chr>            <chr>         <dttm>              <dttm>              <chr>                      <chr>            <chr>          <chr>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl>
1 4CA9676997DAFFF6 classic_bike  2021-11-26 10:27:28 2021-11-26 11:22:13 Michigan Ave & Oak St      13042            Michigan Ave … 13042      41.9   -87.6    41.9   -87.6 casual      0  
2 F3E84A230AF2D676 classic_bike  2021-11-15 09:35:03 2021-11-15 09:42:08 Clark St & Grace St        TA1307000127     Clark St & Le… TA1309…    42.0   -87.7    42.0   -87.7 casual   1941. 
3 A1F2C92308007968 electric_bike 2021-11-10 16:27:02 2021-11-10 17:04:28 Leamington Ave & Hirsch St 307              Leamington Av… 307        41.9   -87.8    41.9   -87.8 casual      0  
4 9B871C3B14E9BEC4 classic_bike  2021-11-09 19:51:36 2021-11-09 20:11:17 Desplaines St & Kinzie St  TA1306000003     Desplaines St… TA1306…    41.9   -87.6    41.9   -87.6 casual      0  
5 2A81E957DD24A3DC classic_bike  2021-11-06 19:14:10 2021-11-06 19:33:19 Larrabee St & Armitage Ave TA1309000006     Michigan Ave … 13042      41.9   -87.6    41.9   -87.6 casual   2524. 
6 A57F8F504A01004E electric_bike 2021-11-18 11:58:24 2021-11-18 12:08:35 Michigan Ave & Oak St      13042            Michigan Ave … 13042      41.9   -87.6    41.9   -87.6 casual     23.7
# … with abbreviated variable names ¹end_station_name, ²end_station_id, ³start_lat, ⁴start_lng, ⁵member_casual, ⁶ride_distance
```

### Change member_casual column to “usertype”

```r
all_trips <- all_trips %>%
  rename(usertype = member_casual)

colnames(all_trips)
```

****************************Console output****************************

```r
[1] "ride_id"  [2] "rideable_type"  [3] "started_at"  [4] "ended_at"  [5] "start_station_name"  [6] "start_station_id"  [7] "end_station_name"  [8] "end_station_id"    
 [9] "start_lat". [10] "start_lng"  [11] "end_lat"  [12] "end_lng"  [13] "usertype" [14] "ride_distance"
```

```r
table(all_trips$usertype)
```

****************************Console output****************************

```r
casual  member 
1768182 2642256
```

```r
head(all_trips)
```

****************************Console output****************************

```r
# A tibble: 6 × 14
  **ride_id          rideable_type started_at          ended_at            start_station_name         start_station_id end_station_…¹ end_s…² start…³ start…⁴ end_lat end_lng usert…⁵ ride_…⁶**
  <chr>            <chr>         <dttm>              <dttm>              <chr>                      <chr>            <chr>          <chr>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl>
1 4CA9676997DAFFF6 classic_bike  2021-11-26 10:27:28 2021-11-26 11:22:13 Michigan Ave & Oak St      13042            Michigan Ave … 13042      41.9   -87.6    41.9   -87.6 casual      0  
2 F3E84A230AF2D676 classic_bike  2021-11-15 09:35:03 2021-11-15 09:42:08 Clark St & Grace St        TA1307000127     Clark St & Le… TA1309…    42.0   -87.7    42.0   -87.7 casual   1941. 
3 A1F2C92308007968 electric_bike 2021-11-10 16:27:02 2021-11-10 17:04:28 Leamington Ave & Hirsch St 307              Leamington Av… 307        41.9   -87.8    41.9   -87.8 casual      0  
4 9B871C3B14E9BEC4 classic_bike  2021-11-09 19:51:36 2021-11-09 20:11:17 Desplaines St & Kinzie St  TA1306000003     Desplaines St… TA1306…    41.9   -87.6    41.9   -87.6 casual      0  
5 2A81E957DD24A3DC classic_bike  2021-11-06 19:14:10 2021-11-06 19:33:19 Larrabee St & Armitage Ave TA1309000006     Michigan Ave … 13042      41.9   -87.6    41.9   -87.6 casual   2524. 
6 A57F8F504A01004E electric_bike 2021-11-18 11:58:24 2021-11-18 12:08:35 Michigan Ave & Oak St      13042            Michigan Ave … 13042      41.9   -87.6    41.9   -87.6 casual     23.7
# … with abbreviated variable names ¹end_station_name, ²end_station_id, ³start_lat, ⁴start_lng, ⁵usertype, ⁶ride_distance
```

```r
tail(all_trips)
```

****************************Console output****************************

```r
# A tibble: 6 × 14
  **ride_id          rideable_type started_at          ended_at            start_station_name         start_station_id end_station_…¹ end_s…² start…³ start…⁴ end_lat end_lng usert…⁵ ride_…⁶**
  <chr>            <chr>         <dttm>              <dttm>              <chr>                      <chr>            <chr>          <chr>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl>
1 DA551F0A9C0DBCA2 classic_bike  2022-10-24 17:45:38 2022-10-24 17:48:02 Sedgwick St & North Ave    TA1307000038     Lincoln Ave &… chargi…    41.9   -87.6    41.9   -87.7 member    4436.
2 BC3BFA659C9AB6F1 classic_bike  2022-10-30 01:41:29 2022-10-30 01:57:16 Clifton Ave & Armitage Ave TA1307000163     Lincoln Ave &… chargi…    41.9   -87.7    41.9   -87.7 casual    3020.
3 ACD65450291CF95F classic_bike  2022-10-30 01:41:54 2022-10-30 01:57:09 Clifton Ave & Armitage Ave TA1307000163     Lincoln Ave &… chargi…    41.9   -87.7    41.9   -87.7 casual    3020.
4 4AAC03D1438E97CA classic_bike  2022-10-15 09:34:11 2022-10-15 10:03:21 Sedgwick St & North Ave    TA1307000038     Wabash Ave & … TA1307…    41.9   -87.6    41.9   -87.6 casual    2427.
5 8E6F3F29785E5D40 classic_bike  2022-10-09 10:21:34 2022-10-09 10:43:45 Sedgwick St & North Ave    TA1307000038     Damen Ave & C… 13271      41.9   -87.6    41.9   -87.7 member    3970.
6 8D14CBE672431920 docked_bike   2022-10-22 13:17:13 2022-10-22 13:46:14 Clark St & Armitage Ave    13146            Wabash Ave & … TA1307…    41.9   -87.6    41.9   -87.6 casual    3090.
# … with abbreviated variable names ¹end_station_name, ²end_station_id, ³start_lat, ⁴start_lng, ⁵usertype, ⁶ride_distance
```

```r
dim(all_trips)
```

****************************Console output****************************

```r
[1] 4410438 [2] 14
```

### Calculate ride data for each day, month and year. Also create a “weekday” column

```r
all_trips$date <- as.Date(all_trips$started_at)
all_trips$month <- format(as.Date(all_trips$date), "%m")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$weekday <- format(as.Date(all_trips$date), "%A")

head(all_trips)
```

****************************Console output****************************

```r
# A tibble: 6 × 19
  **ride_id          rideable_…¹ started_at          ended_at            start…² start…³ end_s…⁴ end_s…⁵ start…⁶ start…⁷ end_lat end_lng usert…⁸ ride_…⁹ date       month day   year  weekday**
  <chr>            <chr>       <dttm>              <dttm>              <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <date>     <chr> <chr> <chr> <chr>  
1 4CA9676997DAFFF6 classic_bi… 2021-11-26 10:27:28 2021-11-26 11:22:13 Michig… 13042   Michig… 13042      41.9   -87.6    41.9   -87.6 casual      0   2021-11-26 11    26    2021  Friday 
2 F3E84A230AF2D676 classic_bi… 2021-11-15 09:35:03 2021-11-15 09:42:08 Clark … TA1307… Clark … TA1309…    42.0   -87.7    42.0   -87.7 casual   1941.  2021-11-15 11    15    2021  Monday 
3 A1F2C92308007968 electric_b… 2021-11-10 16:27:02 2021-11-10 17:04:28 Leamin… 307     Leamin… 307        41.9   -87.8    41.9   -87.8 casual      0   2021-11-10 11    10    2021  Wednes…
4 9B871C3B14E9BEC4 classic_bi… 2021-11-09 19:51:36 2021-11-09 20:11:17 Despla… TA1306… Despla… TA1306…    41.9   -87.6    41.9   -87.6 casual      0   2021-11-09 11    09    2021  Tuesday
5 2A81E957DD24A3DC classic_bi… 2021-11-06 19:14:10 2021-11-06 19:33:19 Larrab… TA1309… Michig… 13042      41.9   -87.6    41.9   -87.6 casual   2524.  2021-11-06 11    06    2021  Saturd…
6 A57F8F504A01004E electric_b… 2021-11-18 11:58:24 2021-11-18 12:08:35 Michig… 13042   Michig… 13042      41.9   -87.6    41.9   -87.6 casual     23.7 2021-11-18 11    18    2021  Thursd…
# … with abbreviated variable names ¹rideable_type, ²start_station_name, ³start_station_id, ⁴end_station_name, ⁵end_station_id, ⁶start_lat, ⁷start_lng, ⁸usertype, ⁹ride_distance
```

```r
tail(all_trips)
```

****************************Console output****************************

```r
# A tibble: 6 × 19
  **ride_id          rideable_…¹ started_at          ended_at            start…² start…³ end_s…⁴ end_s…⁵ start…⁶ start…⁷ end_lat end_lng usert…⁸ ride_…⁹ date       month day   year  weekday**
  <chr>            <chr>       <dttm>              <dttm>              <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <date>     <chr> <chr> <chr> <chr>  
1 DA551F0A9C0DBCA2 classic_bi… 2022-10-24 17:45:38 2022-10-24 17:48:02 Sedgwi… TA1307… Lincol… chargi…    41.9   -87.6    41.9   -87.7 member    4436. 2022-10-24 10    24    2022  Monday 
2 BC3BFA659C9AB6F1 classic_bi… 2022-10-30 01:41:29 2022-10-30 01:57:16 Clifto… TA1307… Lincol… chargi…    41.9   -87.7    41.9   -87.7 casual    3020. 2022-10-30 10    30    2022  Sunday 
3 ACD65450291CF95F classic_bi… 2022-10-30 01:41:54 2022-10-30 01:57:09 Clifto… TA1307… Lincol… chargi…    41.9   -87.7    41.9   -87.7 casual    3020. 2022-10-30 10    30    2022  Sunday 
4 4AAC03D1438E97CA classic_bi… 2022-10-15 09:34:11 2022-10-15 10:03:21 Sedgwi… TA1307… Wabash… TA1307…    41.9   -87.6    41.9   -87.6 casual    2427. 2022-10-15 10    15    2022  Saturd…
5 8E6F3F29785E5D40 classic_bi… 2022-10-09 10:21:34 2022-10-09 10:43:45 Sedgwi… TA1307… Damen … 13271      41.9   -87.6    41.9   -87.7 member    3970. 2022-10-09 10    09    2022  Sunday 
6 8D14CBE672431920 docked_bike 2022-10-22 13:17:13 2022-10-22 13:46:14 Clark … 13146   Wabash… TA1307…    41.9   -87.6    41.9   -87.6 casual    3090. 2022-10-22 10    22    2022  Saturd…
# … with abbreviated variable names ¹rideable_type, ²start_station_name, ³start_station_id, ⁴end_station_name, ⁵end_station_id, ⁶start_lat, ⁷start_lng, ⁸usertype, ⁹ride_distance
```

```r
str(all_trips)
```

**********Console output**********

```r
tibble [4,410,438 × 19] (S3: tbl_df/tbl/data.frame)
 $ ride_id           : chr [1:4410438] "4CA9676997DAFFF6" "F3E84A230AF2D676" "A1F2C92308007968" "9B871C3B14E9BEC4" ...
 $ rideable_type     : chr [1:4410438] "classic_bike" "classic_bike" "electric_bike" "classic_bike" ...
 $ started_at        : POSIXct[1:4410438], format: "2021-11-26 10:27:28" "2021-11-15 09:35:03" "2021-11-10 16:27:02" "2021-11-09 19:51:36" ...
 $ ended_at          : POSIXct[1:4410438], format: "2021-11-26 11:22:13" "2021-11-15 09:42:08" "2021-11-10 17:04:28" "2021-11-09 20:11:17" ...
 $ start_station_name: chr [1:4410438] "Michigan Ave & Oak St" "Clark St & Grace St" "Leamington Ave & Hirsch St" "Desplaines St & Kinzie St" ...
 $ start_station_id  : chr [1:4410438] "13042" "TA1307000127" "307" "TA1306000003" ...
 $ end_station_name  : chr [1:4410438] "Michigan Ave & Oak St" "Clark St & Leland Ave" "Leamington Ave & Hirsch St" "Desplaines St & Kinzie St" ...
 $ end_station_id    : chr [1:4410438] "13042" "TA1309000014" "307" "TA1306000003" ...
 $ start_lat         : num [1:4410438] 41.9 42 41.9 41.9 41.9 ...
 $ start_lng         : num [1:4410438] -87.6 -87.7 -87.8 -87.6 -87.6 ...
 $ end_lat           : num [1:4410438] 41.9 42 41.9 41.9 41.9 ...
 $ end_lng           : num [1:4410438] -87.6 -87.7 -87.8 -87.6 -87.6 ...
 $ usertype          : chr [1:4410438] "casual" "casual" "casual" "casual" ...
 $ ride_distance     : num [1:4410438] 0 1941 0 0 2524 ...
 $ date              : Date[1:4410438], format: "2021-11-26" "2021-11-15" "2021-11-10" "2021-11-09" ...
 $ month             : chr [1:4410438] "11" "11" "11" "11" ...
 $ day               : chr [1:4410438] "26" "15" "10" "09" ...
 $ year              : chr [1:4410438] "2021" "2021" "2021" "2021" ...
 $ weekday           : chr [1:4410438] "Friday" "Monday" "Wednesday" "Tuesday" ...
```

### Calculate trip duration in seconds and store as "ride_length”

```r
all_trips$ride_length <- difftime(all_trips$ended_at, all_trips$started_at)

head(all_trips)
```

********Console output********

```r
# A tibble: 6 × 20
  **ride_id      ridea…¹ started_at          ended_at            start…² start…³ end_s…⁴ end_s…⁵ start…⁶ start…⁷ end_lat end_lng usert…⁸ ride_…⁹ date       month day   year  weekday˟ ride_…˟**
  <chr>        <chr>   <dttm>              <dttm>              <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <date>     <chr> <chr> <chr> <chr>    <drtn> 
1 4CA9676997D… classi… 2021-11-26 10:27:28 2021-11-26 11:22:13 Michig… 13042   Michig… 13042      41.9   -87.6    41.9   -87.6 casual      0   2021-11-26 11    26    2021  Friday   3285 s…
2 F3E84A230AF… classi… 2021-11-15 09:35:03 2021-11-15 09:42:08 Clark … TA1307… Clark … TA1309…    42.0   -87.7    42.0   -87.7 casual   1941.  2021-11-15 11    15    2021  Monday    425 s…
3 A1F2C923080… electr… 2021-11-10 16:27:02 2021-11-10 17:04:28 Leamin… 307     Leamin… 307        41.9   -87.8    41.9   -87.8 casual      0   2021-11-10 11    10    2021  Wednes…  2246 s…
4 9B871C3B14E… classi… 2021-11-09 19:51:36 2021-11-09 20:11:17 Despla… TA1306… Despla… TA1306…    41.9   -87.6    41.9   -87.6 casual      0   2021-11-09 11    09    2021  Tuesday  1181 s…
5 2A81E957DD2… classi… 2021-11-06 19:14:10 2021-11-06 19:33:19 Larrab… TA1309… Michig… 13042      41.9   -87.6    41.9   -87.6 casual   2524.  2021-11-06 11    06    2021  Saturd…  1149 s…
6 A57F8F504A0… electr… 2021-11-18 11:58:24 2021-11-18 12:08:35 Michig… 13042   Michig… 13042      41.9   -87.6    41.9   -87.6 casual     23.7 2021-11-18 11    18    2021  Thursd…   611 s…
# … with abbreviated variable names ¹rideable_type, ²start_station_name, ³start_station_id, ⁴end_station_name, ⁵end_station_id, ⁶start_lat, ⁷start_lng, ⁸usertype, ⁹ride_distance,
#   ˟weekday, ˟ride_length
```

```r
str(all_trips)
```

****************************Console output****************************

```r
tibble [4,410,438 × 20] (S3: tbl_df/tbl/data.frame)
 $ ride_id           : chr [1:4410438] "4CA9676997DAFFF6" "F3E84A230AF2D676" "A1F2C92308007968" "9B871C3B14E9BEC4" ...
 $ rideable_type     : chr [1:4410438] "classic_bike" "classic_bike" "electric_bike" "classic_bike" ...
 $ started_at        : POSIXct[1:4410438], format: "2021-11-26 10:27:28" "2021-11-15 09:35:03" "2021-11-10 16:27:02" "2021-11-09 19:51:36" ...
 $ ended_at          : POSIXct[1:4410438], format: "2021-11-26 11:22:13" "2021-11-15 09:42:08" "2021-11-10 17:04:28" "2021-11-09 20:11:17" ...
 $ start_station_name: chr [1:4410438] "Michigan Ave & Oak St" "Clark St & Grace St" "Leamington Ave & Hirsch St" "Desplaines St & Kinzie St" ...
 $ start_station_id  : chr [1:4410438] "13042" "TA1307000127" "307" "TA1306000003" ...
 $ end_station_name  : chr [1:4410438] "Michigan Ave & Oak St" "Clark St & Leland Ave" "Leamington Ave & Hirsch St" "Desplaines St & Kinzie St" ...
 $ end_station_id    : chr [1:4410438] "13042" "TA1309000014" "307" "TA1306000003" ...
 $ start_lat         : num [1:4410438] 41.9 42 41.9 41.9 41.9 ...
 $ start_lng         : num [1:4410438] -87.6 -87.7 -87.8 -87.6 -87.6 ...
 $ end_lat           : num [1:4410438] 41.9 42 41.9 41.9 41.9 ...
 $ end_lng           : num [1:4410438] -87.6 -87.7 -87.8 -87.6 -87.6 ...
 $ usertype          : chr [1:4410438] "casual" "casual" "casual" "casual" ...
 $ ride_distance     : num [1:4410438] 0 1941 0 0 2524 ...
 $ date              : Date[1:4410438], format: "2021-11-26" "2021-11-15" "2021-11-10" "2021-11-09" ...
 $ month             : chr [1:4410438] "11" "11" "11" "11" ...
 $ day               : chr [1:4410438] "26" "15" "10" "09" ...
 $ year              : chr [1:4410438] "2021" "2021" "2021" "2021" ...
 $ weekday           : chr [1:4410438] "Friday" "Monday" "Wednesday" "Tuesday" ...
 $ ride_length       : 'difftime' num [1:4410438] 3285 425 2246 1181 ...
  ..- attr(*, "units")= chr "secs"
```

### Convert "ride_length" from difftime to numeric to run calculations on data set

```r
is.difftime(all_trips$ride_length)
```

****************************Console output****************************

```r
TRUE
```

```r
all_trips$ride_length <- as.numeric(as.difftime(all_trips$ride_length))

is.numeric(all_trips$ride_length)
```

****************************Console output****************************

```r
TRUE
```

```r
str(all_trips)
```

****************************Console output****************************

```r
tibble [4,410,438 × 20] (S3: tbl_df/tbl/data.frame)
 $ ride_id           : chr [1:4410438] "4CA9676997DAFFF6" "F3E84A230AF2D676" "A1F2C92308007968" "9B871C3B14E9BEC4" ...
 $ rideable_type     : chr [1:4410438] "classic_bike" "classic_bike" "electric_bike" "classic_bike" ...
 $ started_at        : POSIXct[1:4410438], format: "2021-11-26 10:27:28" "2021-11-15 09:35:03" "2021-11-10 16:27:02" "2021-11-09 19:51:36" ...
 $ ended_at          : POSIXct[1:4410438], format: "2021-11-26 11:22:13" "2021-11-15 09:42:08" "2021-11-10 17:04:28" "2021-11-09 20:11:17" ...
 $ start_station_name: chr [1:4410438] "Michigan Ave & Oak St" "Clark St & Grace St" "Leamington Ave & Hirsch St" "Desplaines St & Kinzie St" ...
 $ start_station_id  : chr [1:4410438] "13042" "TA1307000127" "307" "TA1306000003" ...
 $ end_station_name  : chr [1:4410438] "Michigan Ave & Oak St" "Clark St & Leland Ave" "Leamington Ave & Hirsch St" "Desplaines St & Kinzie St" ...
 $ end_station_id    : chr [1:4410438] "13042" "TA1309000014" "307" "TA1306000003" ...
 $ start_lat         : num [1:4410438] 41.9 42 41.9 41.9 41.9 ...
 $ start_lng         : num [1:4410438] -87.6 -87.7 -87.8 -87.6 -87.6 ...
 $ end_lat           : num [1:4410438] 41.9 42 41.9 41.9 41.9 ...
 $ end_lng           : num [1:4410438] -87.6 -87.7 -87.8 -87.6 -87.6 ...
 $ usertype          : chr [1:4410438] "casual" "casual" "casual" "casual" ...
 $ ride_distance     : num [1:4410438] 0 1941 0 0 2524 ...
 $ date              : Date[1:4410438], format: "2021-11-26" "2021-11-15" "2021-11-10" "2021-11-09" ...
 $ month             : chr [1:4410438] "11" "11" "11" "11" ...
 $ day               : chr [1:4410438] "26" "15" "10" "09" ...
 $ year              : chr [1:4410438] "2021" "2021" "2021" "2021" ...
 $ weekday           : chr [1:4410438] "Friday" "Monday" "Wednesday" "Tuesday" ...
 $ ride_length       : num [1:4410438] 3285 425 2246 1181 1149 ...
```

### Remove rides that are less than 0 seconds and 0 metres

```r
all_trips <- all_trips %>%
+   filter(ride_length > 0) %>%
+   filter(ride_distance > 0)

head(all_trips)
```

****************************Console output****************************

```r
# A tibble: 6 × 20
  **ride_id      ridea…¹ started_at          ended_at            start…² start…³ end_s…⁴ end_s…⁵ start…⁶ start…⁷ end_lat end_lng usert…⁸ ride_…⁹ date       month day   year  weekday˟  ride_…˟**
  <chr>        <chr>   <dttm>              <dttm>              <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <date>     <chr> <chr> <chr> <chr>     <dbl>
1 F3E84A230AF… classi… 2021-11-15 09:35:03 2021-11-15 09:42:08 Clark … TA1307… Clark … TA1309…    42.0   -87.7    42.0   -87.7 casual  1941.   2021-11-15 11    15    2021  Monday      425
2 2A81E957DD2… classi… 2021-11-06 19:14:10 2021-11-06 19:33:19 Larrab… TA1309… Michig… 13042      41.9   -87.6    41.9   -87.6 casual  2524.   2021-11-06 11    06    2021  Saturd…    1149
3 A57F8F504A0… electr… 2021-11-18 11:58:24 2021-11-18 12:08:35 Michig… 13042   Michig… 13042      41.9   -87.6    41.9   -87.6 casual    23.7  2021-11-18 11    18    2021  Thursd…     611
4 C79A70DFD07… electr… 2021-11-23 22:14:11 2021-11-23 22:44:01 Sherid… 604     Sherid… 604        42.1   -87.7    42.1   -87.7 casual     6.63 2021-11-23 11    23    2021  Tuesday    1790
5 41E7F8FB564… electr… 2021-11-05 16:48:10 2021-11-05 16:53:18 Clark … TA1307… Clark … TA1309…    42.0   -87.7    42.0   -87.7 member  1932.   2021-11-05 11    05    2021  Friday      308
6 3DEAAA1701B… electr… 2021-11-18 08:40:38 2021-11-18 08:48:56 Despla… TA1306… Dearbo… TA1305…    41.9   -87.6    41.9   -87.6 member  1416.   2021-11-18 11    18    2021  Thursd…     498
# … with abbreviated variable names ¹rideable_type, ²start_station_name, ³start_station_id, ⁴end_station_name, ⁵end_station_id, ⁶start_lat, ⁷start_lng, ⁸usertype, ⁹ride_distance,
#   ˟weekday, ˟ride_length
```

```r
dim(all_trips)
```

****************************Console output****************************

```r
[1] 4207351 [2] 20
```

## Analyse

This section will cover the calculations performed, trends and relationships identified and a conduction of descriptive analysis on the data set.

### Descriptive ride length (in seconds) analysis

```r
summary(all_trips$ride_length)
```

****************************Console output****************************

```r
Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
  1.0      371       636     992.6     1114   2061244
```

### Summary statistics to compare ride length of casual and member riders

```r
aggregate(all_trips$ride_length ~ all_trips$usertype, FUN = mean)
  all_trips$usertype all_trips$ride_length

aggregate(all_trips$ride_length ~ all_trips$usertype, FUN = median)
  all_trips$usertype all_trips$ride_length

aggregate(all_trips$ride_length ~ all_trips$usertype, FUN = max)
  all_trips$usertype all_trips$ride_length

aggregate(all_trips$ride_length ~ all_trips$usertype, FUN = min)
  all_trips$usertype all_trips$ride_length
```

****************************Console output****************************

```r
1  casual   1384.6729
2  member    741.3947
```

```r
1  casual   826
2  member   541
```

```r
1  casual   2061244
2  member     89575
```

```r
1  casual   1
2  member   1
```

### Summary statistics to compare ride distance of casual and member riders

```r
aggregate(all_trips$ride_distance ~ all_trips$usertype, FUN = mean)
  all_trips$usertype all_trips$ride_distance

aggregate(all_trips$ride_distance ~ all_trips$usertype, FUN = median)
  all_trips$usertype all_trips$ride_distance

aggregate(all_trips$ride_distance ~ all_trips$usertype, FUN = max)
  all_trips$usertype all_trips$ride_distance

aggregate(all_trips$ride_distance ~ all_trips$usertype, FUN = min)
  all_trips$usertype all_trips$ride_distance
```

****************************Console output****************************

```r
1  casual   2323.760
2  member   2111.424
```

```r
1  casual   1780.596
2  member   1548.659
```

```r
1  casual   1190854.54
2  member     27518.42
```

```r
1  casual   0.03321865
2  member   0.02016286
```

### Average ride length by weekday for casual and member riders

```r
aggregate (all_trips$ride_length ~ all_trips$usertype + all_trips$weekday, FUN = mean)
   all_trips$usertype all_trips$weekday all_trips$ride_length
```

****************************Console output****************************

```r
1              casual            Friday             1298.8332
2              member            Friday              727.8972
3              casual            Monday             1413.7970
4              member            Monday              712.3448
5              casual          Saturday             1550.9885
6              member          Saturday              838.9709
7              casual            Sunday             1563.3295
8              member            Sunday              825.7307
9              casual          Thursday             1242.5742
10             member          Thursday              715.6762
11             casual           Tuesday             1231.2016
12             member           Tuesday              700.8877
13             casual         Wednesday             1197.2032
14             member         Wednesday              704.7405
```

### Average ride distance by weekday for casual and member riders

```r
aggregate (all_trips$ride_distance ~ all_trips$usertype + all_trips$weekday, FUN = mean)
   all_trips$usertype all_trips$weekday all_trips$ride_distance
```

****************************Console output****************************

```r
1              casual            Friday                2273.075
2              member            Friday                2066.173
3              casual            Monday                2258.196
4              member            Monday                2067.225
5              casual          Saturday                2462.863
6              member          Saturday                2230.627
7              casual            Sunday                2418.339
8              member            Sunday                2184.793
9              casual          Thursday                2256.727
10             member          Thursday                2096.145
11             casual           Tuesday                2227.397
12             member           Tuesday                2076.137
13             casual         Wednesday                2226.972
14             member         Wednesday                2091.090
```

### Correct the weekday order

```r
all_trips$weekday <- ordered(all_trips$weekday, levels=c ("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday" ))
```

### Re-run the previous two average codes for correct weekday order

```r
aggregate (all_trips$ride_length ~ all_trips$usertype + all_trips$weekday, FUN = mean)
   all_trips$usertype all_trips$weekday all_trips$ride_length
```

****************************Console output****************************

```r
1              casual            Monday             1413.7970
2              member            Monday              712.3448
3              casual           Tuesday             1231.2016
4              member           Tuesday              700.8877
5              casual         Wednesday             1197.2032
6              member         Wednesday              704.7405
7              casual          Thursday             1242.5742
8              member          Thursday              715.6762
9              casual            Friday             1298.8332
10             member            Friday              727.8972
11             casual          Saturday             1550.9885
12             member          Saturday              838.9709
13             casual            Sunday             1563.3295
14             member            Sunday              825.7307
```

```r
aggregate (all_trips$ride_distance ~ all_trips$usertype + all_trips$weekday, FUN = mean)
   all_trips$usertype all_trips$weekday all_trips$ride_distance
```

****************************Console output****************************

```r
1              casual            Monday                2258.196
2              member            Monday                2067.225
3              casual           Tuesday                2227.397
4              member           Tuesday                2076.137
5              casual         Wednesday                2226.972
6              member         Wednesday                2091.090
7              casual          Thursday                2256.727
8              member          Thursday                2096.145
9              casual            Friday                2273.075
10             member            Friday                2066.173
11             casual          Saturday                2462.863
12             member          Saturday                2230.627
13             casual            Sunday                2418.339
14             member            Sunday                2184.793
```

```r
head(all_trips)
```

****************************Console output****************************

```r
# A tibble: 6 × 21
  **ride_id      ridea…¹ started_at          ended_at            start…² start…³ end_s…⁴ end_s…⁵ start…⁶ start…⁷ end_lat end_lng usert…⁸ ride_…⁹ date       month day   year  weekday˟  ride_…˟**
  <chr>        <chr>   <dttm>              <dttm>              <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <date>     <chr> <chr> <chr> <chr>     <dbl>
1 F3E84A230AF… classi… 2021-11-15 09:35:03 2021-11-15 09:42:08 Clark … TA1307… Clark … TA1309…    42.0   -87.7    42.0   -87.7 casual  1941.   2021-11-15 11    15    2021  Monday      425
2 2A81E957DD2… classi… 2021-11-06 19:14:10 2021-11-06 19:33:19 Larrab… TA1309… Michig… 13042      41.9   -87.6    41.9   -87.6 casual  2524.   2021-11-06 11    06    2021  Saturd…    1149
3 A57F8F504A0… electr… 2021-11-18 11:58:24 2021-11-18 12:08:35 Michig… 13042   Michig… 13042      41.9   -87.6    41.9   -87.6 casual    23.7  2021-11-18 11    18    2021  Thursd…     611
4 C79A70DFD07… electr… 2021-11-23 22:14:11 2021-11-23 22:44:01 Sherid… 604     Sherid… 604        42.1   -87.7    42.1   -87.7 casual     6.63 2021-11-23 11    23    2021  Tuesday    1790
5 41E7F8FB564… electr… 2021-11-05 16:48:10 2021-11-05 16:53:18 Clark … TA1307… Clark … TA1309…    42.0   -87.7    42.0   -87.7 member  1932.   2021-11-05 11    05    2021  Friday      308
6 3DEAAA1701B… electr… 2021-11-18 08:40:38 2021-11-18 08:48:56 Despla… TA1306… Dearbo… TA1305…    41.9   -87.6    41.9   -87.6 member  1416.   2021-11-18 11    18    2021  Thursd…     498
# … with 1 more variable: weekday <ord>, and abbreviated variable names ¹rideable_type, ²start_station_name, ³start_station_id, ⁴end_station_name, ⁵end_station_id, ⁶start_lat,
#   ⁷start_lng, ⁸usertype, ⁹ride_distance, ˟weekday, ˟ride_length
```

### Analyse rider data by user type and weekday

```r
all_trips %>%
+   mutate(weekday = wday(started_at)) %>% # Weekday field created using wday()
+   group_by (usertype, weekday) %>% # Usertype and weekday grouped
+   summarise(number_of_rides = n(), # Calculates number of rides 
+             average_duration = mean(ride_length)) %>% # Calculates the average duration
+  arrange(usertype, weekday) # Sorts the data
```

****************************Console output****************************

```r
`summarise()` has grouped output by 'usertype'. You can override using the `.groups` argument.
# A tibble: 14 × 4
# Groups:   usertype [2]
   **usertype weekday number_of_rides average_duration**
   <chr>      <dbl>           <int>            <dbl>
 1 casual         1          280364            1563.
 2 casual         2          197346            1414.
 3 casual         3          182229            1231.
 4 casual         4          189679            1197.
 5 casual         5          211725            1243.
 6 casual         6          233947            1299.
 7 casual         7          347877            1551.
 8 member         1          291628             826.
 9 member         2          374390             712.
10 member         3          402895             701.
11 member         4          404541             705.
12 member         5          402490             716.
13 member         6          354495             728.
14 member         7          333745             839.
```

### Analysed data set

```r
all_trips_analysed <- all_trips %>%
+   group_by(usertype, weekday) %>%
+   summarise(number_of_rides = n(), average_duration = mean(ride_length), average_distance = mean(ride_distance)) %>%
+  arrange(usertype, weekday)
```

****************************Console output****************************

```r
`summarise()` has grouped output by 'usertype'. You can override using the `.groups` argument.
> as_tibble(all_trips_analysed)
# A tibble: 14 × 5
   **usertype weekday   number_of_rides average_duration average_distance**
   <chr>    <ord>               <int>            <dbl>            <dbl>
 1 casual   Monday             197346            1414.            2258.
 2 casual   Tuesday            182229            1231.            2227.
 3 casual   Wednesday          189679            1197.            2227.
 4 casual   Thursday           211725            1243.            2257.
 5 casual   Friday             233947            1299.            2273.
 6 casual   Saturday           347877            1551.            2463.
 7 casual   Sunday             280364            1563.            2418.
 8 member   Monday             374390             712.            2067.
 9 member   Tuesday            402895             701.            2076.
10 member   Wednesday          404541             705.            2091.
11 member   Thursday           402490             716.            2096.
12 member   Friday             354495             728.            2066.
13 member   Saturday           333745             839.            2231.
14 member   Sunday             291628             826.            2185.
```

This data set provides valuable insight whereby member riders ride more on the weekdays, specifically on Wednesday. Whilst casual riders prefer to ride mostly during the weekends.

## Share

This section is all about the information that has been discovered and to discuss the findings to help the stakeholders make an accurate informed decision. To visualise this, Tableau was used to create the diagrams and charts below. The dashboard can be viewed [here](https://public.tableau.com/app/profile/jakiya/viz/CyclistBikeShare_16749151981160/CyclistBikeShare).

### User type distribution
![cyclist1](/cyclist1.png "User Distribution")

- Over 50% of Cyclistic’s usertype are member riders
- Significant amount of customer loyalty

### Bike preference
![cyclist2](/cyclist2.png "Bike preference")

- Classic bikes are significantly dominated by member riders
- Docked bikes used by casual riders
- Similar usage of electric bikes by both user types
- Casual riders prefer classic bikes more

### Popular riding days for both user types
![cyclist3](/cyclist3.png "Popular riding days for both user types")

- Member riders ride more on weekdays, notably peaking on Tuesdays
- Casual riders seen to be riding more on weekends
- Easily inferred that member riders use bikes for work whilst casual riders are for more recreational and occasional purposes

### Popular riding months for both user types
![cyclist4](/cyclist4.png "Popular riding months for both user types")

- Bike usage increases after February and drops after July for casual riders
- Member riders are more consistent with their riders throughout the year
- Less frequent bike use for causal riders during winter months

### Average ride length by weekday
![cyclist5](/cyclist5.png "Average ride length by weekday")

- Casual riders ride almost twice as much more than member riders
- Saturday and Sunday have the highest ride length for both user types, particularly for casual riders
- Lower ride lengths by member riders due to work commute

### Total number of rides per hour of the day
![cyclist6](/cyclist6.png "Total number of rides per hour of the day")

- Members riding hours usually peak at 7am - 9am and 5pm
- Casual riding hours gradually increases as the day goes on and dips after 5pm

## Act

This section will cover the recommendations based on the discovered information and findings to the stakeholders. In order to be able to convert Casual riders into Cyclistic members:

- Increase marketing campaigns that are targeted at casual riders through in app notifications, texts and emails with exclusive member rider sign up discounts and benefits. These advertisements would then encourage switching.
- Increase single day and full-day pass prices to incentivise member rider sign-ups. With this being done, discounted member rider prices may attract casual riders to convert.
- Build a rewards system whereby points are added to member riders accounts each time they ride a certain distance. With increased ride frequency, points can be collected  and then be exchanged to get a discount for the yearly subscription.
- Social media platforms should be utilised in order to cover all of the benefits member rider memberships have to offer.
- Offer a seasonal membership that allows people to ride bikes during peak seasons such as spring and summer. This membership would allow riders to have a sense of choice with no long term commitment for seasons where they would not ride. This membership can be priced strategically so that it is better to purchase the membership rather than paying for single day and full day passes for a year.

No further recommendations were made due to a lack in personally identifiable information, such as age, gender, payment and locational information. For Cyclistic to further meet its financial and marketing goals, in the foreseeable future it can focus on customising advertisements for their members based on hobbies, interests and ride usage.