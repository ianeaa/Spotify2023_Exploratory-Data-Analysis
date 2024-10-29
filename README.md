# Spotify2023_Exploratory-Data-Analysis

## About the project
etcetcetc

## Objectives

1. Overview of Data set &emsp;&emsp;&emsp;&ensp;&nbsp; 5. Genre and Music Characteristics
2. Basic Descriptive Statistics &emsp;&nbsp;&nbsp; 6. Platform Popularity
3. Top Performers &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp; 7. Advance Analysis
4. Temporal Trends

| ____   |       ____        |
| ------------- | ------------- |
| 1. Overview of Dataset   | 5. Genre and Music Characteristics        |
| 2. Basic Descriptive Statistics   | 6. Platform Popularity      |
| 3. Top Performers   |  7. Advance Analysis |
| 4. Temporal Trends  |   |

## Codes
pm pag dito


## Results and Discussion
### 1. *Overview of the Dataset 👀*
- The number of rows and columns of the given data is 953 rows 24 columns
  
- Here are the datatypes of each column of the given data:
 
![datatypes](https://github.com/user-attachments/assets/b000984f-4128-4960-9da3-f2128d02eb6e)

  
- Are there any missing values? *(Checked using isnull)*

![nullvalues](https://github.com/user-attachments/assets/5d28f497-378d-469c-bc82-a947c7ffa0cb)

***Discussion:*** 
Based on the results above, the general overview of the data is that the shape of the dataframe is 953 rows and 24 columns having 7 columns classified as objects and 17 as int64. 
When checking for null values, it was found out that there were 50 in_shazam_charts and 95 in the key column

### 2. *Basic Descriptive Statistics 📈*
- Mean, Median, and Standard Deviation of the 'streams' column

  ![meanmedstdev](https://github.com/user-attachments/assets/d5756c26-89c7-4900-aecb-09704241c919)


- Distribution of the data of released_year and artist(s)_count
 
![distrib_artistsandreleasedyr](https://github.com/user-attachments/assets/941a68b2-4ece-4e99-8f3c-2f9ca60ca18e)

***Discussion:*** 
The shown results above for the mean, median, and standard deviation were done by the pandas .mean(), .median(), and .std() functions. 
After that, the three were also calculated in Microsoft Excel portraying the same results. (show proof ge subukan mo kumopya)
### *3. Top performers 🕺💃*

- Top 5 Most Streamed Tracks

| Track |      Artist(s)      |
| ------------- | ------------- |
| 1. Blinding Lights  | The Weeknd       |
| 2. Shape of You   | Ed Sheeran     |
| 3. Someone You Loved   |  Lewis Capaldi |
| 4. Dance Monkey  |  Tones and I |
| 5. Sunflower - Spider-Man: Into the Spider-Verse  | Post Manole, Swae Lee  |
  
- Top 5 Artist(s) by their number of tracks

![top5artistsbytracks](https://github.com/user-attachments/assets/468216fa-25fb-4ff3-a0fe-c3d54d4633f9)


### *4. Temporal Trends 📊*
- Number of tracks released per year

  ![distrib_releasedyr](https://github.com/user-attachments/assets/ed66175f-3eda-4082-8080-01005f6b569a)

- Number of tracks released per month

  ![trackspermonth](https://github.com/user-attachments/assets/ce5d274a-1203-4263-b0bb-6af8ef03b5df)


### *5. Genre and Music Characteristics 🎵*
- Correlation between streams and the given musical attributes in the data

  ![streamcorrelattributes](https://github.com/user-attachments/assets/5ba8463c-82fb-422f-89e8-7d7dd048ada1)

  
- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

  ![correl2v2](https://github.com/user-attachments/assets/70b6e88b-f923-492d-b616-e87814e3de03)


### *6. Platform Popularity 🤩*
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?

  ![trackspercategory](https://github.com/user-attachments/assets/f8faf16e-0f96-4321-bfc9-6215b7f500d3)

### *7. Advanced Analysis 🧠*
- Patterns among key and mode of the tracks

![keyvsmode](https://github.com/user-attachments/assets/6aec42e3-e8b7-4f99-bc96-67ec24692f5e)


- Frequent artist(s) in playlists and/or charts

![frequentartists](https://github.com/user-attachments/assets/95bd24d3-2279-497e-b39e-0a20d15dedb2)








## *References:*

## Author
 - Gabriel Ian E. Argamosa
