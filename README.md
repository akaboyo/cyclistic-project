![cyclistic-image](https://github.com/user-attachments/assets/48f559e5-3e92-4605-8aed-bcaa8d4100b8)

# [CYCLISTIC BIKESHARE CAPSTONE PROJECT](#cyclistic-bikeshare-capstone-project)

## Table of Contents
- [Introduction](#introduction)

- [Business Task](#business-task)

- [Data Collection](#data-collection)

- [Data Exploration](#data-exploration)

- [Data Preparation](#data-preparation)

- [Process](#process)

- [Analyse](#analyse)
  
- [Recommendations](#recommendations)


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
  
## Data Preparation 
Data cleaning was performed to:
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

