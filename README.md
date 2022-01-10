# Data-Engineering-Capstone
Udacity Project 6 of 6

# Project Summary:
This project gathers I94 Immigration data for International travel to the United States, US cities demographics data, and airport codes data in and creates a data pipeline which uses Spark in order to create a data model that includes Fact and Dimension tables for analyzing data relating to Air travel to the United States.

# The project follows the follow steps:

  - Step 1: Scope the Project and Gather Data
  - Step 2: Explore and Assess the Data
  - Step 3: Define the Data Model
  - Step 4: Run ETL to Model the Data
  - Step 5: Complete Project Write Up

# Step 1: Scope the Project and Gather Data

Initially the raw data will be loaded into the project workspace Capstone Project Template.ipynb.

This project creates a data pipeline which extracts the following datasets:
  I94 Immigration data
  US cities demographics data
  Airport codes data
  
Using Spark, the data will then be cleaned and transformed into a data model consisting of fact and dimension tables. Finally, the analytics tables with clean data will be written to parquet files which can be read and analyzed using Spark or copied into a data warehouse such as Amazon Redshift for analysis. Data quality checks will be applied at various stages in the pipeline to ensure the data is loaded correctly and is of high quality.

1. I94 Immigration Data - Contains international visitor arrival data by world regions and select countries (including top 20), type of visa, mode of transportation, age groups, states visited (first intended address only), and the top ports of entry for select countries.

2. Airport Codes Data - This is a simple table of airport codes and corresponding cities. The airport codes may refer to either IATA airport code, a three-letter code which is used in passenger reservation, ticketing and baggage-handling systems, or the ICAO airport code which is a four letter code used by ATC systems and for airports that do not have an IATA airport code (from wikipedia).

3. US Cities Demographics Data - This dataset contains information about the demographics of all US cities and census-designated places with a population greater or equal to 65,000. This data comes from the US Census Bureau's 2015 American Community Survey.

4. Country Codes Data - This dataset contains information that relates the country ID from the I94 Immigration dataset to the full name of the country. This data comes from the I94_SAS_Labels_Descriptions.SAS file

# Step 2: Explore and Assess the Data

# Exploring:
  - Checking for Null Airport ID (ident)
  - Checking for Null iata_code - Will need to drop records with Null iata_code
  - Counting number of each type of airport - Data model will include only airports classified as small_aiport, medium_airport, large_airport, or closed
  - Checking number of records where the airline is Null
  - Counting each state's population by race
  - Checking each data set for duplicates - Will drop all duplicate records in later steps if found
  - Top 10 most common iata_code
  - Unique Airport types
  - Top 10 most common destination cities

# Cleaning Steps
  The following steps will be applied to clean the raw data:
  1. Airport data will be filtered to only include airports of types small airport, medium airport, large airport, and closed. Additionally, the final airports table will only include airports with a valid IATA code.
  2. Travelers table will be created by selecting only travelers who have either a valid flight number or a valid airline since we are creating data model only based on air travel data.
  3.Travel records with an invalid destination city or destination state will be removed.
  4. Population data will be aggregated to include the total population, male population, female populaiton, and foreign-born population totals for each state.
  5. Data types will be cast to the desired types based on the data model.

# Step 3: Define the Data Model
Steps necessary to pipeline the data into the chosen data model:
    1. Read the data from source files into Spark Dataframes
    2. Performing data cleaning steps to handle unwanted data
    3. Join and transform data into fact and dimension tables
    4. Run data quality checks to ensure the fact and dimension tables are created correctly and contain records
    5. Write final fact and dimension tables to destination folders stored as parquet files
    
# Step 4: Run Pipelines to Model the Data

# 4.1 Create the data model
  Load data into Spark for cleaning and transformation:
    - Create airports Dimension Table
    - Create travelers Dimension Table
    - Create travel Fact Table
    - Create us_states Dimension Table
    - Create country Dimension Table
  
# 4.2 Data Quality Checks
  Quality checks include:
    - Verify that there are no Null values in the key columns of any of the tables
    - Check the counts of the tables and verify they match the count of records in the source table views
    - Verify key columns have unique values for all tables
    - Writing the final certified tables to Parquet files

# 4.3 Data dictionary
- Create a data dictionary for your data model. For each field, provide a brief description of what the data is and where it came from. You can include the data dictionary in the notebook or in a separate file.

  The data dictionary and data model are included in the file data_dictionary.xlsx

# Step 5: Complete Project Write Up

# Rationale for choosing Pandas for data exploration and Spark for ETL pipelines and data quality checks:
  - Pandas library was chosen for data exploration and analysis because Pandas stores data in local memory and is meant for working with smaller sets of data.
  - For data transformations, cleaning, and ETL with the full data sets, Spark was chosen as it is optimized for working with large datasets and the tasks can be parallelized across the Spark cluster.

# How often should the data be updated?
  - The data can be expected to be updated on a daily basis, with new records added to the travel Fact table and new travelers added to the travelers table as new unique   records are identified

# Changes in the approach for different scenarios:

# 1.) The data was increased by 100x
  - If the size of the data were to be increased by 100x, we could store the datasets in S3 which provides virtually unlimited storage capacity, we could add additional nodes to the Spark cluster, and we can use a cloud data warehouse such as Amazon Redshift.

# 2.) The data populates a dashboard that must be updated on a daily basis by 7am every day
  - If the data populates a dashboard that must be updated by a certain time each day, Airflow can be used to schedule jobs which start each day with enough time to complete before 7am. The start time can be adjusted depending on how many resources are allocated and how much data is expected to be processed for that day's batch.

# 3.) The database needed to be accessed by 100+ people
  - If the database needed to be accessed by 100+ people, we could load the data into a managed data warehouse such as Amazon Redshift or Snowflake which are able to scale up depending on the amount of users and number of queries being run.
