# Video Game Sales 1976-2017 Analysis with Bigquery and Visualizations in Tableau :video_game:
## The dataset is available on Kaggle through [this link](https://www.kaggle.com/code/kendallgillies/video-game-sales-by-genre). It contains Video games, platforms, release years, and sales. I cleaned and analyzed the data in Bigquery, and I made visualizations in Tableau Public.
### Let's begin by exploring the data.
- We have 17416 rows of data, that includes:
  - Name of the video game
  - Platform (Gaming Console)
  - Release Year
  - Genre
  - Plublisher
  - North America Sales
  - Europe Sales
  - Japan Sales
  - Other Sales
  - Global Sales

```SQL
SELECT * 
FROM  `videogames-363720.VG_2017.vg_2017`
```

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture13.png)


#### Now, lets check for duplicate games with the same name and platform:

```SQL
SELECT Name, Platform, Count(*)
FROM `videogames-363720.VG_2017.vg_2017`
Group By Name, Platform
Having Count(*) > 1
```

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-2.png)

- Great, there are no duplicate Names and Platforms

#### Now let's look at the values with no release year

```SQL
SELECT * 
FROM `videogames-363720.VG_2017.vg_2017`
Where Year_of_release is Null
```
![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture14.png)

- There are 8 values with no release year. However, there is a year at the end of the Name column that we use to fill in the nulls. Lets write a query that extracts the last 4 digits out of the name for these rows and populate it into the Year_of_Release column.

```SQL
SELECT Name, 
cast(array_reverse(split(Name, ' '))[offset(0)] as int64) , Year_of_release
FROM `videogames-363720.VG_2017.vg_2017`
WHERE Year_of_release is null    
````
![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture15.png)

- Great, the array_reverse function worked and extracted the year from the Name and put it in a new column. Lets update the table with these new values:

```SQL
UPDATE `videogames-363720.VG_2017.vg_2017`
SET Year_of_release =  cast(array_reverse(split(Name, ' '))[offset(0)] as int64)
WHERE Year_of_release is null
```
![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-6.png)

- We have populated the 8 rows with no release year and now there aren’t any null values in that column.

## Let's begin the Analysis

- Lets look at the count of games released each year:

```SQL
SELECT Year_of_Release, Count(Name) as total_games_by_year  
FROM `videogames-363720.VG_2017.vg_2017`
Group By Year_of_Release
Order by Year_of_Release 
```
![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture16.png)

- The largest amount of games were released between 2006 and 2011 (more than 1,000 games were released each year!)

- Now, for each year between 1976-2017 let's calcuate the percent of games released in each year:

```SQL
With t1 as (
  SELECT 
    Distinct Year_of_Release, 
    Count(Name) OVER(PARTITION BY Year_of_Release) as Games_per_year, 
    Count(Name) OVER () AS Total_Count, 
  FROM 
    `videogames-363720.VG_2017.vg_2017`
)
SELECT 
  Distinct Year_of_Release, 
  (Games_per_year / Total_Count)* 100 as yearly_percent_games, 
FROM 
  t1 
Order By
  Year_of_Release
```
![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-8.png)


- Let's also calculate the cumulative percent of games released between 1976-2017. I will put this into a graph in tableau:

```SQL
With t1 as (
  SELECT 
    Distinct Year_of_Release, 
    Count(Name) OVER(PARTITION BY Year_of_Release) as Games_per_year, 
    Count(Name) OVER () AS Total_Count, 
  FROM 
    `videogames-363720.VG_2017.vg_2017`
),
t2 as(
SELECT 
  Distinct Year_of_Release, 
  (Games_per_year / Total_Count)* 100 as yearly_percent_games, 
FROM 
  t1 
)
SELECT Year_of_release,
        sum(yearly_percent_games) OVER(Order By Year_of_Release) as cumulative_percnt,
FROM t2
```
![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-9.png)

- From the data  we can see that the total number of games released from the first half of the time period (1976-1996) is 5.7%. This makes sense as consoles became more popular in the early 2000's.

- Now let's look at global sales of video games per year

```SQL
SELECT Year_of_Release, Sum(Global_Sales) as Global_Sales
FROM `videogames-363720.VG_2017.vg_2017`
GROUP By Year_of_Release
Order by Year_of_Release
```
![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-10.png)

- Global sales for all platforms peaked in 2008 when it reached $685 million.

- Now let's look at global sales by platform

```SQL
SELECT Platform, Sum(Global_Sales) as Global_Sales
FROM `videogames-363720.VG_2017.vg_2017`
GROUP By Platform
Order by Global_Sales desc
```
![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-11.png)

- Total Sales for PS2 was the greatest at $1.251 Billion, followed by Xbox360 ($967 million), and PS3 ($934 million).

- Next let's calculate the lifespan (the latest year of realease - the first year the platform released a game)

```SQL
SELECT Platform, MAX(Year_of_Release) - MIN(Year_of_Release) as lifespan
FROM `videogames-363720.VG_2017.vg_2017`
GROUP BY Platform
```
![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-12.png)

- PC has the largested life span at 31 years!


## Visualizations

### Using Tableau Public I created visulizations with the data

![](https://github.com/cdauksas/PortfolioProjects/blob/main/VGTableau.PNG)
