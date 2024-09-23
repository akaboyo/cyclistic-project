![cyclistic-image](https://github.com/user-attachments/assets/48f559e5-3e92-4605-8aed-bcaa8d4100b8)


# [CYCLISTIC BIKESHARE CAPSTONE PROJECT](#cyclistic-bikeshare-capstone-project)

## Introduction

This is a capstone project as a part of my [Google Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-data-analytics) course.
For the analysis, I will be using R programming language and [RStudio](www.rstudio.com) IDE because it is easy for statistical analysis and data visualizations.

Cyclistic is a bike-share company offering riders flexible transportation across the city. The company’s current model allows users to rent bikes on a casual or membership basis. The goal of this project is to analyze historical trip data to understand how annual members and casual riders use Cyclistic bikes differently. The insights gained will inform strategies to convert casual riders into long-term, annual members, as this would increase profitability and retention.

## Business Task
The key objective is to determine how Cyclistic can increase the number of annual members by understanding usage patterns between casual riders and annual members. By identifying the differences in behavior, we can propose marketing strategies aimed at converting casual riders into annual members.

## Data Collection
The Cyclistic historical bike trip data includes information about bike rides taken by customers between 2016 and 2020. The dataset contains over 5.7 million observations and 15 variables. The variables include start and end stations, ride duration, user type, and bike id. The data was downloaded from the Cyclistic website and cleaned using Microsoft Excel and R programming language.

## Data Exploration
- Data Source: Cyclistic provides trip data from June 2021 to May 2022. The dataset includes details such as:
- Ride ID
- Start and end times
- Trip duration
- Start and end station
- Bike type (electric or classic)
- User type (annual member or casual rider)
  
### Data Preparation: Data cleaning was performed to:
- Remove duplicates and missing values.
- Ensure accurate trip times (excluding negative durations).
- Filter out incomplete data points and null values.
- Create new variables to extract time-based features (e.g., day of the week, month).
  
### Key Data Questions:
- How do annual members and casual riders use Cyclistic bikes differently?
-  Why would casual riders buy Cyclistic annual memberships?
- How can Cyclistic use digital media to influence casual riders to become members?

## Process
The following steps were followed during the Data Processing stage :

### Install and load necessary packages
I imported the follwing R packages for the analysis
- library(tidyverse)
- library(lubridate)
- library(janitor)
- library(ggmap)
- library(geosphere)

### Import data into R studio
- df1 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-june2021.csv")
- df2 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-july2021.csv")
- df3 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-aug2021.csv")
- df4 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-sept2021.csv")
- df5 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-oct2021.csv")
- df6 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-nov2021.csv")
- df7 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-dec2021.csv")
- df8 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-jan2022.csv")
- df9 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-feb2022.csv")
- df10 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-march2022.csv")
- df11 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-apr2022.csv")
- df12 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-may2022.csv")

### Combine the individual monthly datasets into one large dataframe
bikeshare <- rbind(df1, df2, df3, df4, df5, df6, df7, df8, df9, df10, df11, df12)

### Converting Date/Time stamps to Date/Time format
bikeshare_clean$started_at <- lubridate::ymd_hms(bikeshare_clean$started_at)

bikeshare_clean$ended_at <- lubridate::ymd_hms(bikeshare_clean$ended_at)

### Parsing start and end hour
bikeshare_clean$start_hour <- lubridate::hour(bikeshare_clean$started_at)

bikeshare_clean$end_hour <- lubridate::hour(bikeshare_clean$ended_at)

## Analyse
The analysis was structured around identifying behavioral differences between the two rider types:

### Compare members and casual users
Analyze the differences between Member and casual riders in terms of total rides taken.

### Members vs Casual riders difference in respect of total rides taken
bikeshare_clean %>% 

    group_by(member_casual) %>% 
    
    summarise(ride_count = length(ride_id), "%" = (length(ride_id) / nrow(bikeshare_clean)) * 100)

ggplot(bikeshare_clean, aes(x = member_casual, fill=member_casual)) +

    geom_bar() +
    
    labs(x="Customer Type", y="Number Of Rides", title= " Members vs Casuals distribution")
    






### [View Project Here](https://www.kaggle.com/code/adebayoadebanjo/my-google-cyclistic-capstone)

