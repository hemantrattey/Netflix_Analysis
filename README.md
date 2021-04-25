# Netflix Viewing History Analysis - Python

A detailed analysis of my viewing history on Netflix over the past year (2020 - 2021) using pandas and seaborn in python. 

### Motivation

Netflix has truly revolutionized the way we consume content. We find ourselves spending hours watching movies or binge watching popular TV Series. Personally, Netflix is my most preferred platform for consuming content just because of the amount of content available and the ease of use of the platform. 

Pandemic hit the world in 2020 and we were forced to live indoors and work from home. Because we were restricted in our houses, people starting consuming more and more content and started spening much more time on Netflix. 

The aim of this project is to analyse my own viewing history on Netflix over the past year. I tried to find some interesting trends in my viewing history during the pandemic and how it has changed over time. 


# Table of Contents

- [Installing Libraries](#Installing-Libraries)
- [Collecting the Data](#Collecting-the-Data)
- [Reading the Data](#Reading-the-Data)
- [Cleaning the Data](#Cleaning-the-Data)
    - Extracting Datetime information
    - Removing unwanted data
- Exporting to CSV
- Data Visualization
    - Average Watch time (Seconds) by day of week
    - Viewing History by day of week
    - Top 10 TV Shows by Average Watch time (Seconds)
    - Top 10 Binge Watching TV Series
    - Distribution of Watch time across months
    - Distribution of Watch time across day of week 

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

- Use the `os.get_cwd()` method to get the current directory of the project and append the ViewingActivity.csv to read the required data. 

- Use `pd.read_csv()` method to read the file as a dataframe which will help us clean the data and manipulate it easily. Make sure to read the documentation for pandas : [https://pandas.pydata.org/docs/](https://pandas.pydata.org/docs/)

- Copy the dataframe using `df.copy()` so that if you make any mistake during analysis, you can always roll back to the original data without losing any information. 

- Print the columns of the dataframe using `df.columns`. Additionally, use the `df.info()` method to see the data type of each column. 
Here, you will notice that the column *Start Time* is object type and should be of DateTime type instead. We will handle this during the data cleaning process. Make sure all other columns have the correct data types. 

# Cleaning the Data