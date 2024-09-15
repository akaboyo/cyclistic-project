# CYCLISTIC BIKESHARE CAPSTONE PROJECT

## Introduction

This is a capstone project as a part of my [Google Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-data-analytics) course.
For the analysis, I will be using R programming language and [RStudio](www.rstudio.com) IDE because it is easy for statistical analysis and data visualizations.

This report aims to analyze the usage patterns of annual members and casual riders of Cyclistic, a bike-share company in Chicago. The objective is to provide insights to the marketing analyst team to design a new marketing strategy to convert casual riders into annual members. The data used for this analysis is the Cyclistic historical bike trip data.

For this project, the following data analysis steps were followed :

 * Ask
 * Prepare
 * Process
 * Analyze 
 * Share
 * Act

**Data Collection**

The Cyclistic historical bike trip data includes information about bike rides taken by customers between 2016 and 2020. The dataset contains over 5.7 million observations and 15 variables. The variables include start and end stations, ride duration, user type, and bike id. The data was downloaded from the Cyclistic website and cleaned using Microsoft Excel and R programming language.

**Data Cleaning**
Before analyzing the data, I cleaned and preprocessed the dataset to remove any missing values, duplicates, or errors. I also transformed some variables into more meaningful formats and performed feature engineering to create new variables that could better predict usage patterns.

**Data Exploration**
I conducted exploratory data analysis (EDA) to identify patterns, trends, and relationships between variables. I used various graphical and statistical techniques to visualize the data and gain insights into the factors that influence usage patterns. I found that annual members take longer rides on average than casual riders and they tend to use the service more frequently.

**Data Analysis**
I conducted statistical analysis to identify significant differences between annual members and casual riders. The analysis revealed that annual members are more likely to ride during rush hour and on weekdays, while casual riders tend to ride on weekends and during the day.



 
## Scenario

You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.


## Ask

Three questions will guide the future marketing program:
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

The director of marketing and your manager *Lily Moreno* has assigned you the first question to answer: *How do annual members and casual riders use Cyclistic bikes differently?*

#### Key tasks

- [x] Identify the business task

    * The main objective is to build the best marketing strategies to turn casual bike riders into annual members by analyzing how the 'Casual' and 'Annual' customers use Cyclistic bike share differently.
    
    
- [x] Consider key stakeholders

    * Cyclistic executive team, Director of Marketing (Lily Moreno), Marketing Analytics team.
    
    
#### Deliverable

- [x] A clear statement of the business task

    * Find the differences between casual and member riders.
    
    
## Prepare

I will use Cyclistic’s historical trip data to analyze and identify trends. The data has been made available by
**Motivate International Inc.** under this [license](https://www.divvybikes.com/data-license-agreement).
Datasets are avilable [here](https://divvy-tripdata.s3.amazonaws.com/index.html).

#### Key tasks

- [x] Download data and store it appropriately.

    * Data has been downloaded from [here](https://divvy-tripdata.s3.amazonaws.com/index.html) and copies have been stored securely on my computer and here on Kaggle.
    
    
- [x] Identify how it’s organized.

    * All trip data are in comma-delimited (.CSV) format. Column names "ride_id", "rideable_type", "started_at", "ended_at", "start_station_name", "start_station_id", "end_station_name", "end_station_id", "start_lat", "start_lng", "end_lat", "end_lng", "member_casual" (Total 13 column)
    
    
- [x] Sort and filter the data.

    * For this analysis I am going to use the last 7 months data of the year 2021 and the first 5 months of year 2022 (June 2021 - May 2022), as it is the more current period to the business task.
    
    
- [x] Determine the credibility of the data.

    * For the purposes of this case study, the datasets are appropriate and it will enable me to answer the business questions. But due data-privacy I cannot use rider's personally identification information, and this will prevent me from determining if a single user/rider taken several rides. All ride ids are unique in this data-set.


#### Deliverable

- [x] A description of all data sources used
 
    * Main source of data provided by the [Cylistic company](https://divvy-tripdata.s3.amazonaws.com/index.html).
    
    

