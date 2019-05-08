# Sparkify Data Modeling

The fictional company **Sparkify** is a new startup in the music streaming field, much akin to other companies like Spotify and Apple Music. Sparkify has done a great job collecting user and event data in JSON logs and have properly tagged this content with helpful metadata. Unfortunately, the state that the data is currently in doesn't make it ideal to gain analytical insights, so Sparkify is looking to their friendly, neighborhood data engineer to transform the data into something more manageable for their analytical needs!

## Transformation Strategy

For the purposes of this effort, we'll be creating an ETL job in a Python format to migrate the JSON data into a newly created **Postgres** database. The choice for this is that Postgres offers a relational database model suitable for aggregating / querying, meeting the needs of analytical users like data scientists or data analysts.

## Database Schema

The Postgres database implements a **star schema** around the data, meaning that there is a centralized *fact table* with complementary *dimension tables*. The JSON data we'll be working with allows for a clean normalization of the data into this format. Below, we'll address how the tables will be created as well as the respective fields each table will maintain.

### Fact Table
Table name: **songplays** <br>
Description: This fact table serves as the glue between the dimension tables, including the primary keys from all those respective dimension tables.
 - songplay_id (PRIMARY KEY)
 - start_time
 - user_id
 - level
 - song_id
 - artist_id
 - session_id
 - location
 - user_agent

### Dimension Tables

Table name: **users** <br>
Description: This table contains basic information about the users of the Sparkify application.
 - user_id (PRIMARY KEY)
 - first_name
 - last_name
 - gender
 - level

Table name: **songs** <br>
Description: This table contains information about songs, including thigns like the title of the song, the year it was released, and how long the song is (in seconds).
 - song_id (PRIMARY KEY)
 - title
 - artist_id
 - year
 - duration

Table name: **artists** <br>
Description: This table contains information about the music artist, specifically their given geographical location.
 - artist_id (PRIMARY KEY)
 - name
 - location
 - latitude
 - longitude

Table name: **time** <br>
Description: This table contains segmented timestamp information on when a particular song was played by a Sparkify user.
 - start_time
 - hour
 - day
 - week
 - month
 - year
 - weekday

## ETL
With the database in place and tables in the star schema, we can now extract, transform, and load (ETL) our data from the provided JSON files. The JSON files are segmented into two specific categories, and we glean the following table information as follows:

- **songs_data**: From these JSON files, we extract information into the *songs* and *artists* dimension tables.
- **log_data**: From these JSON files, we extract information into the *users* and *time* dimension tables.
- Using information from the dimension tables, we build our overarching fact table, *songplays*.

## Files Included
Here is a list of the files used throughout this project and the respective purpose of each of them.
 - **data**: Contains the JSON data we will be working from in creating and populating our Postgres database.
 - **etl.ipynb**: This notebook serves as a sort of playground in order to perform sandboxing activities as we build the ultimate ETL job housed in the *etl.py* file.
 - **test.ipynb**: This notebook serves as a means to ensure that are activities are working correctly, such as ensuring that the tables are properly built and properly populated.
 - **create_tables.py**: This file works in tandem with the *sql_queries.py* file to build the tables we'll be utilizing in our Postgres environment.
 - **etl.py**: This file takes our sandboxing information from the *etl.ipynb* notebook and packages it into a final ETL job that will migrate the JSON data from the *data* folders into our Postgres star schema environment.
 - **sql_queries**: This file contains the SQL queries that will build our Postgres tables and properly populate them with the provided JSON data.
