# ETL preparation of database for Covid-19 in South Korea
#### Columbia University Data Analytics Bootcamp - ETL Project (August 2020) 
#### Project By: Duvan Diaz, Shivani Thakkar, Eline Van Eldere
-------------------------------------------------------------------------------


## Overview
In this project, we want to clean and structure our data using ETL procedure in order to find some answers and trends related to COVID-19.
For this we are using a dataset retrieved via kaggle.com. It was for South Korea and last updated on June 30, 2020.
Dataset URL: https://www.kaggle.com/kimjihoo/coronavirusdataset


## Goals
-      Find appropriate data

-       Create Pandas dataframes for the data

-       Join and clean dataframes in Pandas

-       Connect Pandas to SQL database

-       Load Pandas dataframes into SQL database


## Justification of dataset used
Our group chose this dataset because we considered this a good example of a relevant and up-to-date topic, and we deemed it interesting to see the variations in effects and reactions to Covid-19 in South-Korea. This is a pretty complete and insightful dataset with lots of possibilities for investigation.
The dataset comprises of data from the initial outbreak until June 30th 2020, so it gives an overview of the first 6 months of the pandemic and the responses to it in South-Korea.

## Justification of source selection within dataset
We decided to use specific list of CSV files from the dataset which were the most apt for what we would want to investigate. We skipped others because their contents were outside the scope of this project.

•	We used the following list of CSV files in this project:
*       Case
The number of cases by cause of infection and location
*        Regions
Cases per region, specified into population groups (i.e. schools or nursing homes)
*        PatientInfo
Anonymized patient information (date, status, location, gender, age, etc.)
*        Policy
All government policies over the last six months combined with the total cases according to the date of discovery
*        Time
Dataset with the rate of testing and confirmed cases. Included Number of Released and Deceased patients. 

CSV files we left out:
• SearchTrend

• SeoulFloating

• TimeProvince

• Weather

• TimeAge (derived from PatientInfo)

• TimeGender (derived from PatientInfo)


## I. Data Cleanup
For the dataset to be ready for analysis, we performed actions on it in Jupyter Notebook. We started by storing the CSV files into Pandas data frames.
We read the CSV files into Pandas, to begin with we loaded more than we ended up using, as we wanted to have as much information as possible to start from and wanted to be able to use them if necessary. Afterwards we ended up selecting the ones we could perform the analysis on we found the most interesting.
We started out by creating data frames in Pandas. After merging them together we selected the columns we could use in our analysis and copied those, rather than just drop them, because we would still be able to retrieve them if necessary. We replaced the ‘NAN’ values with ‘0’. Furthermore, we dropped the duplicates. We then established the connection to Postgres.

The 'PatientInfo' CSV file has defined each patient with a unique id which is stored in a column named ‘patient_id’. Technically, this column should have all unique values but there was one duplicate value in it. So, we checked both records which was having the same patient id. Both rows were having same values except ‘city’ value. So, we checked for the difference and those locations were almost same so we considered  the second row a duplicate value and dropped it.

As part of the cleanup of duplicates, we were able to see that in pandas, two rows or more are considered duplicates even if one of their values in a column may be different, they are still duplicates based on the index and other columns.
Also, by renaming a 'start_date' column as a 'date' column, both data frames from each csv could be merged using date as the main column.
In conclusion we selected an organized set of csv databases, which allowed us to speed up the cleanup process.

## II. Data Analysis

  The first part of the analysis was to convert the merged data frame of 'policy.csv', and 'time.csv', which displayed the information of total safety policies and the number of cases, deaths, and testing count; and it was transformed into an average count of results, according to the start date of the policy implementation. Following, the average results data frame, was the final table that was used to upload to the SQL server database which, and that allowed us to perform queries for extra analysis. We imported both 'pandasql' and 'sqlalchemy' dependencies so that we would be able to perform queries to the SQL database for all created tables('regions, testingpolicies, patient_info'). Some of the queries performed were used to find specific results that showed the number of implemented safety policies and the type of policies they were. Also, we were able to look at the rate of patients that were released, and those who unfortunately were deceased, according to the date of policies.
  
Other Queries:
-       Investigating the effect of various policies installed over time by the South Korean government
-       Looking at the infection rate over time and its nature: grouped infections or individual ones
-       Determining which age group and gender was most affected
-       Trying to find out if there is any clear distinction in ‘number of infections’ or ‘death rate’ between different age groups or genders

Upon transfering the data frames to Postgres, we created the tables and assigned the primary keys. We assigned it to the variables to prevent creating duplicates i.e. we assigned a primary key to 'city'.
  
        
## III. Conclusions

We were able to determine that even though the rate of testing increased, and the implementation of more and more strictly policies increased as well, the effect it had on the spread of the virus in South Korea, was minimal, showing that it helped reduce the speed of the rate of spread of the virus, but it did not slow down the reporting of number of both negative and positive cases. Concluding that the rate overall slowed down but maintained a steady increment. We selected an organized set of csv databases, which allowed us to speed up the cleanup process.
