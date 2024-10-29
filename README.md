# Spotify2023_Exploratory-Data-Analysis 

## Table of Contents
- About the project
- Objectives
- Codes
- Results and Discussion
- References
- Author


## About the project &emsp;&emsp;&emsp;&emsp;**Oh wag mo na kopyahin kaibigan**
Spotify, is an audio streaming service that offers users access to music tracks, podcasts, and other media through a subscription model providing both free (ad-supported) and premium (ad-free, subscription-based) tiers to users worldwide. It is a publicly traded company that was founded by Swedish entrepreneurs Daniel Ek and Martin Lorentzon in 2006. 

This project is a comprehensive **Exploratory Data Analysis (EDA)** of Spotify's 2023 dataset. The goal is to examine patterns, trends, and insights within Spotify's dataset, focusing on on different parameters that contribute to Spotify's system. By analyzing, visualizing, and interpreting the data, this project aims to uncover meaningful insights that highlight how users engage with the platform, which artists and genres dominate the charts, and how Spotify's content is distributed across various playlists and charts.


## Objectives

1. Overview of Data set &emsp;&emsp;&emsp;&ensp;&nbsp;&nbsp;&nbsp; 5. Genre and Music Characteristics
2. Basic Descriptive Statistics &emsp;&nbsp;&nbsp;&ensp; 6. Platform Popularity
3. Top Performers &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;&nbsp;&nbsp; 7. Advance Analysis
4. Temporal Trends

| ____   |       ____        |
| ------------- | ------------- |
| 1. Overview of Dataset   | 5. Genre and Music Characteristics        |
| 2. Basic Descriptive Statistics   | 6. Platform Popularity      |
| 3. Top Performers   |  7. Advance Analysis |
| 4. Temporal Trends  |   |

## Codes *(View the SPOTIFY-EDA.ipynb for the full output)*
luh kokopya sya oh

*Load the file and import the libraries*
```python
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# load the file
# use encoding ISO-9959-1 to ensure the characters in the file are read correctly
# if not used, the file would not load
df = pd.read_csv('spotify-2023.csv', encoding = 'ISO-8859-1')
```
*Find the number of rows and columns of the data set*
```python
sh = df.shape
print(f"The number of rows and columns in rowXcolumn is = {sh}")
```
*Find out the datatypes of each column*
```python
datatypes = df.dtypes
print(datatypes)
```
*Check for missing values using isnull*
```python
nullval = pd.isnull(df).sum()
print(nullval)
```
*Mean, median, and standard deviation of the 'streams' column*
```python
# since the datatype of stream is initially an object, convert it to a numeric value
# since there is a specific row in the streams column that is not a numeric value therefore use errors = 'coerce'
df['streams'] = pd.to_numeric(df['streams'], errors = 'coerce')  
print(f"The data type of column 'stream' is now {df['streams'].dtypes}")

# mean
dfmean = df['streams'].mean()

# median
dfmedian =  df['streams'].median()

# standard deviation
dfstdev = df['streams'].std()

print(f"Mean:{dfmean}")
print(f"Median:{dfmedian}")
print(f"Standard deviation:{dfstdev}")
```
*Distribution of released_year and artist_count? Are there any noticeable trends or outliers?*
```python
# use histplot to show the two together side-by-side
fig, axes = plt.subplots(1, 2, figsize= (14, 6))

# distribution of released years
sns.histplot(df['released_year'], kde=False, ax = axes[0])
axes[0].set_title('Distribution of Released Years')

# distribution of artist count
sns.histplot(df['artist_count'], kde=False, ax = axes[1])
axes[1].set_title('Distribution of Artist Count')

plt.tight_layout() 
plt.show()

# for the trends and outliers
plt.figure(figsize=(8, 4))
sns.scatterplot(data=df, x='released_year', y='artist_count', alpha = 0.4) #alpha = 0.4 for opacity
plt.title('Released Year vs Artist Count')
plt.xlabel('Released Year')
plt.ylabel('Artist Count')
plt.show()
```
## Results and Discussion
### 1. *Overview of the Dataset 👀*
- The number of rows and columns of the given data is 953 rows 24 columns
```
The number of rows and columns in rowXcolumn is = (953, 24)
```
  
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
  
  *In Microsoft Excel* attach din ung file ng excel

   ![excel](https://github.com/user-attachments/assets/5865a5ea-b932-4785-8cec-4e014b448397)



- Distribution of the data of released_year and artist(s)_count
 
![distribartistsreleasedyr](https://github.com/user-attachments/assets/f1f89bb9-16cc-44f9-a814-bd3fdcb964ee)

![trendsoutliers](https://github.com/user-attachments/assets/9be9f57f-2c7c-413e-a0ba-7cfa47da737b)


***Discussion:*** 
The shown results above for the mean, median, and standard deviation were done by the pandas .mean(), .median(), and .std() functions. 
After that, the three were also calculated in Microsoft Excel portraying the same results.(show proof ge subukan mo kumopya)
As for the trends and outliers that can be noticed, it can be seen that as the years progress, more tracks are released per year and most of them are done by solo artists.
### *3. Top performers 🎤*

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

***Discussion:*** 
The table above shows the Top 5 Tracks and their corresponding Artist(s). As for the graph, it is seened that Taylor Swift has the highest amount of track released
followed by The Weeknd, then, Bad Bunny and SZA. Lastly, Harry Styles.


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

![freqartists](https://github.com/user-attachments/assets/e7b7e0cb-a3d0-416c-b391-f0a80ef7ae59)







## *References:*
 - Colón, L. (2024, October 25). Spotify. Encyclopedia Britannica. https://www.britannica.com/topic/Spotify
## Author
 - Gabriel Ian E. Argamosa
