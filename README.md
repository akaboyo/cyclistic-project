![cyclistic-image](https://github.com/user-attachments/assets/48f559e5-3e92-4605-8aed-bcaa8d4100b8)

# [CYCLISTIC BIKESHARE PROJECT](#cyclistic-bikeshare-project)

## Table of Contents
- [Executive Summary](#executive-summary)
  
- [Introduction](#introduction)

- [Business Task](#business-task)

- [Data Sources](#data-sources)

- [Data Preparation](#data-preparation)

- [Process](#process)

- [Analyse](#analyse)
  
- [Recommendations](#recommendations)

## Executive Summary

The Cyclistic Bike-Share Project is a data analysis case study focused on understanding patterns in bike usage between annual members and casual riders. Using historical ridership data spanning multiple months, the analysis merges and cleans large trip datasets, engineer key time-based features, and applies exploratory techniques to compare user behaviours across time, ride type, and usage frequency.

The project answers strategic questions such as how ride frequency and duration differ between membership types, which periods of the year see the highest usage, and what behavioural trends might inform targeted marketing strategies. Visualizations in the repository — including distribution charts, trend plots, and comparative graphs — highlight clear differences in behaviour between user segments. These insights support recommendations that Cyclistic could use to tailor promotions, optimize service offerings, and ultimately increase annual memberships

## Introduction

Cyclistic is a bike-sharing program where riders can use bicycles through either a casual (pay-as-you-go) model or an annual membership. Understanding how these two groups differ in their usage patterns can help Cyclistic tailor marketing, service planning, and customer retention strategies.

## Business Task
The main objective of this analysis is:

- To understand how *annual members* and *casual riders* use Cyclistic bikes differently.
- To identify patterns that might explain why casual riders do not convert to annual membership.
- To provide actionable insights and recommendations that could support marketing and service decisions.

## Data Sources

The dataset includes historical trip records with the following types of information:

- Ride IDs
- Start and end timestamps
- Bike type
- Rider type (member vs casual)
- Duration and derived ride statistics

Data was collected across multiple CSV files covering a 12-month period and consolidated for analysis.

## Data Preparation 
Data cleaning and preparation included:

- Merging multiple monthly CSV files into a single dataset.
- Handling missing values and duplicates.
- Ensuring date/time fields are correctly formatted.
- Creating new features such as *ride duration*, *day of week*, *month*, and *hour of day* for deeper analysis.
  
### Key Data Questions:
- How do annual members and casual riders use Cyclistic bikes differently?
-  Why would casual riders buy Cyclistic annual memberships?
- How can Cyclistic use digital media to influence casual riders to become members?

## Process
The following steps were followed during the Data Processing stage :

### Install and load necessary packages
I imported the follwing R packages for the analysis

library(tidyverse)

library(lubridate)

library(janitor)
  
library(ggmap)
  
library(geosphere)

### Import data into R studio
df1 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-june2021.csv")
  
df2 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-july2021.csv")

df3 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-aug2021.csv")

df4 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-sept2021.csv")

df5 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-oct2021.csv")

df6 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-nov2021.csv")

df7 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-dec2021.csv")

df8 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-jan2022.csv")

df9 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-feb2022.csv")

df10 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-march2022.csv")

df11 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-apr2022.csv")

df12 <- read_csv("/kaggle/input/cyclistic-bikeshare/cyclistic-data-may2022.csv")

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

![chart1](https://github.com/user-attachments/assets/441fbd0f-fe0a-418b-bdcb-9c25b1466d7d)

### KEY INSIGHT
From the Members vs Casuals distribution chart above, members are ~57% while casual riders are ~43% of the total riders for the twelve months under review. Members ride share is ~14% more than that of casual riders.

### Distribution of total rides data by rider-type and day of week

### Setting the order of the days of the week

bikeshare_clean$day_of_week <- ordered(bikeshare_clean$day_of_week,
                                      
                                    levels=c( "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"))

bikeshare_clean %>% 

  group_by(member_casual, day_of_week) %>%  

  summarise(number_of_rides = n() 
            
  ,.groups="drop") %>% 

  arrange(member_casual, day_of_week)

![chart2](https://github.com/user-attachments/assets/caaa31c6-117f-41f8-a9fa-c9aa9f6d13d9)

### KEY INSIGHT
The chart above shows that both members and casual riders took averagely consistent trips throughout the week. However, it is obvious that more of the members use bikes more than the casual riders for each day of the week. This suggests that the members are daily local commuters who rely heavily on bikes. On the other hand, casual riders appear to use bikes more on sunday indicating recreational or leisure patterns.

### Visualize total rides data by rider-type and month

bikeshare_clean %>%  

  group_by(member_casual, month) %>% 
  
  summarise(number_of_rides = n(),.groups="drop") %>% 
  
  arrange(member_casual, month)  %>% 
  
  ggplot(aes(x = month, y = number_of_rides, fill = member_casual)) +
  
  labs(title ="Total rides by Members and Casual riders Vs. Month", x = "Month", y= "Number Of Rides") +
  
  theme(axis.text.x = element_text(angle = 45)) +
  
  geom_col(width=0.5, position = position_dodge(width=0.5)) +
  
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))

  ![chart3](https://github.com/user-attachments/assets/8541a307-4775-45e5-bf38-03e17eaccb19)


### KEY INSIGHT
The summer months of May,June, July, August and September are the busiest time of the year for both members and casual riders, although members remained high up till October. It is possible that winter and inclement weather resulted in a significant drop in total rides in the months of November, December, January and February for both type of customers. But we can see that member's total rides are higher than casual riders throughout the year except from June, July and August. This may suggest that Casual riders engage more in recreational or leisurely activities than Members during summer season.

### Comparison of ride distance between Members and Casual riders

bikeshare_clean %>% 

  group_by(member_casual) %>% drop_na() %>%
  
  summarise(average_ride_distance = mean(ride_distance)) %>%
  
  ggplot() + 
  
  geom_col(mapping= aes(x= member_casual,y= average_ride_distance,fill=member_casual), show.legend = FALSE)+
  
  labs(title = "Average travel distance by Members and Casual riders", x="Member and Casual riders", y="Average distance In Km")

  ![chart4](https://github.com/user-attachments/assets/eb07721e-6fc2-4ffd-a676-1606596b4e50)

### KEY INSIGHT
The chart above shows that both rider-types traveled about the same average distance. This similarity could be possible due to the fact that members take same ride time throughout the week, but casual riders took rides mostly on weekends with more ride time.

### Analysis and visualization of Bike Preferences Vs. Total rides by Members and casual riders

bikeshare_clean %>%

    group_by(rideable_type) %>% 
    
    summarise(count = length(ride_id))

ggplot(bikeshare_clean, aes(x=rideable_type, fill=member_casual)) +

    labs(x="Rideable type", title="Rideable type Vs. total rides by Members and casual riders") +
    
    geom_bar()

![chart5](https://github.com/user-attachments/assets/b91b2a19-b5bc-4f36-94b1-d2535df841c9)

### KEY INSIGHTS
- From the above visualization, we see that both rider-types mostly use classic bikes, followed by electric bikes. Docked bikes are mostly used by casual riders.

- Classic bikes being more popular might indicate that riders, regardless of membership status, prefer a more traditional biking experience, possibly due to cost-effectiveness, availability, or habit.
- Casual and annual riders may not see the need for electric bikes unless it’s for specific use cases, like longer distances or faster commuting.
- Electric bikes are often more expensive to rent, so the preference for classic bikes may suggest that both casual riders and members are cost-conscious, opting for cheaper transportation, especially for shorter trips.
- This could indicate a pricing sensitivity among Cyclistic’s user base, meaning pricing strategies around electric bikes should be carefully structured to avoid alienating users.

## RECOMMENDATIONS

- IMPROVE USER EXPERIENCE: Offer a weekend-only membership at a discounted price other than the full annual membership subscription package.This might attract more Casual riders who use the bikes mostly for recreational activities.
- LOYALTY PROGRMAS: Coupons and discounts could be handed out along with the annual subscription / weekend-only membership for the usage of electric bikes targeting casual riders. This may be an area of growth for Cyclistic since this bike-type is already popular among both types of riders.
- TARGETED MARKETING CAMPAIGNS: Create marketing campaigns which can be sent via email,digital advertisement through local influncers on social media platforms like Instagram, Youtube, Tik-Tok to promote the Bikeshare and show its appeal to local residents in the docking stations, explaining why annual membership is beneficial. Campaigns should be placed during the peak months of the year.
- Cyclistic could take advantage of the clear preference for classic bikes while also working to enhance the appeal and affordability of electric bikes. Marketing campaigns might focus on promoting the benefits of electric bikes for specific rider types (e.g., long-distance commuters or casual riders looking for a more relaxed experience) to drive up their usage without disrupting the existing demand for classic bikes.
  
## LIMITATION
All ride ids are unique so we cannot conclude if the same rider takes several rides. More rider data is needed for further analysis

## Technologies Used

- R (for data cleaning and analysis)
- ggplot2 and tidyverse (for visualization and manipulation)
- Git & GitHub (for version control)
