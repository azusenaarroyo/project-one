A. Datasets
.....
B. Data Preprocessing
The main steps in preparation of data for modelling on the two datasets were:

Data Reduction-
The original Crime data we were trying to use did not include the time of the incident, only the time the incident was reported. It also only included crimes in the time span of 2015-2018. We decided instead to use Crime Reports which included
crimes reported from 2003 up to the present day. The were nearly two and half million records which was more than enough for our purposes. 
We filtered crimes that did not have a UCR category which is a code used by the FBI as part of its Uniform Crime Reporting program. We also filtered out other row that were missing data of that we planned on using in our machine learning model, 
latitude, longitude, and time of day the crime occurred for example. After reducing the data, we had a dataset of crimes occuring between 2003 and March 3rd, 2022.

We also joined the crime data with Socioeconomic Status of CDC Social Vulnerability Index data. We joined on CensusTract and by doing that, filtered the data down to around 90,000 rows. Many of the census tracts in the crime data
did not match census tracts in the Socioeconomic data. This caused some problems and is area for growth in this project as a more comprehensive joining of records could yield different results.
Because of the discrepancy of records between crime data merged with census data and crime data on by itself, we decided to construct two parallel dataframes with two machine learning models one for each set of data. (actually four as we also
had one set with all violent crimes grouped into a single gourp and one set without the grouping per each dataset.)

 
Data Cleaning and Preprocessing-

To merge the census data with crime data, we joined on the CensusTract column. However, in the CensusTract dataset, the “Location” column held the census tract information and was stored in a string with the words “Census Tract”, followed by the census tract number, and then the name of the county, for example: 
Census Tract 15.05, Travis County, Texas. The census tract number was separated out in excel using text to columns and separating on ‘ ‘, before loading the data into PostgreSQ database.

The first cleaning step was dropping columns that we knew would not make sense to encode and include in our study: 'ID','GOHighestOffDesc','Location', 'ClearanceDate', 'Clearance','XCoordinate','YCoordinate','Location','Address',
'ReportDateTime','ReportDate','ReportTime', 'the_geom', 'County', 'Loc'.

We changed the datatype of fields that were object type though all the values were decimals, ex. Latitude and Longitude.

We had a lot of data and a relatively small number of rows with null values, so we tried dropping nulls. When we did this, we noticed that all Rapes were removed from the dataframe. This was because longitude and latitude were null for all Rapes,
as the location of the crimes are not included in the report to protect the victims. So, we used pgeocode to set the Longitude and Latitude of the rape cases based on the zipcode of the crime, which was included.

We seperated out the month, day, time and day of week from the OccurredDateTime column so we could use them in our ML model.

We also made OccurredTime an integer datatype for example 10:45 pm would be 2245 and then added OccurredHour so we could group crimes by the hour they were committed.

We had a handful of columns that were object datatypes with several different values, for example Location - the location a crime was committed. We wanted to preserve these values as they may have weight in the ML model so we encoded them.
To encode them, we calculated the frequency pct of each value and added a new column, ex: location_dist and populated the column with the occurrence frequency.
We did the same thing for CensusTract, zipcode, APDSector.






There were a few Nan values in some of the latitude and longitude locational values of incidents in the Denver dataset. As these variables aren’t directly used the final dataset, these incidents do not need to be dropped from the dataset. There were also some incidents that took place well beyond Denver’s boundary, so they were removed from the dataset. There were no other inconsistencies in the data for the attributes to be used.
Data Reduction-
In both the datasets, we selected attributes that were of use to us and removed the rest. In the Denver dataset, we retained only three columns to use for the modelling. From the GDELT dataset, we retained three columns which were then transformed and integrated into the Denver crime dataset.
Another way we reduced the data was by using the ‘IS_CRIME’ column to remove the traffic accidents in the datasets as traffic accidents were reported as incidents in the dataset.
Data Transformation-
The data transformation on the Denver dataset was to break down the ‘FIRST_OCCURRENCE_DATE’ data into features such as year, month, day, day of the week and hour. We also minimized the ‘OFFENSE_CATEGORY_ID’ values from 14 different values to 7. Crimes such as aggravated assault, murder and arson which risk fatal harm are categorized as dangerous crimes, incidents of some form of theft such as larceny or robbery were combined into one category and vehicle-related crimes were combined into one category. The ‘HOUR’ feature and the ‘TONE’ feature was categorized into 6 intervals for the association rule mining part of the modelling.
In the GDELT dataset, we multiplied the number of articles and the average tone for each event, then we calculated the average tone for events on a particular day, which became our variable representing the news cycle in the city of Denver on that day.   
Data Integration-
We integrated the GDELT and Denver datasets using the date as the key to obtaining the final dataset that was used for the modelling. The date attribute that was used as the key was present in the GDELT dataset as ‘SQLDATE’ so we converted the ‘FIRST_OCCURRENCE_DATE’ by using DateTime functions to obtain a variable in the form that was present in the GDELT dataset.       
C. Data Analysis
By conducting an initial statistical analysis of the datasets, we tried to understand the data and realize the features that shall be used in the modelling. Data has been visualized to better understand the data and each of them covers how criminal incidences were spread according to each aspect of a variable.
Figure 1 shows how crime is distributed in the city of Denver in terms of the different types of crimes that are occurring in the city. As we can clearly see the most frequent type of crime occurring in Denver (except for ‘all-other-crimes) are public disorder, larceny and thefts from motor vehicles. Crimes such as robbery, arson and murder are the least frequent types of crime in the city.
Figure 2 shows the hourly distribution of crime in Denver. Crime is significantly less in late night to early morning and peaks during the early evening from 4 P.M. to 6 P.M. and late night at 10 P.M.
Figure 3 shows the overall trend of crime throughout the time period of the data. There are a few peaks and troughs and the overall mean is around 185 cases per day in the city of Denver. 
Figure 4 shows neighbourhoods with the highest number of cases in the city. Neighbourhoods such as Five Points are clearly the most dangerous with well over 20,000 cases recorded in the neighbourhood, with at least 5000 more cases than the second most dangerous neighbourhood which is CBD.