# Austin Crime Analysis

## Objective
* Improving public safety by predicting crime in certain locations in the Austin area. We selected this topic due to these reasons:

### Benefits
* Reducing costs of reports and case investigations
* Support community health and service efforts
* Reducing crime
* Allocating law enforcement in different areas
* Determining predictors of crime

## Data Sources and Description
We retrieved our datasets from the City of Austin’s [open data portal](https://data.austintexas.gov/). We worked with the two following data sets:

* [Crime Reports](https://data.austintexas.gov/Public-Safety/Crime-Reports/fdj4-gpfu): This dataset contains a record of incidents that the APD responded to and wrote a report on from the years 2003 to present. If there were multiple offenses that occurred in one incident, only the highest offense was recorded.
* [Socioeconomic Status of CDC Social Vulnerability Index](https://data.austintexas.gov/Health-and-Community-Services/Socioeconomic-Status-of-CDC-Social-Vulnerability-I/jnp3-n7ij): This dataset is from the Social Vulnerability Index (SVI) which was created to help public health officials identify communities that will most likely need support during a hazardous event. In addition, the SVI indicates the vulnerability of every U.S Census tract based off of 15 social factors including unemployment, minority status, and disability. Only the socioeconomic status features are represented in the dataset and are from the years 2014, 2016, and 2018. 

Below are the features of both datasets that we inner joined on the Census Tract feature
![ERD_AustinCrime](https://user-images.githubusercontent.com/91927712/161450234-f68c4138-998f-4401-afa4-c9e1517fa8f3.png)


## This week everyone should:
* Get familiar with data
* Database and cleaning process - Aaron Hall, Azusena Arroyo
  * Exported Annual Crime Reports and Crime Data data from https://data.austintexas.gov/
  * Loaded raw data into Postgres database hosted by AWS
  * Compared Crime Data to Crime Reports data in Postgres - Filtered some data in Postgres
  * Used Psycopg2, a PostgreSQL database adapter for Python, to connect to Postgres database in Jupyter Notebook
  * created a Pandas dataframe of Crime Data to use for our model using Pandas.io.sql.read_sql method
  * Data cleaning involved dropping columns,'ID','GOHighestOffDesc','Location', 'ClearanceDate', 'Clearance', etc...(more to add here)
  * Converting columns to integers or floats, zipcode, Longitude, Latitude
  * removing rows with bad data - for example, one crime, according to its coordinates, took place in the Atlantic Ocean, or columns with NULL values in key columns
  * binning longitude and latitude values so they could be used to find areas of more crime
  * separating date into days, months, and years and putting time into the hour the crime occurred. 
  * creating a new column, Day of the Week
* Someone should look into machine learning model - Daren Garcia
* Relevant data and feature engineering - Azusena Arroyo, Charla Garcia

## Data Exploration Phase
During the data exploration phase we looked into: 
* count of crime types and identified theft as being the crime with the highest occurence followed by burglary
* count of location type where crimes occured and identified home/residence as the location of most crimes followed by parking lots/garages
* count of zipcodes in total which totalled to 50
* count of council districts which totalled to 10
* count of ADP sector which totalled to 12
* count of ADP district which totalled to 15
* count of crimeazusenaarroyo type (NIBRS UCR) totals to 6

## Future Steps
* Use SQLAlchemy to pull data into the database
* Once you clean data through pandas, push data back into the database: Data cleaning will include: Parsing date by date of the month and/or season, other suggestions welcome. 
* Assumption: We will assume that the incident happened within 2 weeks of the reporting date.
  * Using the Crime Reports data, we have the exact date and time of the incident - so by changing to this dataset, we don't need to make this assumption.
   
* Machine learning model will be a classification model - Multinomial logistic regression for the types of crime type. Can also use random forest.
* Look at predictions
* Analyze predictions

## Dashboard 
The dashboard includes the following technologies: 
* HTML
* Python Libraries: matplotlib, 
* Javascript Libraries: Leaflet, 
* Tableau interactive visualizations 

We would like to include the following interactive elements to view:
* types of crime by zipcode
* types of crime by day of the week
* types of crime by time of day

[Google Slides Presentation
](https://docs.google.com/presentation/d/1EN3ammW-Wlooi3852pIWSFRROaggzyB0reHSVfxEoX8/edit#slide=id.g11f323755e1_1_2810)
