## Data Lake with Apache Spark
---

### Introduction 
In this project, I try to help one music streaming startup, Sparkify, to move their date warehouse to a data lake. Specifically, I bulid an ETF pipline to extract their data from **S3** and processes them using **Spark**, and loads the data into a new **S3** as a set of *dimensional tables*. This will allow their analytics team to continue finding insights in what songs their users are listening to.

### Dataset 
Datasets used in this project are provided in two public **S3** buckets. One bucket contains info about songs and artists, the second bucket has info concerning actions done by users (which song are listening, etc.. ). The objects contained in both buckets are JSON files.

### Database Schema

I createa a star schema optimized for queries on song play analysis as follows:

![schema](/images/schema.png)

This includes the following tables.

#### Fact Table 
+ **songplays** - records in event data associated with song plays i.e. records with page `NextSong`

#### Dimension Tables
+ **users** - users in the app
+ **songs** - songs in music database
+ **artists** - artists in music database
+ **time** - timestamps of records in **songplays** broken down into specific units

### Spark Process
The ETL job processes the `song files` then the `log files`. The `song files` are listed and iterated over entering relevant information in the artists and the song folders in parquet. The `log files` are filtered by the *NextSong* action. The subsequent dataset is then processed to extract the date, time, year etc. fields and records are then appropriately entered into the `time`, `users` and `songplays` folders in parquet for analysis.


### Project Structure

+ `etl.py` - The ETL to reads data from **S3**, processes that data using **Spark**, and writes them to a new **S3**
+ `dl.cfg` - Configuration file that contains info about AWS credentials
+ `test.ipynb` - test file for `etl.py`

### Code Explanation
This is my Python script for processing song and log data by extracting dimensional tables from the data and loading them back into S3 in Parquet format.

I use Spark to read in song and log data from S3, then process the data to extract the necessary information for dimensional tables. I then write the dimensional tables back to S3 in Parquet format.

The first function, `process_song_data`, extracts data from the song files and creates two dimensional tables: `songs`, which includes information about each song such as title, artist, and duration, and `artists`, which includes information about each artist such as name, location, and latitude/longitude coordinates. The tables are written back to S3 in Parquet format.

The second function, `process_log_data`, extracts data from the log files and creates four dimensional tables: `users`, which includes information about each user such as their name, gender, and level; `time`, which includes information about each event such as the start time, hour, day, week, month, and year; `songplays`, which includes information about each song play such as the start time, user ID, song ID, artist ID, and location; and `songplays_id`, which assigns a unique ID to each song play. These tables are also written back to S3 in Parquet format.