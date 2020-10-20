# Capstone Project on COVID-19

## Overview
Our group (Martin Kaminskyj, Sami Elkhayri, Muzznah Ansari, Femi Adeleke, and Patricia Lan) chose to perform data analysis on coronavirus disease of 2019 (COVID-19). The topic of COVID-19 was chosen because it is a current, global issue with extensive socioeconomic impact, and public data is available. Thus, many approaches to analysis are possible. 

This project evaluates government measures to contain the spread of COVID-19, starting from January 1, 2020 to August 31, 2020. The dataset from Oxford university's Blavatnik School of Government contained indices that categorized government responses to COVID-19, Google Community Mobility Reports recorded changes in people's mobility, and Our World in Data provided medical data on COVID-19 (e.g. number of confirmed cases and deaths).

Questions the project hoped to answer were the following: 
1) Are there general correlations between feature vs. target variables?
2) Government measures:
    a) Which measures resulted in desirable outcomes?
    b) Which stringency measures are associated with lower rate of increase of total cases? Total deaths?
3) Can an unsupervised machine learning (ML) model group countries by all variables to create meaningful group profiles? (Do group profiles correlate with desirable outcomes?)
4) Supervised machine learning (ML):
    a) Can a model accurately predict future total cases/deaths based on feature variables?
    b) Which feature variables contribute the most to the model?
5)  High Risk Continent: Which continent poses the most risk  based on age variation, total cases and extreme poverty rating.

## Analysis
View the [Google Slides presentation](https://docs.google.com/presentation/d/1sH4ivyYJxLH2zuCJpQTzjlnWvE8RgMRwBxGjeOCTDVo/edit?usp=sharing) for descriptions of the data exploration and analysis. 

To view the code used to find correlations between feature and target variables, and to create an unsupervised ML model, visit the [analysis_correlations_unsupervisedML directory](analysis_correlations_unsupervisedML). Jupyter notebooks are numbered from 1 to 11, which follows the progression of the analysis. Except for files containing raw data (see "Resources" below), the CSV files in the folder are intermediate files necessary for the progression of the analysis. Image files are byproducts of the notebooks and were used in [Google Slides presentation](https://docs.google.com/presentation/d/1sH4ivyYJxLH2zuCJpQTzjlnWvE8RgMRwBxGjeOCTDVo/edit?usp=sharing).

To view the code used to analyse the correlation between the timing and severity of each countries initial stringency efforts and their outcome of total cases and deaths as a percentage of population, visit the [analysis_gov_regulation_impact directory](analysis_gov_regulation_impact).

To view the code used to determine the continent that poses the highest risk based on age variation, total cases and poverty level, visit the [analysis_median_age_per_continent directory](analysis_median_age_per_continent). 

## Database 
Data was stored in a SQLite database ([covid_db.db](analysis_gov_regulation_impact/Resources/covid_db.db)) to be queried for input into machine learning models. Figure 1 shows an entity relationship diagram (ERD) of the database.

#### Figure 1. Database ERD
![](ERD_final.png)

## Machine Learning
Supervised machine learning(ML) models were created to predict target variables (total cases and total deaths) based on the following feature variables :
- Population
- Population density
- Median age
- Current total cases
- Current total death
- School Closing
- Workplace Closing
- Cancel public events
- Restriction on gatherings
- Close public transport
- Stay at home requirement
- Restriction on internal movement
- International travel controls
- Public information Campaigns
- Income support

Models included neural network, Support Vector Regression (SVR), and Random Forest Regressor. R-squared values were used to assess for the strength of correlations between feature and target variables. Model accuracy was assessed by comparing R-squared values between training and testing sets.

### Data Preprocessing
For selection and preprocessing of features, their data types and correlations were checked. Once the features were selected, both the feature variables, and targets were scaled. 

### Feature Selection
Feature selection was an iterative process whereby the initial selected features were adjusted according to the outputted feature importance /contribution by the RandomForestRegressor model. For more detail on the decision-making process check the following notebooks:
1. [FeatureSelection_FeatureImportance_totalcases.ipynb](analysis_gov_regulation_impact/FeatureSelection_FeatureImportance_totalcases.ipynb)
2. [FeatureSelection_FeatureImportance_totaldeaths.ipynb](analysis_gov_regulation_impact/FeatureSelection_FeatureImportance_totaldeaths.ipynb)
   The above notebooks helped us determine the best features and model for further analysis.

3. [HealthEco_Component_Feature_Importance.ipynb](analysis_gov_regulation_impact/HealthEco_Component_Feature_Importance.ipynb)
4. [Stringency_Component_Feature_Importance.ipynb](analysis_gov_regulation_impact/Stringency_Component_Feature_Importance.ipynb)
The above notebooks were used to determine the importance of the individual components of the Stringency Index, HealthSupport Index, and EconomicSupport Index. Based on the findings, stay at home requirement, a restriction on internal movement and international travel control from the Stringency Index, in addition to income support (EconomicSupport Index) were the most significant features for the selected ML model.

### Feature Engineering
The original dataset contained, total deaths and total cases for a given date. To predict future total cases and deaths, 12 new columns were created, three each for 30,45,60 and 75 days out. The columns were date plus a time-delta e.g. 30 days, total deaths at 30 days and total cases at 30 days. For coding detail check [ML Notebook]( analysis_gov_regulation_impact/Stringency_Component_total_cases_deaths.ipynb)

### Training and Testing Set
The 95% of the data was used for training and 5% of the data was kept for testing purposes. For details check [ML Notebook]( analysis_gov_regulation_impact/Stringency_Component_total_cases_deaths.ipynb)

### Choice of Model
The models tried were, SVR, RandomForestRegressor and Neural Networks. Out of the three we decided to use RandomForestRegressor for understanding feature contribution/importance and Neural Networks for predicting future target values.

### Reasons

#### Benefits
* High R2 values
* Ability to extrapolate from a given dataset
* Neural Network is a flexible model that adapts itself to the shape of the data and can dynamically pick the best regression (i.e. logistic, linear, polynomial etc.)  for a particular data.

#### Limitations
* Neural Networks require specialized software to run and are computationally intensive.
* Black box: It does not explain how the results were achieved.

## Software
See [Technology.md](Technology.md) for a description of the technologies, languages, tools, and algorithms used in this project. [Requirements.txt](Requirements.txt) contains a list of the packages/modules/libraries needed to run the Jupyter Notebooks (in "analysis_gov_regulation_impact") for the supervised machine learning model. 

## Dashboard
[Click here](https://pmmfs.github.io/) to view the dashboard. Files used to create the dashboard are available in the [dashboard repo](https://github.com/UofT-DataBootcamp-Coursework/pmmfs.github.io). 

## Resources

### Data
- [regional_mobility.csv](analysis_correlations_unsupervisedML/regional_mobility.csv) (Google Community Mobility Reports)
- [owid-covid-data(Aug31,2020).csv](analysis_gov_regulation_impact/Resources/raw/owid-covid-data(Aug31,2020).csv) (Our World in Data - COVID-19 database)
- [OxCGRT_latest(Aug31,2020).csv](analysis_gov_regulation_impact/Resources/raw/OxCGRT_latest(Aug31,2020).csv) (University of Oxford - COVID-19 Government Response Tracker)

### Software
See [Technology.md](Technology.md) and [Requirements.txt](Requirements.txt).

### Other
- [The dashboard](https://pmmfs.github.io/)
- [Dashboard repo](https://github.com/UofT-DataBootcamp-Coursework/pmmfs.github.io) (Contains files used to create the dashboard.)
- [Google Slides presentation](https://docs.google.com/presentation/d/1sH4ivyYJxLH2zuCJpQTzjlnWvE8RgMRwBxGjeOCTDVo/edit?usp=sharing) (Provides detailed information about the project.)
