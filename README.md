# Spotify2023_Exploratory-Data-Analysis 

## Table of Contents
- [About the project](#about-the-project)
- [Objectives](#objectives)
- [Codes](#codes)
- [Results and Discussion](#results-and-discussion)
- [Summary](#summary)
- [References](#references)
- [Author](#author)

## About the project 
Spotify, is an audio streaming service that offers users access to music tracks, podcasts, and other media through a subscription model providing both free (ad-supported) and premium (ad-free, subscription-based) choices to users worldwide. It is a publicly traded company that was founded by Swedish entrepreneurs Daniel Ek and Martin Lorentzon in 2006. 

This project is a comprehensive **Exploratory Data Analysis (EDA)** of Spotify's 2023 dataset. The goal is to examine patterns, trends, and insights within Spotify's dataset, focusing on on different parameters that contribute to Spotify's system. By analyzing, visualizing, and interpreting the data, this project aims to uncover meaningful insights that highlight how users engage with the platform, which artists and genres dominate the charts, and how Spotify's content is distributed across various playlists and charts.


## Objectives

1. Overview of Data set &emsp;&emsp;&emsp;&ensp;&nbsp;&nbsp;&nbsp; 5. Genre and Music Characteristics
2. Basic Descriptive Statistics &emsp;&nbsp;&nbsp;&ensp; 6. Platform Popularity
3. Top Performers &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;&nbsp;&nbsp; 7. Advance Analysis
4. Temporal Trends


## Codes 
## *(View the SPOTIFY-EDA.ipynb for the full output)*


*Load the file and import the libraries*
```python
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# load the file
# use encoding = 'ISO-9959-1' to ensure the characters in the file are read correctly
# if not used, the file would not load
df = pd.read_csv('spotify-2023.csv', encoding = 'ISO-8859-1')
df
```
*Find the number of rows and columns of the data set*
```python
sh = df.shape
print(f"The number of rows and columns in (row, column) is = {sh}")
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
# since there is a specific row in the streams column that is not a numeric value therefore use errors = 'coerce' to turn it to NaN
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
sns.histplot(df['released_year'], kde = False, ax = axes[0])
axes[0].set_title('Distribution of released_year')

# distribution of artist count
sns.histplot(df['artist_count'], kde = False, ax = axes[1])
axes[1].set_title('Distribution of artist_count')

plt.tight_layout() 
plt.show()

# for the trends and outliers
plt.figure(figsize = (8, 4))
sns.scatterplot(x = 'released_year', y = 'artist_count', data = df,  alpha = 0.4) #alpha = 0.4 for opacity
plt.title('Released Year vs Artist Count')
plt.show()
```
*For the Top 5 Tracks*
```python
#sort then .head since top 5
sorted = df.sort_values(by = 'streams', ascending = False)
sorted.head()
```
*Top 5 Artists by the number of tracks*
```python
# top artists by their number of tracks
counts = df['artist(s)_name'].value_counts().head() # to show the list

#to be used for the graph .head() for the top 5, .index() to have their index values, .tolist() to make it into a list
top5 = counts.head().index.tolist()
print(counts)
print(top5) # just to check if its a list

# use is.in(top5) to filter the values in the artist(s)_name column to those that are in the top5 list
filtered = df[df['artist(s)_name'].isin(top5)]

# graph
plt.figure(figsize = (9, 6))
sns.countplot(x = 'artist(s)_name', data = filtered)
plt.title('Top 5 artists by number of tracks')
plt.xlabel('artist(s)_Name')
plt.ylabel('Number of Tracks')
plt.show()
```
*Number of tracks released per year*
```python
# tracks released per year

# for the list, use .value_counts() to check for the count of tracks released per year
# .reset_index() to place a corresponding index for the data
track_year = df['released_year'].value_counts().reset_index()
print(track_year) 
track_year.columns = ['released_year', 'num_tracks']

# to show what is the year with the highest tracks released
# use.loc on the corresponding column and .idxmax() to find the index of the highest value
max_year = track_year.loc[track_year['num_tracks'].idxmax(), 'released_year']
# use .max() to find the highest value
max_tracks = track_year['num_tracks'].max() 
# display
print(f"Year {max_year} with {max_tracks} tracks")

# graph
plt.figure(figsize = (14, 9))
sns.barplot(x = 'released_year', y = 'num_tracks', data = track_year) 
plt.title('Number of Tracks Released Per Year')
plt.xlabel('Year')
plt.ylabel('Number of Tracks')
plt.xticks(rotation = 45)
plt.show()
```
*Number of tracks released per month*
```python
# Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases

track_month = df['released_month'].value_counts().reset_index().sort_values(by = 'released_month') 

# create a dictionary for the name of the months that is equivalent to their month number
months = {1:"January", 2:"February", 3:"March", 4:"April", 5:"May", 6:"June", 7:"July", 8:"August", 9:"September", 10:"October", 11:"November", 12:"December"}

# use map to convert the number to the equivalent month in the dictionary
track_month['name_month'] = track_month['released_month'].map(months) 
#print
print(track_month)
# to show the month with the highest release
max_month = track_month.loc[track_month['count'].idxmax(), 'name_month']
month_released = track_month['count'].max()
print(f"Month with highest release is {max_month} with {month_released} songs")

# graph
plt.figure(figsize = (10,7))
sns.barplot(x = 'name_month', y = 'count', data = track_month)
plt.title("Number of releases per month")
plt.xlabel("Months")
plt.xticks(rotation = 45)
plt.show()
```
*Correlation between streams and musical attributes*
```python
# use .corr() to get their correlation
correl = df[['streams','danceability_%', 'valence_%', 'energy_%', 'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']].corr()

# correl['streams'] to check the relation between streams column from each attribute
correl_streams = correl['streams'].sort_values(ascending = False) 
print("Correlation to streams")
print(correl_streams)

# using a heatmap since heatmaps are the common graphs for correlation
plt.figure(figsize = (9,6))
sns.heatmap(correl, annot = True, cmap = "inferno", fmt = ".1g", linewidths = 0.5)
plt.show()
```
*Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?*
```python
# Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%
correl_DE = df[['danceability_%', 'energy_%']].corr()
correl_VA = df[['valence_%', 'acousticness_%']].corr()

print("The correlation between danceabiity_% and energy_% is")
print(correl_DE)
print()
print("The correlation between valence_% and acousticness_% is")
print(correl_VA)
```
*How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?*
```python
# use .sum() to compile the total number of tracks in the playlists
# use .apply(pd.to_numeric, errors='coerce') to apply individually the errors = 'coerce' since initially the datatype of in_deezers_playlist is object
numtrack_playlists = df[['in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']].apply(pd.to_numeric, errors = 'coerce').sum()
print("Number of tracks in the categories:")
print(numtrack_playlists)

# graph
plt.figure(figsize = (8, 5))
sns.barplot(x = numtrack_playlists.index, y = numtrack_playlists.values)
plt.title("Number of Tracks in Each Category")
plt.xlabel("Category")
plt.yscale('log')
plt.ylabel("Number of Tracks")
plt.show()

# based on the results in number 5, which is the 'sorted' data frame here are the Top 5 tracks with their respective count in the playlists
df.loc[[55, 179, 86, 620, 41], ['track_name', 'artist(s)_name', 'in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']].head()
```
*Patterns among tracks with regards to key and mode*
```python
stream_data = df['mode'].value_counts()
# Calculate the mean streams for each mode
# use .groupby() to get the average of mode and key with respect to streams
stream_data = df.groupby(['mode','key'])['streams'].mean().reset_index()
# sort highest to lowest
sorted_stream = stream_data.sort_values(by = 'streams', ascending = False)
print("Average streams by mode and key\n", sorted_stream)

# graph
plt.figure(figsize = (8, 6))
color = ['#435E55FF','#D64161FF']
sns.barplot(x = 'key', y = 'streams', hue = 'mode', data = sorted_stream, palette = color)
plt.xlabel("Key")
plt.ylabel("Average Streams")
plt.title("Patterns among tracks with key and mode by average") 
plt.legend(title = 'Mode')
plt.show()

```
*Most frequent artist in playlists and/or charts*
```python
# since there are errors when computing with the chosen columns, convert columns to numeric by using errors = 'coerce'
conv_col = ['in_spotify_playlists', 'in_spotify_charts', 'in_apple_playlists', 'in_apple_charts', 
            'in_deezer_playlists', 'in_deezer_charts', 'in_shazam_charts']
df[conv_col] = df[conv_col].apply(pd.to_numeric, errors='coerce')

# for the appearances of artists
# use groupby to aggregate the appearance of the artist(s) per column
appear = df.groupby('artist(s)_name')[conv_col].sum()
appear['Total_appearances'] = appear.sum(axis=1)
sorted_appear = appear.sort_values(by = 'Total_appearances', ascending = False).reset_index()

# graph
plt.figure(figsize = (9,6))
sns.barplot(x = 'artist(s)_name', y = 'Total_appearances', hue = 'Total_appearances', data = sorted_appear.head())
plt.title("Top 5 artists in all playlists provided") # it can be Top n, just modify the .head() to .head(n)
plt.ylabel("Artists")
plt.show()
```


## Results and Discussion
### 1. *Overview of the Dataset 👀*
- The number of rows and columns of the given data is 953 rows 24 columns
```
The number of rows and columns in (row, column) is = (953, 24)
```
  
- Here are the datatypes of each column of the given data:
 
![datatypes](https://github.com/user-attachments/assets/b000984f-4128-4960-9da3-f2128d02eb6e)

  
- Are there any missing values? *(Checked using isnull)*

![nullvalues](https://github.com/user-attachments/assets/5d28f497-378d-469c-bc82-a947c7ffa0cb)

***Discussion:*** 
Based on the results above, the general overview of the data is that the shape of the data frame is 953 rows and 24 columns, with seven columns classified as objects and 17 as int64.

When checking for null values, it was found that there were 50 in in_shazam_charts and 95 in the key column.

### 2. *Basic Descriptive Statistics 📈*
- Mean, Median, and Standard Deviation of the 'streams' column

  ![meanmedstdev](https://github.com/user-attachments/assets/d5756c26-89c7-4900-aecb-09704241c919)
  
  *In Microsoft Excel* 

   ![excel](https://github.com/user-attachments/assets/5865a5ea-b932-4785-8cec-4e014b448397)



- Distribution of the data of released_year and artist(s)_count
 
![distribartistsreleasedyr](https://github.com/user-attachments/assets/f1f89bb9-16cc-44f9-a814-bd3fdcb964ee)

![trendsoutliers](https://github.com/user-attachments/assets/96dc37be-375c-4655-a15d-daac28bf41f8)




***Discussion:*** 
The results shown above for the mean, median, and standard deviation were determined by the pandas .mean(), .median(), and .std() functions. 
After that, the three were also calculated in Microsoft Excel to check, and the results were the same.

As for the trends and outliers that can be noticed, it can be seen that as the years progress, more tracks are released per year, and most of them are done by solo artists.
### *3. Top performers 🎤*

- Top 5 Most Streamed Tracks

| Track |      Artist(s)      |
| ------------- | ------------- |
| 1. Blinding Lights  | The Weeknd       |
| 2. Shape of You   | Ed Sheeran     |
| 3. Someone You Loved   |  Lewis Capaldi |
| 4. Dance Monkey  |  Tones and I |
| 5. Sunflower - Spider-Man: Into the Spider-Verse  | Post Malone, Swae Lee  |
  
- Top 5 Artist(s) by their number of tracks

![top5artistsbytracks](https://github.com/user-attachments/assets/468216fa-25fb-4ff3-a0fe-c3d54d4633f9)

***Discussion:*** 
The table above shows the Top 5 Tracks and their corresponding Artist(s). As for the graph, it is observed that Taylor Swift has the highest amount of track released
followed by The Weeknd, then, Bad Bunny and SZA, and lastly, Harry Styles.


### *4. Temporal Trends 📊*
- Number of tracks released per year

  ![tracksyear](https://github.com/user-attachments/assets/b54dc5ab-8188-488b-acb4-ab7a9dab4b91)

- Number of tracks released per month

  ![trackpermonth](https://github.com/user-attachments/assets/50378ed4-28fa-4fdc-a0ab-8e83a916d2ac)

***Discussion:*** 
The year 2022 was the year when the highest number of song tracks were released. The months where there was the highest amount of releases are January, probably because it is a new year, to begin with, and next is the month of May. The month with the lowest release is the month of August.

### *5. Genre and Music Characteristics 🎵*
- Correlation between streams and the given musical attributes in the data

  ![streamcorrelattributes](https://github.com/user-attachments/assets/5ba8463c-82fb-422f-89e8-7d7dd048ada1)

  *Heatmap*
  
  ![heatmap](https://github.com/user-attachments/assets/71babda8-5b70-4ddf-a46d-1a0564c49761)

  
- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

  ![correl2v2](https://github.com/user-attachments/assets/c0ecee87-4e3a-404a-8dbd-7e88f0d4edc7)


***Discussion:***
When talking about the correlation between streams and musical attributes, it can be observed that the values are far from one *(closer to one represents a high relationship)*,
meaning that none of the attributes affect or have a relationship with streams

On the other hand, when looking at the correlation between danceability_% and energy_%, it can be seen that it is still far from 1 since it needs to have at least 0.6 to show there is a good correlation.
Therefore, it can be concluded that the danceability of a song is likely to be energetic.

Lastly, for valence_% and acousticness_%, the results show that they do not affect each other or only little since the value is close to zero and is negative(-0.081907). Meaning that the valence of a track has inversely related and has little to no effect on its acoustics.


### *6. Platform Popularity 🤩*
- How do the numbers of tracks in spotify_playlists, deezeer_playlist, and apple_playlists compare? Which platform seems to favor the most popular tracks?

  ![trackseachcategorygraph](https://github.com/user-attachments/assets/c8ef5d98-5855-4074-b440-d336d3458802)

  
  ![trackseachcategorytable](https://github.com/user-attachments/assets/433b2bea-8470-4a88-9972-07d92317e276)

  

***Discussion:***
A significant difference can be observed across spotify_playlists, deezer_playlist, and apple_playlists in the data. Nearly 5 million tracks are included in Spotify playlists, showing the platform's
widespread usage of playlists for music and other stuffs such as promotion and organization. However, only a few make it onto Deezer's playlist having 95,913 tracks, showing that only a few are popular enough to achieve this position.
Lastly, compared to Spotify, Apple Music Playlist has fewer carefully selected tracks, with 64,625 music in playlists, probably because of a different strategy to their choices. This distribution highlights Spotify's high chart selectivity and reliance on playlists for song discovery.

In the table shown, based on the earlier findings about the Top 5 tracks, it can be observed that spotify_playlists is the platform that favors the most popular tracks



### *7. Advanced Analysis 🧠*
- Patterns among key and mode of the tracks

![keyvsmode](https://github.com/user-attachments/assets/6aec42e3-e8b7-4f99-bc96-67ec24692f5e) 
![modekey2](https://github.com/user-attachments/assets/d26612da-b188-426d-93a8-1c3435e03eb1)




- Frequent artist(s) in playlists and/or charts

![freqartists](https://github.com/user-attachments/assets/e7b7e0cb-a3d0-416c-b391-f0a80ef7ae59)

***Discussion:***
When looking at the average of tracks with respect to their mode and key, it can be seen that the most common key is E major, having 7.605963e+08 streams, and the least common is G# minor, only having  3.219036e+08 streams. E major is the highest probably because it is said to be the "brightest and most powerful key." 

When comparing all charts in the given dataset, it is found that The Weeknd is the most frequent artist that is played, having 150,273 appearances in all playlists combined. Then, Taylor Swift and Ed Sheeran were chosen for the Top 2 and 3. 
The code for this is set to only find the Top 5 frequent artist(s), but it can be modified to how many are needed.


## Summary
**Loading the data** - initially, the dataset is unable to load so the use of encoding = 'ISO-9959-1' was done to ensure the characters in the file are read correctly

**Familiarizing with the data** - in the dataset there is a non-numeric value in the streams column which made the other processes in the data-analysis done with the use of errors = 'coerce'. This was proven when the file was checked in Microsoft Excel. This was also done in part on the correlation and Top 5 Artist(s) in all playlists to make sure there is no non-numeric value.

**Visualization** - the use of histplot, barplot, scatterplot, countplot, displot was done to interpret data. A heatmap was used to correlate streams with the music attributes. As for the second part of the correlation, the same process was done as the one done on the first one; however, I chose not to use a graph since there were only two values compared each time, so the numeric representation is enough.

**Reflection:**

The project was challenging for me as a beginner programmer, but I learned a lot and gained valuable skills. I hope that this project can help future programmers and/or artist(s).


## *References:*
 - ECE2112 Lecture Materials
 - Colón, L. (2024, October 25). Spotify. Encyclopedia Britannica. https://www.britannica.com/topic/Spotify
 - Spotify Data Analysis Project | Spotify Data Analysis Using Python | Data Analysis | Simplilearn. https://www.youtube.com/watch?v=8d7ywKCm6HI
 - 35 - Pandas - pandas.to_numeric() Method. https://www.youtube.com/watch?v=DrQzwmPr8Ts
 - Seaborn Is The Easier Matplotlib. https://www.youtube.com/watch?v=ooqXQ37XHMM&t=36s
 - Group By and Aggregate Functions in Pandas | Python Pandas Tutorials. https://www.youtube.com/watch?v=VRmXto2YA2I

## The project is mostly focused on the use of PANDAS and different visualization techniques in Python. If there are any problems, inquiries, or recommendations about the experiment, please let me know!
## Author
 - Gabriel Ian E. Argamosa
