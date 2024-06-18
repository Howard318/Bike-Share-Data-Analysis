# Bike-Share-Data-Analysis
For Coursea DA Capstone Project - Bike-Share Data Analysis

## Intro
Welcome to the Cyclistic bike-share analysis case study! This project showcases my work as a junior data analyst for Cyclistic, a fictional bike-share company based in Chicago. The case study is part of my professional portfolio and demonstrates my proficiency in data analysis, visualization, and strategic marketing insights.

## Table of Contents
- [Company Overview](#company-overview)
- [Project Scenario](#project-scenario)
- [Objectives](#objectives)
- [Tools](#tools)
- [MySql Code](#mysql-code)
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
Insterting all 12 months of data into the table for consolidate.
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
Removing unwanted / unneccessary columns
```sql
ALTER TABLE `BikeData_Combine_Tablaeu`
	DROP COLUMN start_station_name, 
	DROP COLUMN start_station_id,
    DROP COLUMN end_station_name,
	DROP COLUMN end_station_id;
```
Create another other table for checking if there are duplicates. If Checking returns 2 or more means there are identical rows and need to be remove.   
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



## Key Findings
The analysis revealed distinct usage patterns between casual riders and annual members. For instance, casual riders tend to take longer rides during weekends and holidays, while annual members often use the bikes for shorter, routine commutes. These insights suggested targeted marketing strategies to appeal to casual riders' preferences and lifestyle.

## Recommendations
Based on the findings, I proposed the following strategies to increase annual memberships:

- Develop weekend and holiday promotions to attract casual riders.
- Highlight the convenience and cost savings of annual memberships for regular users.
- Utilize digital media campaigns to emphasize the exclusive benefits and flexibility of being an annual member.

## Limitations 
As one month's worth of data already exceeds the maximum rows that both Excel and Google Sheets can handle, I chose MySQL to merge all 12 months of data and clean the data.




## Conclusion
This project underscores the importance of data-driven decision-making in marketing strategy. By understanding user behavior and preferences, Cyclistic can effectively tailor its marketing efforts to convert more casual riders into loyal annual members. This case study not only highlights my analytical skills but also my ability to translate data insights into practical business recommendations.






