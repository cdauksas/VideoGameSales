# Video Game Sales 1976-2017 Analysis with Bigquery and Tableau
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

///SQL
SELECT * 
FROM  `videogames-363720.VG_2017.vg_2017`
///

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture13.png)


- Now, lets check for duplicate games with the same name and platform:

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-2.png)

- Great, there are no duplicate Names and Platforms

- Now let's look at the values with no release year

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture14.png)

- There are 8 values with no release year. However, there is a year at the end of the Name column that we use to fill in the nulls. Lets write a query that extracts the last 4 digits out of the name for these rows and populate it into the Year_of_Release column.

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture15.png)

- Great, the array_reverse function worked and extracted the year from the Name and put it in a new column. Lets update the table with these new values:

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-6.png)

- We have populated the 8 rows with no release year and now there aren’t any null values in that column.

## Let's begin the Analysis

- Lets look at the count of games released each year:

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture16.png)

- The largest amount of games were released between 2006 and 2011 (more than 1,000 games were released each year!)

- Now, for each year between 1976-2017 let's calcuate the percent of games released in each year:

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-8.png)


- Let's also calculate the cumulative percent of games released between 1976-2017. I will put this into a graph in tableau:

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-9.png)

- From the data  we can see that the total number of games released from the first half of the time period (1976-1996) is 5.7%. This makes sense as consoles became more popular in the early 2000's.

- Now let's look at global sales of video games per year

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-10.png)

- Global sales for all platforms peaked in 2008 when it reached $685 million.

- Now let's look at global sales by platform

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-11.png)

- Total Sales for PS2 was the greatest at $1.251 Billion, followed by Xbox360 ($967 million), and PS3 ($934 million).

- Next let's calculate the lifespan (the latest year of realease - the first year the platform released a game)

![](https://github.com/cdauksas/PortfolioProjects/blob/main/Picture-12.png)

- PC has the largested life span at 31 years!


## Visualizations

### Using Tableau Public I created visulizations with the data

![](https://github.com/cdauksas/PortfolioProjects/blob/main/VGTableau.PNG)
