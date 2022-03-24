# Notes/Steps taking in ETL process.

* Created a database diagram QuickDBD.
  * Added DB_Documents folder in GitHub repo with database definition and pic of ERD.
* Exported Annual Crime Reports and Crime Data data from https://data.austintexas.gov/
* Loaded raw data into a new Postgres database, austincrimes, hosted by AWS
* Compared Crime Data to Crime Reports data in Postgres - Filtered some data in Postgres
* Used Psycopg2, a PostgreSQL database adapter for Python, to connect to Postgres database in Jupyter Notebook
* created a Pandas dataframe of Crime Data to use for our model using Pandas.io.sql.read_sql method
* Data cleaning involved dropping columns,'ID','GOHighestOffDesc','Location', 'ClearanceDate', 'Clearance', etc...(more to add here)
* Converting columns to integers or floats, zipcode, Longitude, Latitude
* removing rows with bad data - for example, one crime, according to its coordinates, took place in the Atlantic Ocean, or columns with NULL values in key columns
* binning longitude and latitude values so they could be used to find areas of more crime
* separating date into days, months, and years and putting time into the hour the crime occurred. 
* creating a new column, Day of the Week
* Exported Socioeconomic Status of CDC Social Vulnerability Index from https://data.austintexas.gov/
* Pared down Socioeconomic Status of CDC Social Vulnerability Index to have columns we are interested in.
* Parsed the Location colum and created a new CensusTract column that matches CensusTract on Austin Crime Data.
* Imported to new CensusTract table in PostgreSQL austincrimes db.
