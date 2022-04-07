# Austin Crime Analysis

## Why Crime?
Austin, TX is one of the fastest growing major cities in Texas and we were interested in learning about it's crime profile and if there was a way to predict crime in Austin. In addition, we were interested in learning of any trends that could help give direction to community health and service efforts to improve public safety.

## Questions we hope to answer
* Where does crime occur the most?
* What types of crime occur the most?
* What times of the day or the month does crime occur?
* Are there predictors of crime and if so, what are some of those predictors?
* Can socioeconomic profiles by location help predict crime?
* Can crime be accurately predicted?

## Data Sources and Description
We retrieved our datasets from the City of Austin’s [open data portal](https://data.austintexas.gov/). We worked with the two following data sets:

* [Crime Reports](https://data.austintexas.gov/Public-Safety/Crime-Reports/fdj4-gpfu): This dataset contains a record of incidents that the APD responded to and wrote a report on from the years 2003 to present. If there were multiple offenses that occurred in one incident, only the highest offense was recorded.
* [Socioeconomic Status of CDC Social Vulnerability Index](https://data.austintexas.gov/Health-and-Community-Services/Socioeconomic-Status-of-CDC-Social-Vulnerability-I/jnp3-n7ij): This dataset is from the Social Vulnerability Index (SVI) which was created to help public health officials identify communities that will most likely need support during a hazardous event. In addition, the SVI indicates the vulnerability of every U.S Census tract based off of 15 social factors including unemployment, minority status, and disability. Only the socioeconomic status features are represented in the dataset and are from the years 2014, 2016, and 2018. 

Below are the features of both datasets that we inner joined on the Census Tract feature
![ERD_AustinCrime](https://user-images.githubusercontent.com/91927712/161450234-f68c4138-998f-4401-afa4-c9e1517fa8f3.png)

## Preprocessing & Data Exploration Phase
  * Exported Annual Crime Reports and Crime Data data from https://data.austintexas.gov/
  * Loaded raw data into Postgres database hosted by AWS
  * Compared Crime Data to Crime Reports data in Postgres - Filtered some data in Postgres
  * Used Psycopg2, a PostgreSQL database adapter for Python, to connect to Postgres database in Jupyter Notebook
  * created a Pandas dataframe of Crime Data to use for our model using Pandas.io.sql.read_sql method
  * Data cleaning involved dropping columns,'ID','GOHighestOffDesc','Location', 'ClearanceDate', 'Clearance', etc...(more to add here)
  * Converting columns to integers or floats, zipcode, Longitude, Latitude
  * Removing rows with bad data - for example, one crime, according to its coordinates, took place in the Atlantic Ocean, or columns with NULL values in   key columns
  * Binning longitude and latitude values so they could be used to find areas of more crime
  * Separating date into days, months, and years and putting time into the hour the crime occurred. 
  * Creating a new column, Day of the Week

During the data exploration phase we looked into: 
* count of crime types and identified theft as being the crime with the highest occurence followed by burglary
* count of location type where crimes occured and identified home/residence as the location of most crimes followed by parking lots/garages
* count of zipcodes in total which totalled to 50
* count of council districts which totalled to 10
* count of ADP sector which totalled to 12
* count of ADP district which totalled to 15
* count of crimeazusenaarroyo type (NIBRS UCR) totals to 6

## Machine Learning Model
### Description of Data Preprocessing
We began with a very large dataset of 2.4 million rows of crime reports. The first step was to clean out any reports that: 

1. didn't result in an actual crime report,
2. were not crimes that were coded by the FBI as part of its Uniform Crime Reporting program.

Using this code and the code descriptions helped group the crimes of our study into 7 types of crime:

* Burglary
* Theft
* Auto Theft
* Aggravated Assualt
* Rape 
* Murder
* Robbery

After this filtering, we were left with approximately 260,0000 rows of crime data to use for our analysis.

### Description of feature engineering and the feature selection, including their decision making process
To help us predict the type of crime, we combine features from our AustinCrime dataset that record details about the crime committed, such as the occurred time, time of week, hour, and census tract. Census tracts were used to join this data along with socioeconomic features, such as unemployment rates, per capita income, and estimates of people below the poverty line. Since these are directly tied to the census tract, we decided that it would be best to include these in our machine learning model since they can be very good predictors in determining what type of crime is committed. 

### Description of how data was split into training and testing sets
Seventy-five percent of our data was allocated towards training, while the remaining twenty-five percent was allocated for testing.

### Explanation of model choice, including limitations and benefits
Our initial model choice was using a multinomial logistic regression model, given that our target, CategoryDescription, is analogous to crime type. This model had the benefit of fitting our data into a target variable that consists of categorical data rather than quantitative. However, a limitation was that our model's classification report suggested that this type of model was not the best fit for our data, since it can be that location features, such as the census tract, were impairing the accuracy of our model.

### Explanation of changes in model choice (if changes occurred between the Segment 2 and Segment 3 deliverables
As a result, we changed our model to a random forest classifier model. This fits a number of decision tree classifiers on various sub-samples of our dataset and uses averaging to the improve the predictive accuracy of the model, as well as regulating over-fitting of the model. We proceed to create a confusion matrix and classification report that demonstrates a considerable improvement in the predictive accuracy of our model.

### Description of how they have trained the model thus far and any additional training that will take place
So far, our random forest is using 1000 estimators. The model was trained using by using a StandardScaler instance for our features, and we analyzed the key differences when fitting the model with Austin Crime data alone and when merged with the Census Tract socioeconoomic data. We found that the accuracy for our model was improved using the Austin Crime data alone; when removing the socioeconomic features (unemployment, educational attainment, etc.), the predicitive accuracy of our model increases, suggesting that these socioeconomic factors are not very indicative for predicting crime type. We also looked at training the data with and without grouping some of the target categories, but seemed to not affect the predictive accuracy of our model. 

### Description of current accuracy score
Our current accuracy score for our best random forest classifier is 0.74. This seems to suggest fairly high accuracy and the further metrics of the classification report do not seem to indicate overfitting. We also used Optuna for hyperparameter tuning; more specifically, to maximize our macro F1 and/or weighted F1 score, which currently score a 0.45 and 0.69 respectively. Given that our Austin Crime data alone has about 260,000 rows of data and 17 features, it seems to be faily reliable, but given class imbalances, we might benefit from maximizing the F1 scores with Optuna. By updating the number of trials and trees, we were able to increase our weighted F1 score to 0.70.

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
* Interactive navigation bar
* Tableau interactive visualizations 

We would like to include the following interactive elements to view:
* types of crime by zipcode
* types of crime by day of the week
* types of crime by time of day

[Google Slides Presentation
](https://docs.google.com/presentation/d/1EN3ammW-Wlooi3852pIWSFRROaggzyB0reHSVfxEoX8/edit#slide=id.g11f323755e1_1_2810)
