# Netflix Viewing History Analysis - Python

A detailed analysis of my viewing history on Netflix over the past year (2020 - 2021) using pandas and seaborn in python. 

*P.S. : Refer the jupyter notebooks to understand better. This README file is just a description of the project.*

*File ***script1-data_manipulation.ipynb*** contains initial data cleaning and wrangling using which I export the data to a csv file.*

*File ***Script2-Data_Analysis.ipynb*** contains all the working for generating the visualizations.* 


### Motivation

Netflix has truly revolutionized the way we consume content. We find ourselves spending hours watching movies or binge watching popular TV Series. Personally, Netflix is my most preferred platform for consuming content just because of the amount of content available and the ease of use of the platform. 

Pandemic hit the world in 2020 and we were forced to live indoors and work from home. Because we were restricted in our houses, people starting consuming more and more content and started spening much more time on Netflix. 

The aim of this project is to analyse my own viewing history on Netflix over the past year. I tried to find some interesting trends in my viewing history during the pandemic and how it has changed over time. 


# Table of Contents

- [Installing Libraries](#Installing-Libraries)
- [Collecting the Data](#Collecting-the-Data)
- [Reading the Data](#Reading-the-Data)
- [Cleaning the Data](#Cleaning-the-Data)
- [Exporting to CSV](#Exporting-the-Data)
- [Data Visualization](#Data-Visualization)

# Installing Libraries

Use the following command to install the libraries required for this project
> pip install *library_name*

Install the following libraries :

1. pandas
2. matplotlib
3. seaborn

# Collecting the Data

If you have an active Netflix account, you can download your viewing history for free from your ***Account*** page. 

Visit this link for more help : [https://help.netflix.com/en/node/101917](https://help.netflix.com/en/node/101917)

You will have to send a request to Netflix and they will send an email with the files. It can take upto 30 days but I got my data after 24 hours. 

Once you have the data downloaded, you will get multiple folders and all folders will be explained in a Cover Sheet provided by Netflix. For our use, we will only look at the folder *CONTENT_INTERACTION* and specifically only the `ViewingActivity.csv` file inside that folder. 

The `ViewingActivity.csv` file contains the following columns (*as per the Cover Sheet provided by Netflix*):

- ***Profile Name*** : the name of the profile in which viewing occurred. 
- ***Start Time*** : the UTC date and time viewing started.
- ***Duration*** : the length of the viewing session.
- ***Attributes*** : this column shows additional details of interactions with streamed content, where available
    - **Autoplayed : user action: None** : means that the viewer did not interact with that TV show or movie.
    - **Autoplayed : user action: Unspecified** : means that the viewer either interacted with the TV show or movie (such as clicking on the box art and viewing the TV show or movie page while the auto-played content plays), or that the auto-played content was watched longer than 2 minutes.
    - **Autoplayed : user action: User_Interaction** : means that the viewer interacted with the TV show or movie in a browser, by clicking on the video player controls or using keyboard shortcuts.
    - **View was hidden** : indicates that the TV show or movie was marked “hide from viewing history” in Account settings.
    - **Has branched playback** : indicates that the member can make choices during playback, to control what happens next.
- ***Title*** : the TV show or movie viewed.
- ***Supplemental Video Type*** : videos other than a TV show or movie, such as trailers or montages.
    - The reference **N/A** means not applicable.
- ***Device Type*** : the device type from which the TV show or movie was streamed.
- ***Bookmark*** : the most recent viewing position (relative to the total length of the TV show or movie) from the particular playback session of the TV show or movie.
- ***Latest Bookmark*** : indicates whether the Bookmark is the most recent viewing position (relative to the total length of the TV show or movie) from the most recent playback session of a TV show or movie.
    - **Not latest view** : indicates that a particular playback session is not the most recent playback for the TV show or movie and therefore the Bookmark is not the most recent.
- ***Country*** : the country from which the TV Show or Movie was viewed. 


Understanding what the data and especially what the columns mean is a very important step because without knowing what the data is about, our analysis won't be good. 

We can now proceed and start importing the data into our project. 

# Reading the Data

In this project, I will be working on jupyter notebooks. Jupyter notebooks are good if you want to analyze your data quickly and share the analysis with people. 

- Import the necessary libraries. 

    ```
    import pandas as pd
    import seaborn as sns
    import os
    import matplotlib.pyplot as plt
    ```

- Use the `os.get_cwd()` method to get the current directory of the project and join the *ViewingActivity.csv* using the ```os.path.join()``` method to read the required data. 

- Use `pd.read_csv()` method to read the file as a dataframe which will help us clean the data and manipulate it easily. Make sure to read the documentation for pandas : [https://pandas.pydata.org/docs/](https://pandas.pydata.org/docs/)

- Copy the dataframe using `df.copy()` so that if you make any mistake during analysis, you can always roll back to the original data without losing any information. 

- Print the columns of the dataframe using `df.columns`. Additionally, use the `df.info()` method to see the data type of each column. 
Here, you will notice that the column *Start Time* is object type and should be of DateTime type instead. We will handle this during the data cleaning process. Make sure all other columns have the correct data types. 

# Cleaning the Data

A data analyst spends most of their time cleaning the data and structuring the data into a acceptable format. 

(Refer to the ***script1-data_manipulation.ipynb*** notebook to understand the data cleaning process better. )

My aim here is to complete the following tasks :

- [***Task 1 :***](#Task-1) Drop the unwanted columns like *Country*, *Attributes*, *Bookmark*, *Latest Bookmark* etc. 

- [***Task 2 :***](#Task-2) Use the *Start Time* column and convert it into datetime and extract useful information such as *Day Name*, *Year*, *Month*, *Week* etc. 

- [***Task 3 :***](#Task-3) Filter and keep only useful data and remove unwanted rows of data. 

- [***Task 4 :***](#Task-4) Split the Title column to extract *Season* and *Episode* information. 

## Task 1 

When we imported the data as a dataframe, we had 10 columns and the column definitions are described above. Some columns such as Country, Attributes, Bookmark and Latest Bookmark are not required for analysis because either they have a lot of **NaN** values or just 1 value is repeated (for instance in Country column only 1 value is present i.e. India). 

Consequently, such columns can be dropped using the following piece of code. (Refer the jupyter notebooks to understand better)

``` 
columns_to_drop = ['Country', 'Bookmark', 'Latest Bookmark', 'index', 'Attributes']
my_data = my_data.drop(columns = columns_to_drop)
``` 
 Alternatively, you can also use ```df.drop(columns = columns_to_drop, inplace = True)``` to achieve the same result. 

 ## Task 2

 When data is imported from a csv file, the columns that should have DateTime data type is imported as a Object data type. Hence, we need to convert the ***Start Time*** (renamed to Date) column to DateTime using ```pd.to_datetime()``` method. 

Once the required column has been converted to a DateTime data type, we can now access DateTime properties such as extracting month, day name etc using the ```df['Date'].dt``` accessor. 

(Refer the [documentation](https://pandas.pydata.org/docs/reference/api/pandas.Series.dt.html?highlight=series%20dt#pandas.Series.dt) to learn more about different DateTime properties). 

 Also, since the data given is in UTC time zone, I converted it to my local time zone i.e IST (GMT+5:30) using ```df['Date'].dt.tz_localize('GMT').dt.tz_convert('Asia/Kolkata')```

 (Refer the [article](https://stackoverflow.com/questions/22800079/converting-time-zone-pandas-dataframe) to understand the conversion). 

 Finally, we will get *Month*, *Year*, *day_name*, *day_of_week* and a *day* column. This helps us group data by Month or Day_Name to extract useful insights. 

 ## Task 3

 There are a lot of unwanted rows in the data. For instance, the Supplemental Video Type tells us if what you watched was a trailer or a hook or a preview. Because Netflix has an autoplay trailer option whenever you hover above a Movie or TV Show, it is recorded as something you watched. Hence, we will remove such rows because their watch time will be at max 1 minute and it is not useful for our analysis. We are interested in Movies and TV Shows and not the trailers or the hooks. 

 The Supplmental Video Type is NaN when you watch a Movie or TV Show. Hence, we will only keep rows where Supplemental Video Type is NaN using ```my_data = my_data[my_data['Supplemental Video Type'].isna()]```.

This is the data we will use for further analysis. 

## Task 4

The data has a column named ***Title*** which stores information about what you watched. Netflix stores the title of a TV Series or a Movie in a particular format i.e. TV Show : Season : Episode. We can thus split the column on colon and split the Title into 3 different columns. This is illustrated below. 

```my_data[['TV Show', 'Season', 'Episode']] = my_data['Title'].str.split(':', expand = True, n = 2)```

Since movies don't have a season or an episode, all rows where Season is Null will be the movies. The same is mapped in the data using the lambda function and apply method. 

```my_data['Content Type'] = my_data['Season'].apply(lambda x : 'Movie' if x == None else 'TV Show')```


# Exporting the Data

Finally, after cleaning the data and manipulating it as required, the data can be exported to a csv file so that you don't have to keep running the cells again and again. This can be done using ```df.to_csv(filename)```.


# Data Visualization

Please look at the [README](/viz/README.md) file inside the *viz* folder to learn about the data visualization and analysis part of the project. 
