# GoogleData Analytics Capstone - Cyclistic 

This is my Google Data Analyst capstone project where I was tasked with providing insights from a dataset to help create a marketing campaign. In this project I used Excel for data cleaning where I organized the time format, deleted blank unusable rows, and found null values from improper calculations. Followed by using SQL to derive actionable insights that I then later visualized with Tableau.

#### Business Task
Design a new marketing strategy to convert casual riders into annual members

#### Ask
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

#### SQL
```
--Most popular bike type between members
SELECT member_casual, rideable_type, COUNT(rideable_type) AS count
FROM PortfolioProject..tempTable
GROUP BY member_casual, rideable_type
ORDER BY member_casual, COUNT(rideable_type) DESC
```

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/31958728-10c3-42b8-bf12-2dd2c0803fc5/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211121T181725Z&X-Amz-Expires=86400&X-Amz-Signature=2c9b98c3ee2a1858808e702c9f9c16a8ceff91ec0aae97a7390b3c2acb2ee077&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```
--Casual vs Annual Member: Most popular days for our riders?
SELECT
	member_casual,
	new_day =
	CASE day_of_week
		WHEN '1' THEN 'sunday'
		WHEN '2' THEN 'monday'
		WHEN '3' THEN 'tuesday'
		WHEN '4' THEN 'wednesday'
		WHEN '5' THEN 'thursday'
		WHEN '6' THEN 'friday'
		WHEN '7' THEN 'saturday'
		END,
  COUNT(day_of_week) AS count
FROM PortfolioProject..tempTable
GROUP BY day_of_week, member_casual
ORDER BY COUNT(day_of_week) DES
```

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4546831c-a399-4ddd-89cf-b06e42cff856/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211121T181700Z&X-Amz-Expires=86400&X-Amz-Signature=7f8aca009d2d80b6be7fbf31974d222bce09872c87e988ef3f83391bd87e9ac0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)


```
--Joined multiple talbes into one temp table
IF OBJECT_ID ('dbo.tempTable', 'U') IS NOT NULL  
DROP TABLE dbo.tempTable;  
GO  

SELECT ride_id, member_casual, rideable_type, start_station_name, day_of_week, ride_length
INTO dbo.tempTable
FROM PortfolioProject..[202009-divvy-tripdata] 
WHERE start_station_name IS NOT NULL
UNION
SELECT ride_id, member_casual, rideable_type, start_station_name, day_of_week, ride_length
FROM PortfolioProject..[202010-divvy-tripdata]
WHERE start_station_name IS NOT NULL
UNION
SELECT ride_id, member_casual, rideable_type, start_station_name, day_of_week, ride_length
FROM PortfolioProject..[202011-divvy-tripdata] 
WHERE start_station_name IS NOT NULL
UNION
SELECT ride_id, member_casual, rideable_type, start_station_name, day_of_week, ride_length
FROM PortfolioProject..[202012-divvy-tripdata]
WHERE start_station_name IS NOT NULL
```

```
--- Average ride time Members vs Casual
SELECT 
	member_casual, 
	cast(cast(avg(cast(CAST(ride_length as datetime) as float)) as datetime) as time) AvgTime
FROM PortfolioProject..tempTable
Group BY member_casual
```
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5558f020-3ac6-4c60-84c7-5c627c2d05ad/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211121T181755Z&X-Amz-Expires=86400&X-Amz-Signature=caada5aba3184e809c8568dd907a166339287c491e9090de741ee966c8e9b18e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### Tableau
![](./images/PieChart.png)
![](./images/stat2.JPG)
![](./images/PopularMap.png)
![](./images/PopularDays.png)
![](./images/PreferredBikes.png)
![](./images/HighAvgDays.png)
