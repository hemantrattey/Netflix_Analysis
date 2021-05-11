# Data Visualization

This folder consists of all the analysis and visualizations that have been generated for this project. 

(Refer the **Script2** jupyter notebook to understand the code for the same). 

The following are the visualizations which will be generated. 

- [***Task 1 :***](#Task-1) : Average Watch time (Seconds) by day of week
- [***Task 2 :***](#Task-2) : Viewing History by day of week
- [***Task 3 :***](#Task-3) : Top 10 TV Shows by Average Watch time (Seconds)
- [***Task 4 :***](#Task-4) : Top 10 Binge Watching TV Series
- [***Task 5 :***](#Task-5) : Distribution of Watch time across months
- [***Task 6 :***](#Task-6) : Distribution of Watch time across day of week 

Before proceeding with the data visualization, I extracted the hours, minutes and seconds from the ***Duration*** column and converted up all the values to seconds and added them up (see code below). This is because counting the number of episodes per day was not a good variable as some values are less than a minute and I wouldn't like to call them as watching an episode. 

```
# Convert duration HH:MM:SS to number of minutes 
df['Duration (Seconds)'] = df['Duration'].str.split(':').apply(lambda x: int(x[0])*3600 + int(x[1])*60 + int(x[2]))
```

*P.S. : plots are generated using [Seaborn](https://seaborn.pydata.org/) and [Matplotlib](https://matplotlib.org/) but feel free to use your own preferable library for this.* 

## Task 1

To get the average watch time for each day of week, I grouped the data by *day_name* column and took the mean of *Duration(Seconds)* column. Since day of week is a categorical variable, it made sense to plot a bar graph with day of week as the *x-axis* and the average value as *y-axis*. See the code below to understand this. 

```
avg_seconds_per_day = df.groupby('day_name')['Duration (Seconds)'].mean().sort_values()
day_of_week = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
sns.set_style("darkgrid")
font = {'family': 'serif',
        'color': '#004466',
        'weight': 'normal',
        'size': 16}
plt.figure(figsize=(20,15))
ax = sns.barplot(x = avg_seconds_per_day.index, y = avg_seconds_per_day.values, palette=("Blues_d"), ci = None, order=day_of_week)
ax.set_ylabel('Viewing Duration (in Seconds)',fontdict={'size': 13, 'family': 'serif'})
ax.set_xlabel('Day of Week', fontdict={'size': 13, 'family': 'serif'})
ax.set_title('Average Viewing Time per Day (in seconds)', fontdict=font)
ax.tick_params(axis = 'both', labelsize= 12)
plt.savefig(os.path.join(pwd, 'viz', 'Average Viewing Duration per Day (seconds).png'))
```

*Please note that seaborn orders the axis based on what it discovered first, hence I passed the *day_of_week* list to order the axis from Monday to Sunday.* 

Alternatively, I could have just passed the ***day_name*** column as x argument and the ***Duration(Seconds)*** column as the y argument in the ```barplot()``` method and by default seaborn would calculate the mean value for each day. 

But, I wanted to know how the grouping look like that's why I grouped the data first then plotted it. 

## Task 2

To get the viewing history by day of week (Most watched on which day), I counted the number of days on which I used Netflix and watched something. This is basically a frequency table plotted as a bar graph. To check the frequency you can use the ```df['day_name'].value_counts()``` method which will return a series with the unique values in day_name column as the index and the count of each value as the column. See the code below to understand. 

```
sns.set_style("darkgrid")
font = {'family': 'serif',
        'color': '#004466',
        'weight': 'normal',
        'size': 16}
plt.figure(figsize=(20,15))
sns.countplot(x = df['day_name'], palette=('Blues_d'), order=day_of_week)
ax.set_ylabel('Count of Days',fontdict={'size': 13, 'family': 'serif'})
ax.set_xlabel('Day of Week', fontdict={'size': 13, 'family': 'serif'})
ax.set_title('Most Watched on Which Day?', fontdict=font)
ax.tick_params(axis = 'both', labelsize= 12)
plt.savefig(os.path.join(pwd, 'viz', 'Most Watched on Which Day.png'))
```

If you print ```df['day_name'].value_counts()``` you get the following result and can match it with the graph :
```
Saturday     104
Tuesday       77
Thursday      75
Monday        74
Wednesday     74
Friday        62
Sunday        33
Name: day_name, dtype: int64
```

## Task 3

Now, before doing any further analysis, I split the data into *TV Shows* and *Movies* seperately using the ***Content_Type*** column and filtering the data on it. 

To plot the Top 10 TV Shows by Average Watch time, I first grouped the data by ***TV Show*** column and took the mean of the ***Duration (Seconds)*** column. I then sorted the data on Duration (Seconds) in descending order to get the Top 10 only. 

Alternatively, if I just wanted to see the Top 10 TV Shows by their watch time then instead of taking the mean I would've taken the sum. See the code below. 

```
most_watched_tv_series = tv_shows.groupby('TV Show')['Duration (Seconds)'].mean().reset_index().sort_values(by = 'Duration (Seconds)', ascending = False)
font = {'family': 'serif',
        'color': '#004466',
        'weight': 'normal',
        'size': 16}
plt.figure(figsize=(22, 12))
ax = sns.barplot(y = most_watched_tv_series['TV Show'][:10], x = most_watched_tv_series['Duration (Seconds)'][:10], orient = 'h', ci = 'None')
ax.set_ylabel('TV Series', fontdict={'size': 13, 'family': 'serif'})
ax.set_xlabel('Watch Time (in seconds)', fontdict={'size': 13, 'family': 'serif'})
ax.set_title('Top 10 TV Shows by Average Duration (seconds)', fontdict=font)
ax.tick_params(axis = 'both', labelsize= 12)
plt.savefig(os.path.join(pwd, 'viz', 'Top 10 TV Shows by Average Duration (seconds).png'))
```

## Task 4

To get the Top 10 TV Series which I binge watched, I counted the number of episodes I watched each day for each TV Show i.e. I grouped the TV Shows dataframe on the ***Date*** and the ***TV Show*** and returned the count of ***Episodes***. This count represents how many episodes I watched per day per TV Show. I then filtered the data where count was greater than 5 since I consider binge watching as watching more than 5 episodes per day per TV series. Finally, I just grouped the data on ***TV Show** and summed up all the episodes to get the top 10 most binge watched TV series. See the code below. 

```
binge = tv_shows.groupby([tv_shows.index, 'TV Show'])['Episode'].count().reset_index().sort_values(by='Episode', ascending=False)

binge = binge[binge['Episode'] > 5]

top_10_binge = binge.groupby('TV Show')['Episode'].sum().reset_index().sort_values(by = 'Episode', ascending = False)

font = {'family': 'serif',
        'color': '#004466',
        'weight': 'normal',
        'size': 16}
plt.figure(figsize=(22, 12))
ax = sns.barplot(y = top_10_binge['TV Show'][:10], x = top_10_binge['Episode'][:10], orient = 'h', ci = 'None')
ax.set_ylabel('TV Series', fontdict={'size': 13, 'family': 'serif'})
ax.set_xlabel('Watch Time (in seconds)', fontdict={'size': 13, 'family': 'serif'})
ax.set_title('Top 10 Binge Watching TV Series', fontdict=font)
ax.tick_params(axis = 'both', labelsize= 12)
plt.savefig(os.path.join(pwd, 'viz', 'Top 10 Binge Watching TV Series.png'))
```

## Task 5

To get a distribution of watch time across the months, I chose to plot a boxplot where the *x axis* was the ***Month*** and the *y axis* represented the distribution of ***Duration (Seconds)***. This basically means that seaborn will look at all rows of data, will then see the month name and see the value of Duration (Seconds) and put it as a point on the plot. Each observation of data is a point on the data (distribution of numerical variable across a categorical variable). See the code below. 

```
plt.figure(figsize=(20, 10))
ax = sns.boxplot(x = df['Month'], y = df['Duration (Seconds)'])
ax.set_ylabel('Watch Time (Seconds)', fontdict={'size': 13, 'family': 'serif'})
ax.set_xlabel('Month', fontdict={'size': 13, 'family': 'serif'})
ax.set_title('Distribution of Duration (Seconds) over the months', fontdict=font)
ax.tick_params(axis = 'both', labelsize= 12)
# ax.set_xticklabels(month_name, rotation = 45)
plt.savefig(os.path.join(pwd, 'viz', 'Distribution of Duration (Seconds) over the months.png'))
```

## Task 6

To get the distribution of Duration (Seconds) across day_name, similar to the plot generated in the previous task, just pass the ***day_name** in the x argument of ```boxplot()``` method. See the code below. 

```
plt.figure(figsize=(20, 10))
ax = sns.boxplot(x = df['day_name'], y = df['Duration (Seconds)'], order = day_of_week)
ax.set_ylabel('Watch Time (Seconds)', fontdict={'size': 13, 'family': 'serif'})
ax.set_xlabel('Day of Week', fontdict={'size': 13, 'family': 'serif'})
ax.set_title('Distribution of Duration (Seconds) over the day of week', fontdict=font)
ax.tick_params(axis = 'both', labelsize= 12)
# ax.set_xticklabels(month_name, rotation = 45)
plt.savefig(os.path.join(pwd, 'viz', 'Distribution of Duration (Seconds) over the day of week.png'))
```