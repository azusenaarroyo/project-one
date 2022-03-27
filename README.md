# project-one
A shared repository for first projects

## Objective
* Improving public safety by predicting crime in certain locations in the Austin area. We selected this topic due to these reasons:

### Benefits
* Reducing costs of reports and case investigations
* Support community health and service efforts
* Reducing crime
* Allocating law enforcement in different areas
* Determining predictors of crime

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

## Future Steps
* Use SQLAlchemy to pull data into the database
* Once you clean data through pandas, push data back into the database: Data cleaning will include: Parsing date by date of the month and/or season, other suggestions welcome. 
* Assumption: We will assume that the incident happened within 2 weeks of the reporting date.
  * Using the Crime Reports data, we have the exact date and time of the incident - so by changing to this dataset, we don't need to make this assumption.
   
* Machine learning model will be a classification model - Multinomial logistic regression for the types of crime type. Can also use random forest.
* Look at predictions
* Analyze predictions

[Google Slides Presentation
](https://docs.google.com/presentation/d/1EN3ammW-Wlooi3852pIWSFRROaggzyB0reHSVfxEoX8/edit#slide=id.g11f323755e1_1_2810)
