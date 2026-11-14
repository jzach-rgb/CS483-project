FareWise – NYC Taxi Fare Prediction
CS 483 – Requirement 1: Data Discovery and Infrastructure
Team: Data Minerz

1. Project Goal

The name of our project is FareWise. Our goal is to estimate the cost of a taxi trip in NYC and investigate the impact of weather on the price of the trip. The features we will use are the coordinates of the start point and the end point, the time of pick-up, the number of passengers, and weather-related features.

Further, we will develop a model that can predict the price of the trip and classify the trips as more than $20 and $20 or less. Also, we will use mapping to investigate the problem of taxi costs and its causes in various districts of NYC. For Requirement 1, we concentrated on the preparation of the dataset. Our prediction models are not trained yet.

2. Data Sources

We use three public data sources.

NYC Taxi Fare Prediction (Kaggle)
Link: https://www.kaggle.com/competitions/new-york-city-taxi-fare-prediction/data
Published through a Kaggle competition hosted by Google Cloud. We downloaded the full training CSV and uploaded it to Databricks. 

Historical Weather (Open-Meteo)
Link: https://open-meteo.com/en/docs/historical-weather-api
The NYC hourly weather data from 2009-2015 was obtained in CSV format. This dataset contains information on temperature, rain, snow, precipitation, and wind speed. The Open-Meteo API is licensed under the CC BY 4.0 license. The free API also has usage limits.
License: https://creativecommons.org/licenses/by/4.0/

NYC Taxi Zones (NYC Open Data)
Link: https://data.cityofnewyork.us/Transportation/NYC-Taxi-Zones/8meu-9t5y
This is available from the NYC Taxi and Limousine Commission (TLC). We have downloaded the GeoJSON file version and loaded the same into Databricks. This file is publicly available. The licensing details are not mentioned on the dataset website, hence we shall give credits and find out about usage details of the data.

3. Dataset Description

Our taxi training data set includes 55,423,856 rows and occupies 5,697,178,298 bytes (or around 5.7 Gb). It comprises 8 original features – fare amount, pick-up time, pick-up and drop-off coordinates, passenger count, and a record key. Trips cover period from 2009 to June 2015.

Our weather CSV data set includes 56,928 hourly observations and has 6 fields: time, temperature, precipitation, rain, snowfall, and wind speed. We ensured that hourly stamps were unique and there were no missing values among these fields.

Our new GeoJSON file includes 263 NYC taxi zones. Every zone has a name, borough, location ID, and geographical coordinates. Compared to CSV file, GeoJSON file comprises nested objects and coordinate array. We loaded it in PySpark, checked its schema and ensured that this file contains MultiPolygon geographical shapes. We did not measure its size yet.

We already merged taxi and weather data sets using pickup hour field. As a result of our first cleaning process, we left 54,033,721 taxi trips, all of which had a corresponding weather hour. Next, we eliminated 21 fares greater than $500. Our stored taxi-weather data set has 54,033,700 rows and 15 columns.

The taxi zones are currently stored separately. Later, we can use them to show neighborhoods on a map or identify which zone a pickup happened in.

4. Analysis of the 5 Vs

Volume

The size of the taxi data file is quite large, as there are more than 55 million trips in it, which take up 5.7 GB of space. The amount of resources required for uploading, reading, cleaning, and joining this entire file is greater compared to a smaller file. Databricks and PySpark were used to work on the entire training file and not just a sample of the data file.

Velocity

Taxi trips occur on a daily basis while weather keeps changing every hour. However, the data files that we have downloaded are historical in nature. The existing model uses batch processing rather than live streaming. In case there are new data files of taxi and weather in the future, we can repeat the whole process.

Variety

The dataset used for this project is of three distinct kinds. The taxi rides and the weather are the CSV data files. The zones are the semi-structured GeoJSON data files with geographic coordinates nested within them. Thus we have different file types as well as different kinds of data – fares, weather, and neighborhoods.

For reading the CSV as well as GeoJSON data, we have used PySpark. We have successfully done the join operation of taxi rides and hourly weather data. However, matching the taxi coordinates with GeoJSON zones will require an additional geographic matching step that we aim to explore later on.

Veracity

We performed data cleaning on the taxi data set prior to analyzing it. We identified 376 entries that had a missing value for the drop-off latitude/longitude, 3,834 fares equal to $0 or below, and 195,532 passenger counts outside our range of 1 to 6. We also identified 2,745 invalid values for the latitude/longitude during our initial run.

We applied filters for the fares, passenger count and coordinates using the criteria we specified for the study. This filtering operation resulted in the elimination of 1,390,135 rows with a remaining total of 54,033,721 entries. We later eliminated 21 exceptionally high fares above $500, resulting in 54,033,700 entries. We also identified no unmatched weather hour in the initial data clean up.

Our filtering criteria are not without flaws; real trips will probably be eliminated and some exceptional trips will remain. Location, timestamp and geographic boundary checks are also required.

Value

We are interested in understanding if we can forecast taxi fares and if incorporating weather variables helps improve the performance of the model. Moreover, we would like to understand the differences in taxi fare amounts in various areas of NYC based on taxi zones.

We will compare prediction models that use weather variables versus those that do not use weather variables. In the case of fare amount prediction, we can analyze prediction errors, while for classification of rides where the fare is more than $20, we will analyze metrics like precision and recall as less than 10% of the data was classified into this category in our initial dataset.

5. Data Preparation

We created a common Databricks workspace and uploaded our data to a Unity Catalog Volume:

/Volumes/workspace/default/farewise_data/

The taxi datasets are in taxi/, weather datasets in weather/, and the GeoJSON dataset in zones/. There are also processed/ and processed_final/ directories for saving the taxi-weather dataset.

What we've done so far:

- Uploaded the complete taxi CSV, weather CSV, and taxi zones GeoJSON.
- Loaded all three formats with PySpark and inspected their structures.
- Inspected the number of records, missing values, and weird taxi values.
- Converting the taxi features into numeric values and creating pickup timestamps.
- Matched taxi trips with weather trips by pickup hours.
- Creating the clean taxi-weather dataset and storing it in Parquet.
- Reading the GeoJSON file, extracting its features, and counting 263 zones.

6. Limitations and Risks

Our data about the trip and weather belongs to 2009-2015 and hence might not be up-to-date regarding the taxi fare. Zone boundaries file is created in 2025 and hence may not align with the zones used in our older dataset.

Our data about the weather refers to just one location in New York City, and hence it may not necessarily reflect the weather at every point of pick-up. Also, we need to consider time zones while analyzing the data. Certain records in our data might have incorrect coordinates and fares. Our methods for cleaning the data may filter out some trips that might have been valid.

Our data doesn't contain any information about traffic, road distance, and trip duration. Large-scale analysis also requires time and space in Databricks. Lastly, we need to follow Kaggle competition guidelines and give credit to Open-Meteo and NYC Open Data sources.

Sources

Kaggle taxi data: https://www.kaggle.com/competitions/new-york-city-taxi-fare-prediction/data
Open-Meteo weather: https://open-meteo.com/en/docs/historical-weather-api
NYC Taxi Zones: https://data.cityofnewyork.us/Transportation/NYC-Taxi-Zones/8meu-9t5y

The record counts and processing results above come from our Databricks notebook. The taxi zones have been loaded and checked, but not yet joined with the taxi records.
