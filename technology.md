# Technologies Used

## Data Cleaning and Analysis
Pandas will be used to clean the data and perform an exploratory analysis. Further analysis will be completed using Python.

## Database Storage
Raw data was loaded into PostgreSQL hosted by AWS will be used, and psycopg2 was used to connect our PostgreSQL database to Jupyter Notebook.

## Machine Learning
SciKitLearn is the ML library we'll be using to create a classifier. Our training and testing setup is a multinomial logistic regression that seeks to predict a categorical variable, crime type, based on different variables for geographic location, such as zip codes and census tracts. We will also be looking at using different time elements of when a crime occurs, such as its month or day, to predict crime type.

## Dashboard
We intend on using an interactive Tableau dashboard that will allow users to view crime type predictions based on the factors discussed above.
