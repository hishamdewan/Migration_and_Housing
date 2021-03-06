# Impact of Migration on Housing Prices
## Project Outline
- [Impact of Migration on Housing Prices](#impact-of-migration-on-housing-prices)
  - [Project Outline](#project-outline)
  - [Introduction to the Project](#introduction-to-the-project)
    - [Selected Topic](#selected-topic)
    - [Underlying reason](#underlying-reason)
    - [Description of the source of data](#description-of-the-source-of-data)
    - [Questions we hope to answer with the data](#questions-we-hope-to-answer-with-the-data)
  - [Data Exploration](#data-exploration)
  - [Data Analysis and creation of Machine Learning Models](#data-analysis-and-creation-of-machine-learning-models)
    - [Model 1: House price and Migration](#model-1-house-price-and-migration)
    - [**Model 2: House Price Index and 30 year Fixed Mortagage Rates**](#model-2-house-price-index-and-30-year-fixed-mortagage-rates)
      - [Neural Network Model](#neural-network-model)
    - [**Model 3: Linear Regression of Zillow House Price Index on Housing Inventory in the US**](#model-3-linear-regression-of-zillow-house-price-index-on-housing-inventory-in-the-us)
  - [Database](#database)
  - [Dashboard](#dashboard)
    - [Presentation](#presentation)
    - [Speaker Notes](#speaker-notes)
  - [Conclusion](#conclusion)
  - [Recommendations for Future Analysis](#recommendations-for-future-analysis)
  - [Techology Used](#techology-used)
  - [Team Members](#team-members)

## Introduction to the Project
### Selected Topic
The purpose of this project is to gauge the impact of in-country population migration in the U.S. on house prices. In addition, we will identify population migration patterns in the U.S. using various visualizations. We will also look for factors other than migration that could impact housing prices in the U.S. Our hypothesis is that population growth drives up real estate prices (and vice versa). 

### Underlying reason
We think population migration has wide ranging implications for the U.S population including economic, political, demographic and societal impact. Population migration could impact economic growth, cost of living, real estate prices, transportation, congressional redistricting, government budgets, demographic makeup, etc. In this analysis, we will show population migration patterns in the U.S. and how that has impacted housing prices. We will also look for factors other than migration that could impact housing prices in the U.S. We think drivers of residential real estate prices are broadly categorized into three groups:
	- Housing demand drivers: Population growth, GDP growth, population income and wealth
	- Housing supply : For sale inventory, housing production, housing unit growth
	- Capital market factors: Mortgage interest rate, stock prices
There are many other factors that could impact residential real estate prices but we will only test a handful of individual factors in this analysis.

### Description of the source of data

- The American Community Survey (ACS) done by the U.S. Census provides data on changes taking place in population and housing in the U.S. We are using county to county migration data from ACS during 2015-19. [County-to-County Migration Flows: 2015-2019 ACS](https://www.census.gov/data/tables/2019/demo/geographic-mobility/county-to-county-migration-2015-2019.html). 
	- The raw data is in this [text file](Resources/Net_Gross_US.txt)
- Estimated total population by county over 2010-19 from the US Census: [Total population by county 2010-19](https://www2.census.gov/programs-surveys/popest/datasets/2010-2019/counties/totals/co-est2019-alldata.csv)
	- The raw data is in this [csv file](Resources/County_population_totals_2010_2019_co-est2019-alldata.csv)
- Federal Housing Finance Agency (FHFA) House Price Index (HPI) is a broad measure of the movement of single-family house prices. The FHFA HPI is a weighted, repeat-sales index, meaning that it measures average price changes in repeat sales or refinancings on the same properties. FHFA provides HPI for all counties in the U.S. [FHFA HPI data from US Federal Reserve](https://geofred.stlouisfed.org/map/?th=ylgn&cc=5&rc=false&im=fractile&sb&lng=-100.239&lat=41.558&zm=5&sl&sv&sti=942&rt=county&at=Not%20Seasonally%20Adjusted,%20Annual,%20Index%202000%3D100,%20no_period_desc&fq=Annual&dt=2020-01-01&am=Average&un=lin) 
	- The raw data is in this [Excel file](Resources/GeoFRED_All-Transactions_House_Price_Index_by_County_Index.xlsx)
- Interest rate data: [30-Year Fixed Rate Mortgage Average in the United States](https://fred.stlouisfed.org/series/MORTGAGE30US)
	- The raw data is in this [CSV file](Resources/MORTGAGE30US.csv)
- "All-Transactions House Price Index for the United States" from Federal Housing Finance Agency (FHFA) is a broad measure of the movement of single-family house prices in the US. [All-Transactions House Price Index for the United States](https://fred.stlouisfed.org/series/USSTHPI)
	- 	The raw data is in this [CSV file](Resources/USSTHPI.csv)
- "Zillow Home Value Index (HVI)" is a smoothed, seasonally adjusted measure of the typical home value and market changes across a given region and housing type. 
[Zillow Home Value Index (HVI)](https://www.zillow.com/research/data/)
   - The raw data is in this [csv file](Resources/Zillow_Home_Value_Index_(ZHVI)_Metro_All_Homes_monthly.csv)
- "Zillow For-Sale Inventory" represents the count of unique listings that were active at any time in a given month [Zillow Home Value Index (HVI)](https://www.zillow.com/research/data/)
   - The raw data is in this [csv file](Resources/Zillow_Inventory_Metro_All_Homes_monthly.csv)


### Questions we hope to answer with the data
- What areas are people leaving and where are they going?
- What is the scale of migration over time?
- Which counties saw the highest and lowest % increase and decrease in migration? 
- When people move, are they staying in their current state or go to a different state?
- What is the impact of population change on residential real estate prices?
- How could you use population migration data and real estate prices to make home buying decision or planning your next move?
- What other factors drive real estate price (e.g. interest rates, income/wealth, residential construction activity)?
- What are some other implications of population migration (e.g. overall cost of living, congressional redistricting)?


## Data Exploration
We mentioned above in the data description section above, we had data in various format. We did Data Exploration in Pandas using Pandas, NumPy and Mitosheet.

1. **House Price Index** Data from [House Price Index for all counties (Excel file)](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/GeoFRED_All-Transactions_House_Price_Index_by_County_Index.xlsx) was transformed as below and output file in CSV format [Cleaned data - House Price Index for all counties](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/house_price_df.csv) was generated.

   - Removed unnecessary columns for house prices between 1975-1999,
   - Created a column real estate price change over 2015-2019 for all counties in the CSV file. The column was created in order to run a linear regression model regressing House Price Index of counties against net migration change in those counties during the same period, and
   - Exported a CSV file with cleaned house price data.

   Code base [Jupyter Notebook - House Price Index Data Clean Up](https://github.com/hishamdewan/Migration_and_Housing/blob/main/House_Price.ipynb).

The following screenshot shows a list of states that had counties with >40% housing price (i.e. HPI) increase over 2015-19.

![States_with_counties_with_highest_price_increase](Images/States_with_counties_with_highest_price_increase.png)


2. **Migration** Data from [County-to-County Migration Flows 2015-2019 ACS data (Text file)](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/Net_Gross_US.txt) was transformed as below and the following output file in CSV format [Cleaned data - County Level Migration as % of total population](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/county_level_migration_15-19.csv) was generated.
    
   - Removed unnecessary columns in the migration data,
   - Renamed columns to user friendly names and prepared the data for uploading to a relational database,
   - Reordered the columns,
   - Cleaned values in column "State Name of Geography A",
   - Created pivot table showing population change (i.e. number of people migrating and as % of total population), 
   - Created a column showing population change driven by migration over 2015-2019 for all counties. The column was created in order to run a linear regression model regressing House Price Index of counties against net migration change in those countries during the same period, and
   - Created two CSV files: one for ML model and the other to store in the database.
  
   Code base [Jupyter Notebook - Migration Flows Data Clean Up](https://github.com/hishamdewan/Migration_and_Housing/blob/main/population_change.ipynb).

The pivot table below shows population change by state and county:

![pivot_table_migration_by_state_county](Images/pivot_table_migration_by_state_county.png)
    

3. **Net Migration** Data from [County-to-County Migration Flows 2015-2019 Ins-Outs-Nets-Gross (Excel file)](https://github.com/Bhargavi-ng/Migration_and_Housing/blob/main/Resources/cleaned-county-to-county-2015-2019-ins-outs-nets-gross.xlsx ) was transformed as below and output file in CSV format [Cleaned data - County Level Migration by number of people and source county](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/cleaned-county-to-county-2015-2019-ins-outs-nets-gross.csv) was generated.
  
	  - Removed unnecessary columns in the migration data,
	  - Renamed and reordered the columns to user friendly names, and
	  - Generated new file in csv format.
      
   Code base [Jupyter Notebook - Migration Flows by County](https://github.com/hishamdewan/Migration_and_Housing/blob/main/cleaning_excel_file.ipynb).


4. **Mortgage Rates** Data source [30 year Mortgage Rates](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/MORTGAGE30US.csv) was cleaned and merged with [House Price Index](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/USSTHPI.csv) were transformed and merged before using in the Machine Learning Model 2 - HPI vs Mortgage Rate
	- Convert data from both files to "Quarterly" timeline to use in the model.
	
   Code base [Jupyter Notebook - Linear Regression vs Mortgage Rates](https://github.com/Bhargavi-ng/Migration_and_Housing/blob/main/linear_regression_HPI_vs_30MORTGAGE.ipynb).

5. **Current and Previous Residence** Data from [county-to-county-2015-2019-previous-residence-sort (Excel file)](/Resources/county-to-county-2015-2019-current-residence-sort.xlsx) was transformed as below and output file in CSV format [cleaned_2015_2019_current_previous_residence_data](Resources/cleaned_2015_2019_current_previous_residence_data.csv) was generated.
	  -  Cleaned data provided for each state in separate worksheet in the excel by iterating and doing the below
	     -  Converted multiple header rows into one header row and removed the extra rows across all the worksheets,
	     -  Removed unwanted columns, 
	     -  Removed footer notes rows in each worksheet,
	     -  Filled empty cells with '0', and 
	     -  Replaced '-' for Foreign movers with 'NA', 
	  - Combined data from all states,
	  - Added additional rows and filled values by joining 2 columns, and
	  - Generated csv file.
	Code base [Jupyter Notebook - Current and Previous Residence](https://github.com/Bhargavi-ng/Migration_and_Housing/blob/main/current_residence_county_CleanUp.ipynb).

6. **Zillow House Price Index and House Inventory** Data from [Zillow Inventory Metro All Homes Monthly (csv file)](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources\Zillow_Home_Value_Index_(ZHVI)_Metro_All_Homes_monthly.csv) was transformed as below and the following output file in CSV format [Cleaned data - Zillow HVI Inventory](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/Zillow_HVI_.csv) was generated.
    
   - Removed unnecessary columns
   - Renamed columns to user friendly names and prepared the data for merging 
   - reset the index to Date 
   - transpose the rows into columns for merging the two dataframes into one dataframe
   
  
   Code base [Jupyter Notebook - Zillow House Price Index Cleanup](https://github.com/Bhargavi-ng/Migration_and_Housing/blob/ML_Segment3/linear_regression_ZHPI_vs_inventory.ipynb).



## Data Analysis and creation of Machine Learning Models
We are using two machine learning model for this analysis. Both models use linear regression because it can be used to quantify and measure the variability of two correlated variables. Moreover, a linear regression model can be used to predict one variable using values of another variable.

### Model 1: House price and Migration
We created a linear regression model that regresses percentage house price changes for counties in the US over 2015-19 against population change in those countries during the same period. The linear regression model tests the hypothesis that population growth drives up real estate prices. The ML model takes data from the AWS Relational database. We will assess the quality of the model using R squared and other metrics. 

***Findings:***

The model does not support our hypothesis. The migration of population within U.S. was not a material driver of Housing prices as shown in the below screenshot. The R squared value for this model was ~ 2.7% meaning this model explains only 2.7% of the housing price change over 2015-2019. Thus, based on this model the hypothesis is rejected.

![Linear Regression Plot HPI vs Migration](/Images/Linear_Regression_HPI_vs_Migration.png)

***Data used for the model:*** 

1. **House Price Index** Data - [Cleaned data - House Price Index for all counties](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/house_price_df.csv) was generated.

2. **Migration** Data - [Cleaned data - County Level Migration as % of total population](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/county_level_migration_15-19.csv)
  
3. **Net Migration** [Cleaned data - County Level Migration by number of people and source county](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/cleaned-county-to-county-2015-2019-ins-outs-nets-gross.csv).
      
***Database Integration***

We used AWS Relational Database to store the data needed for this model. A connection was established using Postgres and SQLAlchemy to pull the data needed to run the model as shown in the below screenshot. You can find ERD and more details about the implementation below in the Database section.

![Code snippet  - showing connection to AWS](/Images/ML_AWS_Connection.PNG)

***Training and Testing sets:***

The dataset was split into training and testing sets for the model using SKlearn as shown below.

![HPI vs Migration splitting Datasets for Training and Testing](/Images/HPI_Migration_Split_Datasets.PNG)

***Model Quality:***

This model has a mean squared error value of nearly zero (0.02), which represents a perfect model. We can say that the quality of the model is very good. 

![HPI vs Population Migration Model Quality Evaluation](/Images/HPI_Migration_Model_Quality.PNG)

***Model Limitations:***
	- This linear regression model had only one independent variable - Population Migration with in U.S. Real estate prices have many other variables influencing it like population growth, housing unit growth, economic state of the country, etc.
	- We considered data for a smaller duration - 2015 to 2019 for this model.
	- Also, the geography could potentially be too large: - all counties across U.S. Limiting the data to high migration counties might have provided better output.
	
	
Code base [Jupyter Notebook - Linear Regression model regressing house price and migration](https://github.com/hishamdewan/Migration_and_Housing/blob/main/linear_regression_HPI_vs_pop.ipynb).


### **Model 2: House Price Index and 30 year Fixed Mortagage Rates**
We used a second linear regression model that will regress "**All-Transactions House Price Index for the United States**" on "**30-Year Fixed Rate Mortgage Average in the United States**" between 1975-2021. This model tests the hypothesis that the interest rates drove housing prices in the U.S. We assessed the quality of the model using R squared and other metrics. 

***Findings:***

The model supports our hypothesis that change in interest rate drives housing prices in U.S. as shown below. This model has a R squared value of 0.682 meaning that falling interest rates explains 68.2% of the change in house prices. There are other factors influencing the remaining of the change in house prices.

![Linear Regression Plot HPI vs Mortage Rate](/Images/linear_regression_HPI_vs_MORTGAGE.png)

***Data used:***

Data sources [30 year Mortgage Rates](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/MORTGAGE30US.csv) and [House Price Index](https://github.com/hishamdewan/Migration_and_Housing/blob/main/Resources/USSTHPI.csv) were transformed and merged before using in the model.

***Training and Testing sets:***

We used SKlearn to split the above processed data into training and testing sets as shown below screenshot.

![HPI vs Mortgage splitting Datasets for Training and Testing](/Images/HPI_Mortgage_Split_Datasets.PNG)

***Model Quality:***

![HPI vs Mortgage Model Quality Evaluation](/Images/HPI_Mortgage_Model_Quality.PNG)

This model has a mean squared error value. This is because we did not scale the dataset as it was very small and overfit the model.

***Model Limitations:***
	- We have taken into account only one independent variable (Mortgage rates) in this model. As discussed earlier, real estate prices have many other variables influencing it.
	- We considered data for a longer duration over bigger geographic spread. As in, Interest rate over the past 40 years versus House Price Index change across all of U.S.
	
Code base [Jupyter Notebook - Linear Regression model regressing House price and Mortgage rate](https://github.com/hishamdewan/Migration_and_Housing/blob/main/linear_regression_HPI_vs_30MORTGAGE.ipynb)

#### Neural Network Model
As the above Model 2: House Price Index vs Mortgage Rates was a good model, we created a neural network model using to House Price Index and 30 year fixed Mortgage data to explore the relationship further. But, we did not have sufficient data for the neural network model and thus this model wasn't able to yield meaningful results.

Code base [Jupyter Notebook - Neural Network Model House price and Mortgage rate](https://github.com/hishamdewan/Migration_and_Housing/blob/main/neural_network_HPI_vs_30MORTGAGE.ipynb)

The following is a screenshot of the accuracy performance of our Neural Network Model: 
![Neural Network Model Accuracy](https://github.com/Bhargavi-ng/Migration_and_Housing/blob/main/Images/neural_network_model_accuracy.png) 

### **Model 3: Linear Regression of Zillow House Price Index on Housing Inventory in the US**
We used a third linear regression model that will regress "**Zillow House Value Index**" on "**Zillow For Sale Housing Inventory**" between 2018-2021. This model tests the hypothesis that the housing inventory can drive housing prices in the U.S. We assessed the quality of the model using R squared and other metrics. 

***Findings:***

The model supports our hypothesis that housing inventory drives housing prices in U.S. as shown below. This model has a R squared value of 0.681 meaning that housing inventory explains 68.1% of the change in house prices. This is in addition to the other factors influencing the remaining of the change in house prices.

![Linear Regression Plot ZHVI vs Housing Inventory](/Images/linear_regression_ZHVI_vs_Inventory.PNG)

***Data used:***

Data sources [Zillow Home Value Index (HVI)](https://github.com/Bhargavi-ng/Migration_and_Housing/Resources/Zillow_Home_Value_Index_(ZHVI)_Metro_All_Homes_monthly.csv) and [Zillow For-Sale Inventory](https://github.com/Bhargavi-ng/Migration_and_Housing/Resources/Zillow_Inventory_Metro_All_Homes_monthly.csv) were transformed and merged before using in the model.


***Model Quality:***

![ZHVI vs Inventory Model Quality Evaluation](/Images/ZHVI_vs_Inventory_Model_Quality.png)

This model has a very high mean squared error value. This is because we did not reshape the datasets as they were in a fairly clean format.

***Model Limitations:***
	- This linear regression model only has one independent variable - Housing Inventory. As discussed earlier, real estate prices have many other variables influencing it.
	- The dataset for this model is limited because the duration of the data is only from 2018-2021. Zillow didn't have long term series data for housing inventory. 
	
Code base [Jupyter Notebook - Linear Regression model regressing House price and Housing Inventory](https://github.com/hishamdewan/Migration_and_Housing/blob/main/linear_regression_ZHPI_vs_inventory.ipynb)


## Database
PostgreSQL database in AWS RDS is used to store data used for this project. The static data from FHFA HPI for each county and population migration by county data were preprocessed and loaded into Postgres database in AWS RDS. Those two data sources with merged in AWS using SQL and prepared to be consumed for Machine Learning model.

The following is a screenshot of the entity relations diagram (ERD) we used for the analysis:

![ERD DIAGRAM](https://github.com/Bhargavi-ng/Migration_and_Housing/blob/main/Images/ERD.png)

For the security purposes, sensitive information needed to connect to AWS RDS is saved on config.py file and called in following files to upload and download data.
* [Migration_data_upload.ipynb](https://github.com/Bhargavi-ng/Migration_and_Housing/blob/main/Migration_data_upload.ipynb)
* [loading_house_price_data_to_RDS.ipynb](https://github.com/Bhargavi-ng/Migration_and_Housing/blob/main/loading_house_price_data_to_RDS.ipynb)
* [linear_regression_HPI_vs_pop.ipynb](https://github.com/Bhargavi-ng/Migration_and_Housing/blob/main/linear_regression_HPI_vs_pop.ipynb)

The following is a screenshot of MIGRATIONS_IN_OUTS_NETS_GROSS data table that stores processed
County-to-County Migration Flows 2015-2019 ACS data in the database:

![MIGRATIONS_IN_OUTS_NETS_GROSS table](/Images/MIGRATION_IN_OUTS_NETS_GROSS_data_table.png)

The following is a screenshot of HOUSE_PRICE data table that stores processed FHFA HPI for each county in the database:

![MIGRATIONS_IN_OUTS_NETS_GROSS table](/Images/HOUSE_PRICE_data_table.png)

Below is the SQL query used to join MIGRATIONS_IN_OUTS_NETS_GROSS and HOUSE_PRICE.
```
SELECT
	 B.state_name_of_geography_a,
    B.county_name_of_geography_a,
    B.state_and_county_of_geography_a,
    A.REGION_CODE AS REGION_CODE_geography_a,
    B.state_us_island_area_foreign_region_of_geography_b,
    B.county_name_of_geography_b,
    B.state_and_county_of_geography_b,
    B.flow_from_geography_b_to_geography_a,
    B.counterflow_from_geography_a_to_geography_b, 
    B.net_migration_from_geography_b_to_geography_a1,
    B.gross_migration_between_geography_a_and_geography_b1,
    A."2000" ,A."2001" ,A."2002" ,A."2003" ,A."2004" ,A."2005",
    A."2006" ,A."2007" ,A."2008" ,A."2009",A."2010" ,A."2011",
    A."2012",A."2013" ,A."2014" ,A."2015" ,A."2016",A."2017",
    A."2018",A."2019" ,A."2020",
    A.percent_price_change_2015_19
INTO house_price_migration_in_out
FROM house_price A
INNER JOIN migration_in_outs_nets_gross B
	ON A.state_and_county = B.state_and_county_of_geography_a;
```
The data from house_price_migration_in_out is utilized to run Linear Regression on house price and migration.


## Dashboard
We are using Tableau to create the graphs and charts necessary for visualization. The "Dashboard" and "Story" features of the Tableau will help us with story telling when the time comes for the presentation.
Here is the link to our [Tableau Workbook](https://public.tableau.com/app/profile/hisham.dewan/viz/Migration_flows_Book2/Story-MigrationHousing)

The interactive elements we used for creation of graphs and charts are
- Population migration within U.S.A
  - County level
  - State level
- House Price Index
- Population growth
- Mortgage Interest Rate

Screenshots of visualizations we created are given below.

**Migration Flows - Net Migration for California:**

The **migration flow out** of the state of California is shown in **Red** and **migration into** the state of California is shown in **Green**. As you can see in the image below that between 2015 to 2019 the top five states to which the Californians moved to are Texas(34,571), Arizona(29,773), Nevada(24,180), Oregon(22,517) and Washington(17,220). Except for Texas, the remaining states are the neighboring states. Whereas, the top five states from were people moved to California are New York(11,592), Illinois(7,876), New Jersey(4,604), Massachusetts(3,301) and Pennsylvania(2,476)

![Net Migration for California](/Images/Visual1_Net_Migration_California.PNG)

**Ranking Net Migration - State level:**

The **Blue** color means that the **Net Migration is Positive** (Incoming population > Outgoing population) and **Red** color means **Net Migration is Negative** (Incoming population < Outgoing population). We can see that there are five states with Positive Net Migration of 50,000 or more. They are Florida, Texas, Arizona, North Carolina and Washington. Similarly, there are four states with Negative Net Migration of 50,000 or more. They are New York, California, Illinois and New Jersey.

![Ranking Net Migration - State level](/Images/Visual2_Ranking_Net_Migration_State_level.PNG)


### Presentation
The outline for the presentation has been created as part of Segement2 Deliverable. Here is the [link](https://docs.google.com/presentation/d/1ZMPZ4rW7OaxI3ja43FWr-0ZHVNi6gMHT/edit#slide=id.p1)

### Speaker Notes
The link to document with speaker notes can be found [here](https://docs.google.com/document/d/1EwQFBvosfMiQ6xSrpvW4Zg9VOj4eTRjYihngX7ESDzs/edit#)


## Conclusion
If you are deciding on purchasing residential real estate for either personal or for investment, we recommend that you should focus on supply side drivers such as inventory and capital market driver like Interest rate than demand driver like migration based on our analysis above.


## Recommendations for Future Analysis
For this project, to keep our scope small we considered only one driver/variable at a time for the analysis. We can improve this by looking at other variables like how we used Mortgage Rates when Population Migration did not answer our questions. Some of the variables we can take into account are - wealth and income, construction activity, and housing unit growth. Additionally, we can look at multiple variables at the same time using multiple linear regression models or neural network models. 


## Techology Used 
- Data Clean up: Jupyter Notebook, Python, Pandas Library, Mito, SQLAlchemy
- Database: PostgresSQL, Amazon Relational Database Service
- Machine Learning: SKlearn (Linear Regression, train_test_split, mean_squared_error, r2_score, etc.) 
- Data Visualization: Tableau, Plotly, Matplotlib

## Team Members
Contributor are listed [here](https://github.com/Bhargavi-ng/Migration_and_Housing/graphs/contributors)
1. Hisham Dewan
2. Shirley Liu
3. Zhen Fung
4. Bhargavi Nagarajappa
5. Teruki Ito
6. Merina Kansakar
