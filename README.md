# Sparkify-ETL-Using-Cloud-Technology on Amazon Redshift.

## Table of Contents

- [Sparkify-ETL-Using-Cloud-Technology(Amazon Redshift)](#sparkify-etl-amazon-redshift)
  - [Introduction](#introduction)
  - [Project Description](#project-description)
  - [Project Requirements](#usage)
  - [Datasets](#tools)
  - [Project Files](#project-files)
  - [Usage](#usage)

## Introduction

A music streaming startup, **Sparkify**, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in **S3**, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

The project is to build an ETL pipeline that extracts their data from **S3**, stages them in **Redshift**, and transforms data into a set of dimensional tables for their analytics team to continue finding insights into what songs their users are listening to.

## Project Description
As a Data Engineer working for Sparkify, I am required to build an ETL (Extract, Transform and Load) pipeline that extracts data from an S3 location which has been provided, stages the collected data into Redshift, and transforms the data into a set of dimensional tables for their analytics team. This will further help the team to continue finding insights into what songs their users are listening to. To complete the project, knowledge of certain technology and tools is required: Python, AWS Redshift, AWS s3 and SQL. 

## Project requirements

Using the song and event datasets, create a star schema optimized for queries on song play analysis including the following tables.

#### Fact Table
1. **songplays**: hold records in event data associated with song plays i.e. records with page `NextSong`
     * songplay_id
     * start_time
     * user_id
     * level
     * song_id
     * artist_id
     * session_id
     * location
     * user_agent
     
#### Dimension Tables
1. **users**: users in the app
     * user_id
     * first_name
     * last_name
     * gender
     * level
2. **songs**: songs in music database
    * song_id
    * title
    * artist_id
    * year
    * duration
3. **artists**: artists in music database
    * artist_id
    * name
    * location
    * lattitude
    * longitude
4. **time**: timestamps of records in songplays broken down into specific units
    * start_time
    * hour
    * day
    * week
    * month
    * year
    * weekday

## Datasets

* **Song Dataset**: The first dataset is a subset of real data from the [Million Song Dataset](http://millionsongdataset.com/). Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. <br>For example, here are file paths to two files in this dataset.
  
```
    song_data/A/B/C/TRABCEI128F424C983.json
    song_data/A/A/B/TRAABJL12903CDCF1A.json
```

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

```
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```

*  **Log Dataset**: The second dataset consists of log files in JSON format generated by this [event simulator](https://github.com/Interana/eventsim) based on the songs in the dataset above. These simulate app activity logs from an imaginary music streaming app based on configuration settings.
The log files in the dataset are partitioned by year and month. <br>For example, here are file paths to two files in this dataset.

```
log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json
```

*  **Log Json Meta Information**: Contains the meta information that is required by AWS to correctly load the Log Dataset

## Project Files

* `Cluster_Create_Delete.ipynb`
  * This jupyter notebook is used to create the cluster and deleting it after completing
  * It is a series of instructions to create the cluster and the roles associated to it
  * The file gets its credentials from `dwh.cfg` so you have to fill the required credentials first

* `dwh.cfg`
  * Create your IAM User with Admin Access and put your credentials in this file
  * Put your cluster credentials and number of nodes you want to create in `Cluster_Create_Delete.ipynb` in this file
  * **_Important_**: Don't forget to remove your credentials before publishing the project

* `create_tables.py`
  * You run this python script after creating the cluster
  * This file creates the 2 staging tables which is **_staging_songs_** and **_staging_events_** tables to store the s3 staged json log and songs data into them
  * This file creates the 4 dimensions and fact table to transfer the data in the staging tables into it
  * The queries that execute to run this script is in `sql_queries.py`

* `etl.py`
  * After we create the cluster and the tables we call this script to transfer the data from the s3 json files to the 2 staging tables created
  * Then it transfers the data from the 2 staging tables to the data warehouse dimensions (4 dimensions) and fact.

* `sql_queries.py`
  * This file contains the queries executed to create and drop the tables, transfering the data into the 2 staging tables then the queries to transfer the data into the data warehouse.

## Usage

* First, you have to create the redshift cluster and the IAM Role to stage the data in it by following the `Cluster_Create_Delete.ipynb` instructions
  
* Second, to run the ETL we have to create the database and the tables so we have to execute `create_tables.py` file.
* `Create_tables.py` creates 2 tables for staging the log and song files from _**S3**_ into _**AWS Redshift**_ and 4 dimensions and a fact table for the star schema data warehouse

* Now we can run `etl.py` to transfer our data from songs and log files into the staging database tables then transfer the data from staging tables to the data warehouse dimensions and fact.

**_Important_**: _update the database and account credentials in the files to your own credentials in `dwh.cfg`






