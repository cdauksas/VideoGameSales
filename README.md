# Video Game Sales 1976-2017 Analysis with Bigquery and Tableau
## This dataset is available on Kaggle through this link. It contains Video games, platforms, release years, and their sales. I performed data cleaning and analysis in Google's Bigquery, and I made visualizations in Tableau Public.
- I began my exploring the data:
![](Photo1)

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
- Now, lets check for duplicate games with the same name and platform:

![](Photo2)

- There are 5 duplicate Names and Platforms that that we want removed from this data set. Lets use a window function to select all unique Names and Platforms:

![](Photo3)

- We have selected the unique names and platforms and we see the total results went down by 5 (from 17416 to 17411). Lets copy that into a new table

- Now let's look at the values with no release year
![](Photo4)

- There are 8 values with no release year. However, there is a year at the end of the Name column that we use to fill in the nulls. Lets write a query that extracts the last 4 digits out of the name for these rows and populate it into the Year_of_Release column.

![](Photo5)

- Great, the array_reverse function worked and extracted the year from the Name and put it in a new column. Lets update the table with these new values:

![](Photo6)

- We have populated the 8 rows with no release year and now there aren’t any null values in that column.

## Let's begin the Analysis
- Lets look at the count of games released each year:

![](Photo7)

- The largest amount of games were released between 2006 and 2011 (more than 1,000 games were released each year!)

- Now, for each year between 1976-2017 let's calcuate the percent of games released in each year:

![](Photo8)

- Let's also calculate the cumulative percent of games released between 1976-2017. I will put this into a graph in tableau:

![](Photo9)

- From the data (graph) we can see that the total number of games released from the first half of the time period (1976-2001) is about xx% which is small.

- Now let's look at global sales of video games per year

![](Photo10)

- Now let's look at global sales by platform

![](Photo11)

- Calculate the lifespan

![](Photo11)
