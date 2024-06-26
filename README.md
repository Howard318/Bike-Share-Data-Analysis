# Bike-Share-Data-Analysis
For Coursera DA Capstone Project - Bike-Share Data Analysis

## Intro
Welcome to the Cyclistic bike-share analysis case study! This project showcases my work as a junior data analyst for Cyclistic, a fictional bike-share company based in Chicago. The case study is part of my professional portfolio and demonstrates my proficiency in data analysis, visualization, and strategic marketing insights.

## Table of Contents
- [Company Overview](#company-overview)
- [Project Scenario](#project-scenario)
- [Objectives](#objectives)
- [Tools](#tools)
- [MySql Code](#mysql-code)
- [Analysis and Report](#analysis-and-report)
	- [Overview](#overview)
	- [Total Ride](#total-ride)
	- [Average Ride Time on Weekdays](#average-ride-time-on-weekdays)
	- [User Count Weekdays](#user-count-weekdays)
	- [Hourly Ride Count Distribution](#hourly-ride-count-distribution)
	- [Weather Condition vs. Number of Rides](#weather-condition-vs-number-of-rides)
	- [Distribution Map](#distribution-map)
 	- [Dash Board](#dash-board)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [Conclusion](#conclusion)

## Company Overview
Cyclistic, launched in 2016, has quickly become a prominent bike-share service in Chicago, offering a fleet of over 5,800 bicycles and 600 docking stations. The program distinguishes itself by providing not only traditional two-wheeled bikes but also inclusive options like reclining bikes, hand tricycles, and cargo bikes to cater to riders with diverse needs. Cyclistic aims to be both a convenient commuting option and a leisure activity provider, with about 30% of users commuting daily.

## Project Scenario
As a member of Cyclistic's marketing analytics team, I was tasked with exploring how different customer segments—casual riders and annual members—use our bikes. This analysis aimed to uncover insights that could help convert casual riders into annual members, aligning with the company's goal to boost long-term membership and revenue.

## Objectives
The project was driven by three primary questions:
- How do annual members and casual riders use Cyclistic bikes differently?
- Why would casual riders buy Cyclistic annual memberships?
- How can Cyclistic use digital media to influence casual riders to become members?

## Tools
- MySql - Data cleaning and manipulating
  - Original Data [here](https://drive.google.com/drive/folders/13K9IWhWYIgR-kPKEcjKaGCZuvCY1xkVN?usp=sharing)
- Tableau - Data visulation
  - See my dashboard [here](https://public.tableau.com/views/CourseaBikeCase/FinalDashboard?:language=en-US&publish=yes&:sid=&:display_count=n&:origin=viz_share_link)

## MySQL Code

Creating a table that will contain all 12 months of Share Bike data. 
```sql
Create Table `BikeData_Combine_Tablaeu`
Like `2023 04 - bike data`;
```
Inserting all 12 months of data into the table for consolidation.
```sql
INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2023 04 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2023 05 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2023 06 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2023 07 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2023 08 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2023 09 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2023 10 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2023 11 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2023 12 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2024 01 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2024 02 - bike data`;

INSERT `BikeData_Combine_Tablaeu`
SELECT *
FROM `2024 03 - bike data`;
```
Removing unwanted/unnecessary columns
```sql
ALTER TABLE `BikeData_Combine_Tablaeu`
	DROP COLUMN start_station_name, 
	DROP COLUMN start_station_id,
    DROP COLUMN end_station_name,
	DROP COLUMN end_station_id;
```
Create another table to check if there are duplicates. If Checking returns 2 or more, it means identical rows need to be removed.   
```sql
CREATE TABLE `BikeData_Combine_Tablaeu2` (
  `ride_id` text,
  `rideable_type` text,
  `started_at` text,
  `ended_at` text,
  `start_lat` double DEFAULT NULL,
  `start_lng` double DEFAULT NULL,
  `end_lat` double DEFAULT NULL,
  `end_lng` double DEFAULT NULL,
  `member_casual` text
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

ALTER TABLE `BikeData_Combine_Tablaeu2`
Add Column Checking date AFTER `member_casual`;

ALTER TABLE `BikeData_Combine_Tablaeu2` MODIFY checking INTEGER;

INSERT `BikeData_Combine_Tablaeu2`
SELECT *, 
ROW_NUMBER() OVER(
PARTITION BY ride_id, rideable_type, started_at, ended_at, start_lat, start_lng, end_lat, end_lng, member_casual)
AS Checking 
FROM `BikeData_Combine_Tablaeu`;

Select *
From BikeData_Combine_Tablaeu2
Where Checking >1 or Checking is null;
```
## Analysis and Report
### Table of Content
- [Overview](#overview)
- [Total Ride](#total-ride)
- [Average Ride Time in Weekdays](#average-ride-time-in-weekdays)
- [User Count Weekdays](#user-count-weekdays)
- [Hourly Ride Count Distribution](#hourly-ride-count-distribution)
- [Weather Condition vs. Number of Rides](#weather-condition-vs-number-of-rides)
- [Distribution Map](#distribution-map)

### Overview
This report provides a detailed analysis of the bike-share usage patterns from April 2023 to March 2024, based on the data presented in the dashboard. A total of 5.60 million rides were taken during this period, with members significantly outpacing casual riders. The following sections summarize the key insights from the dashboard.

### Total Ride
Total Rides: 5.60 million  
Members: 64.11% (3.59 million rides)  
Casual Riders: 35.89% (2.01 million rides)  
<img width="1509" alt="Screenshot 2024-06-18 at 11 18 27 AM" src="https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/fe8644af-2901-4f3a-a88b-3cb4e13f2990">

### Average Ride Time on Weekdays
Casual Riders: Longest on Sunday (24.9 mins), shortest on Wednesday (18.3 mins)  
Members: Longest on Saturday (13.9 mins), shortest on Thursday (12.0 mins)  
![Avg Duration Weekday](https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/0cc35a69-8edd-4120-8a91-89a9f93cd124)

### User Count Weekdays
Casual Riders: Most on Saturday (406.18k), least on Monday (229.76k)  
Members: Most on Wednesday (586.89k), least on Sunday (397.19k)  
![User Count Weekday Dash](https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/83a3e29a-565b-4c4b-a54d-115811c40b0b)

## Hourly Ride Count Distribution
Peak Times: Morning and evening rush hours, more pronounced for members.  
![Hourly Distribution](https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/1af812a0-f44d-4e26-a826-95d8dd1e3fdf)

### Weather Condition vs. Number of Rides
Seasonal Trend: More rides in warmer months (June-September).  
Inverse Relationship: Fewer rides as snow depth increases, especially in winter.  
![Snow vs ride](https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/803857d2-df62-47db-a37a-f44d49f255bf)
![Temp vs Ride](https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/0d19afc9-2347-4163-97c4-b6892fc3aa25)

### Distribution Map
An overview of the distribution map
![Map (2)](https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/6c398666-25aa-479a-bedd-db7b024557c9)
Casual riders (blue) are more concentrated between sightseeing locations 
![Map](https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/4e242758-82ee-4ff3-a88d-7fbd60176231)
Members (orange) are more concentrated in local residential areas.
![Map (1)](https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/eb753ddc-fbf5-41ab-99f2-756dc8ee5cc3)

Please click [here](https://public.tableau.com/views/CourseaBikeCase/Map?:language=en-US&publish=yes&:sid=&:display_count=n&:origin=viz_share_link) to interact with the map on Tableau.

### Dash Board
An interactive dashboard can be accessed [here](https://howard318.github.io/bike_share_tableau_dashboard/)
![Final Dashboard (1)](https://github.com/Howard318/Bike-Share-Data-Analysis/assets/38737417/6eed7214-5c99-4f0f-9a91-b0a3a81c7edd)

## Key Findings

The analysis revealed distinct usage patterns between casual riders and annual members. For instance, casual riders tend to take longer rides during weekends and holidays, while annual members often use the bikes for shorter, routine commutes. These insights suggested targeted marketing strategies to appeal to casual riders' preferences and lifestyles.

## Recommendations
Based on the findings, I proposed the following strategies to increase annual memberships:

- Develop weekend and holiday promotions to attract casual riders.
  - Such as providing exclusive discounted weekend passes for members so the casual user will sign up for membership and potentially become a returning customer.
- Highlight the convenience and cost savings between member users and casual users.
- Utilize digital media campaigns to emphasize the exclusive benefits and flexibility of being a member user.

## Limitations 
As one month's worth of data already exceeds the maximum rows that both Excel and Google Sheets can handle, I chose MySQL to merge all 12 months of data and clean the data.

## Conclusion
This project underscores the importance of data-driven decision-making in marketing strategy. By understanding user behavior and preferences, Cyclistic can effectively tailor its marketing efforts to convert more casual riders into loyal annual members. This case study not only highlights my analytical skills but also my ability to translate data insights into practical business recommendations.






