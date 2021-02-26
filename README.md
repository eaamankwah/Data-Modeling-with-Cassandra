# Data-Modeling-with-Cassandra

# Table of Contents
* Overview
* Dataset
* Table Creation
* ETL Pipeline using Python and CQL
* Project Files 
* References

## Overview
This project is second of the Udacity Data Engineering Nanodegree. 

The objective of the project is to create a NoSQL Cassandra database (sparkifydb) with denormalized tables designed to optimize queries on song play analysis. A database and ETL pipeline for this analysis will be created using Python and CQL. The ETL pipeline will be used to transfer data set of CSV files within a directory to create a streamlined CSV file to model and insert data into Apache Cassandra tables.

## Dataset

The dataset is a collected by and online startup platform called Sparkify who wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. Sparkify wants to access the songs  that their users are listening to. The  data currently resides in a directory of CSV files on user activity on the app.

For example,  below are file paths to two files in this dataset.
event_data/2018-11-08-events.csv 
event_data/2018-11-09-events.csv 

## Table Creation
The event_datafile_new.csv contains the following columns:
* artist
* firstName of user
* gender of user
* item number in session
* last name of user
* length of the song
* level (paid or free song)
* location of the user
* sessionId
* song title
* userId

Three tables were created as follows:

**Session History Table**

CREATE TABLE IF NOT EXISTS session_history (
sessionid int, 
userid int, 
iteminsession int, 
song text, 
artist text, 
length float, 
firstname text, 
lastname text, 
gender text,
location text,
level text,
PRIMARY KEY ((sessionid, iteminsession)));

* The primary key consisted of a combination of partition key sessionId, and clustering key itemInSession so that the appropriate filtering could be done.
* The columns of the session_history included sessionId, itemInSession, artist, song_title and song_length.

The query belowas used to test the table.

SELECT artist, song_title, song_length FROM session_songs WHERE sessionId = 338 AND itemInSession = 4

**User History Table**

CREATE TABLE IF NOT EXISTS user_history (
sessionid int, 
userid int, 
iteminsession int, 
song text, 
artist text, 
length float, 
firstname text, 
lastname text, 
gender text,
location text,
level text,
PRIMARY KEY ((userId, sessionid), iteminsession))
WITH CLUSTERING ORDER BY (iteminsession ASC);

* The primary key was consisted of composite partition key userId, sessionId so that it can be unique for every row of data.
* The itemInSession was used as a clustering key so that the output could be ordered by it.
* The columns of our user_history table included userId, sessionId, itemInSession, artist, song and firstName and lastName.

The query belowas used to test the table.

SELECT itemInSession, artist, song, firstName, lastName FROM user_songs WHERE userId = 10 AND sessionId = 182

**Song History Table**
CREATE TABLE IF NOT EXISTS song_history (
sessionId int, 
userId int, 
itemInSession int, 
song text, 
artist text, 
length float, 
firstName text, 
lastName text, 
gender text,
location text,
level text,
PRIMARY KEY (song, userId)
);

The primary key consisted of a partition key song, and clustering key userId order so that every row could be uniquely identified.
The columns of the song_history table included song, firstName, lastName and userId.

The query belowas used to test the table.

SELECT firstName, lastName FROM app_history WHERE song = 'All Hands Against His Own'

## ETL Pipeline using Python and CQL

The ETL pipeline  mainly transferred data from a set of CSV files within a directory to create a streamlined CSV file to model and insert data into Apache Cassandra tables.

## Project Files

1. data - folder called event_data contains the data files, including csv datasets.
2. Project template: contains project instructions from building and implementing  the etl processes to testing the database with select queries including "where" clauses. 
3. README.md - provides discussion on your project.

References

* [Udacity Q & A Platform](https://knowledge.udacity.com/?nanodegree=nd027&page=1&project=572&rubric=2500)
