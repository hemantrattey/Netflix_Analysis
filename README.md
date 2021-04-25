# Netflix Viewing History Analysis - Python

A detailed analysis of my viewing history on Netflix over the past year (2020 - 2021) using pandas and seaborn in python. 

### Motivation

Netflix has truly revolutionized the way we consume content. We find ourselves spending hours watching movies or binge watching popular TV Series. Personally, Netflix is my most preferred platform for consuming content just because of the amount of content available and the ease of use of the platform. 

Pandemic hit the world in 2020 and we were forced to live indoors and work from home. Because we were restricted in our houses, people starting consuming more and more content and started spening much more time on Netflix. 

The aim of this project is to analyse my own viewing history on Netflix over the past year. I tried to find some interesting trends in my viewing history during the pandemic and how it has changed over time. 

# Table of Contents

- [Collecting the Data](#Collecting-the-data)
- Reading the Data 
- Cleaning the Data
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

# Collecting the Data

If you have an active Netflix account, you can download your viewing history for free from your ***Account*** page. 

Visit this link for more help : [https://help.netflix.com/en/node/101917](https://help.netflix.com/en/node/101917)

You will have to send a request to Netflix and they will send an email with the files. It can take upto 30 days but I got my data after 24 hours. 

Once you have the data downloaded, you will get multiple folders and all folders will be explained in a Cover Sheet provided by Netflix. For our use, we will only look at the folder *CONTENT_INTERACTION* and specifically only the `ViewingActivity.csv` file inside that folder. 

The `ViewingActivity.csv` file contains the following columns :



