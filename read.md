6. Limitations and Risks

Our data about the trip and weather belongs to 2009-2015 and hence might not be up-to-date regarding the taxi fare. Zone boundaries file is created in 2025 and hence may not align with the zones used in our older dataset.

Our data about the weather refers to just one location in New York City, and hence it may not necessarily reflect the weather at every point of pick-up. Also, we need to consider time zones while analyzing the data. Certain records in our data might have incorrect coordinates and fares. Our methods for cleaning the data may filter out some trips that might have been valid.

Our data doesn't contain any information about traffic, road distance, and trip duration. Large-scale analysis also requires time and space in Databricks. Lastly, we need to follow Kaggle competition guidelines and give credit to Open-Meteo and NYC Open Data sources.

Sources

Kaggle taxi data: https://www.kaggle.com/competitions/new-york-city-taxi-fare-prediction/data
Open-Meteo weather: https://open-meteo.com/en/docs/historical-weather-api
NYC Taxi Zones: https://data.cityofnewyork.us/Transportation/NYC-Taxi-Zones/8meu-9t5y

The record counts and processing results above come from our Databricks notebook. The taxi zones have been loaded and checked, but not yet joined with the taxi records.
