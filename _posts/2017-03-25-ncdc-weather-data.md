---
layout: post
title: "National Climatic Data Center Weather Data using SQL Server"
date: 2017-03-25
---
<p>The following analysis is conducted using historical weather data from 2000-2017 within the surrounding Boston area provided by the <a href="https://www.ncdc.noaa.gov/cdo-web/datasets">National Climatic Data Center</a>.&nbsp;I began by using the SQL Server Data Tools GUI to import the .csv files I downloaded from the NCDC servers into a table I called weather_table:</p>

The .csv files I used for this data set can be downloaded <a href="https://github.com/michaelip2/michaelip2.github.io/blob/master/_files/879227.csv">here</a>.

<pre><code>
Create table weather_table (
[STATION] varchar(50),
[STATION_NAME] varchar(50),
[WEATHER_DATE] date,
[MDPR] int,
[MDSF] int,
[DAPR] int,
[DASF] int,
[PRCP] int,
[SNWD] int,
[SNOW] int,
[TAVG] int,
[TMAX] int,
[TMIN] int,
[TOBS] int
);
</code></pre>

Upon reading the accompanying documentation for the weather data and inspecting the imported rows, I have noticed there were a substantial number of rows with missing data as denoted with the value -9999 since apparently not all the data were recorded. I decided to update all the -9999 values in these rows as null.

<pre><code>
Update weather_table set -- convert all -9999 values into NULLS
tavg = case when tavg = -9999 THEN NULL else tavg end, 
tmin = case when tmin = -9999 THEN NULL else tmin end, 
tmax = case when tmax = -9999 THEN NULL else tmax end;
</code></pre>

Sample Queries

1) Return past weather data from one and two years ago from today’s date:
<pre><code>
SELECT 
    station AS 'Station', station_name AS 'Station Name', CONVERT(varchar(8), weather_date, 1) AS 'Date', -- give past weather data from 1 and 2 years ago
CASE 
    WHEN (tavg IS NOT NULL and tmin IS NULL and tmax IS NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NOT NULL and tmin IS NULL and tmax IS NOT NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NOT NULL and tmin IS NOT NULL and tmax IS NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NOT NULL and tmin IS NOT NULL and tmax IS NOT NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NULL and tmin IS NOT NULL and tmax IS NOT NULL) THEN CAST(SUM((tmin+tmax)/2) AS varchar(50)) ELSE 'No data' END AS 'Average Temperature',
CASE 
    WHEN tmin IS NULL THEN 'No data' ELSE CAST(tmin AS varchar(50)) END AS 'Minimum Temperature', --CAST() isto bypass printing string restriction in a INT only column
CASE 
    WHEN tmax IS NULL  THEN 'No data' ELSE CAST(tmax AS varchar(50)) END AS 'Maximum Temperature', 
CASE 
    WHEN prcp IS NULL  THEN 'No data' ELSE CAST(prcp AS varchar(50)) END AS 'Precipitation', tavg, tmin, tmax
FROM 
    weather_table 
WHERE 
    weather_Date in (DATEADD(year,-1,CONVERT(datetime,CONVERT(nvarchar(11),GETDATE()))), DATEADD(year,-2,CONVERT(datetime,CONVERT(nvarchar(11),GETDATE()))))
GROUP BY 
    station_name, weather_date, tavg, tmin, tmax, station, prcp
ORDER BY 
    station_name, weather_date
</code></pre>

Output (105 rows):
<p><img src="https://michaelip2.github.io/images/2017-03-25 02_00_42-weather.sql - DESKTOP-BB2A4CL_SQLEXPRESS.weather_database (DESKTOP-BB2A4CL_Micha.png"/></p>

The case statement for Average Temperature, Minimum Temperature, and Maximum Temperature returns the string value in the event tavg, tmin, and tmax have null values but prioritizes to show numerical values if they exist. I included the tavg, tmin, and tmax fields in the tables is to give an accurate representation of what the data appeared after importing. Additionally, I decided to omit the Precipitation field from this point on because the rows mostly contained either 0 or null values.

2) Return the coldest day on average of a specified date:
<pre><code>
SELECT 
    station_name, station, CONVERT(VARCHAR(8), weather_date, 1) AS 'Date', tavg, tmin, tmax, -- coldest day on avg by date
CASE 
    WHEN (tavg IS NOT NULL and tmin IS NULL and tmax IS NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NOT NULL and tmin IS NULL and tmax IS NOT NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NOT NULL and tmin IS NOT NULL and tmax IS NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NOT NULL and tmin IS NOT NULL and tmax IS NOT NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NULL and tmin IS NOT NULL and tmax IS NOT NULL) THEN CAST(CAST(SUM((tmin+tmax)/2.0) AS decimal(16,2)) AS varchar(50)) ELSE 'no data' END AS 'Average Temperature',
CASE 
    WHEN tmin IS NULL THEN 'No data' ELSE CAST(tmin AS varchar(50)) END AS 'Minimum Temperature', --CAST() is to bypass printing string restriction in a INT only column
CASE 
    WHEN tmax IS NULL  THEN 'No data' ELSE CAST(tmax AS varchar(50)) END AS 'Maximum Temperature'
FROM 
    weather_table WHERE weather_date = '1/16/04' and ((tmin IS NOT NULL and tmax IS NOT NULL) or tavg IS NOT NULL) -- filter out rows that are null tavg or a null tmin and tmax
GROUP BY 
    station_name, station, tavg, tmin, tmax, weather_date  
ORDER BY 
    CASE WHEN (tavg IS NULL and tmin IS NOT NULL and tmax IS NOT NULL) THEN (SUM((tmin + tmax)/2.0)) ELSE tavg END;
</code></pre>
Output (20 rows):
<p><img src="https://michaelip2.github.io/images/2017-03-25 02_58_48-weather.sql - DESKTOP-BB2A4CL_SQLEXPRESS.weather_database (DESKTOP-BB2A4CL_Micha.png"/></p>

Notice how the tavg column display null values since the column has no recorded data from the sourced weather data. I decided for the Average Temperature column to return the average of tmin and tmax in the event tavg is missing and use tmin and tmax as proxy variables to provide an accurate substitute for tavg values. In the event the tavg values are present, prioritize and return those values first. This is the what my case statement is trying to accomplish. 

3) Return the hottest day on average from last year:
<pre><code>
SELECT 
    TOP 1 station_name, station, weather_date, tavg, tmin, tmax,
CASE 
    WHEN (tavg IS NOT NULL and tmin IS NULL and tmax IS NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NOT NULL and tmin IS NULL and tmax IS NOT NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NOT NULL and tmin IS NOT NULL and tmax IS NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NOT NULL and tmin IS NOT NULL and tmax IS NOT NULL) THEN CAST(tavg AS varchar(50))
    WHEN (tavg IS NULL and tmin IS NOT NULL and tmax IS NOT NULL) THEN CAST(CAST(SUM((tmin+tmax)/2.0) AS decimal(16,2)) AS varchar(50)) END AS 'Average Temperature'
FROM 
    weather_table 
WHERE 
    station_name like 'Boston%' and WEATHER_DATE >= (DATEADD(dd,-365,CONVERT(datetime,CONVERT(nvarchar(11),GETDATE())))) 
GROUP BY 
    station_name, station, tavg, tmin, tmax, weather_date 
ORDER BY 
    'Average Temperature' DESC
</code></pre>
Output (1 row):
<p><img src="https://michaelip2.github.io/images/2017-03-22 22_31_15-weather.sql - DESKTOP-BB2A4CL_SQLEXPRESS.weather_database (DESKTOP-BB2A4CL_Micha.png"/></p>
