# Life Expectancy and Fertility Rates among Nations

## Introduction

Both life expectancy and fertility are metrics that are used as benchmarks to evaluate the development of a country as well as the strength of their healthcare system. It is in the interest of nations to determine what factors have the largest impact on these statistics if they would like a efficiently allocate thier resources to improve them. This data science project aims to find these factors using multiple regression analysis techniques.

## Data

The dataset used for this analysis was published and made available on the Data Science website [Kaggle](https://www.kaggle.com/), and aggregated by Kacper Kalinowski. It is available [here](https://www.kaggle.com/kacperk77/life-expectancy). The data originally was collected from https://data.worldbank.org/ and is reflective of metrics taken in 2014 across 138 countries.

### Datasets

This overall dataset consists of two overlapping datasets:
•	Dataset 1: 63 countries, 20 variables, and 17 input variables.
•	Dataset 2: 138 countries, 13 variables, and 10 input variables

### Variables

•	**Fertilityrate** (Fertility rate, total (births per woman) 2014)
•	**Lifeexp** (Life expectancy at birth, total (years) 2014)
•	Country (String description of country)
•	*Literacyrate* (Literacy rate, adult total (% of people ages 15 and above) 2014)
•	*Homicidiesper100k* (Homicidies per 100k people 2014)
•	*Electricity* (Electric power consumption (kWh per capita) 2014)
•	Schooling (Ave number of years of Schooling (years) 2014)
•	HIV.AIDS (Deaths per 1 000 live births HIV/AIDS (0-4 years) 2014)
•	Status (Economic development status of country)
•	Wateraccess (Access to improved water sources (% of total population with access 2014)
•	Tuberculosis (Incidence of tuberculosis per 100,000 people 2014)
•	Inflation (Inflation, consumer prices (annual %) 2014)
•	Healthexppercapita (Average health expenditure per capita, PPP 2005-2014)
•	*Internet* (Individuals using the Internet (% of population) 2014)
•	*Gdppercapita* (GDP per capita, PPP (current international $) 2014)
•	CO2 (Average CO2 emissions (metric tons per capita) 2005-2014)
•	*Forest* (Forest area (% of land area) 2014)
•	*Urbanpop* (Urban population 2014)
•	Urbanpopgrowth (Average urban population growth (annual %) 2005-2014)
•	Leastdeveloped (1 = country is considered as least developed; 0 = country is considered as developing or developed)

The features that are *italicized* are features that only exist in Dataset 1, the dataset with 20 columns, but not in Dataset 2, the set with more countries. Output variables are **bolded**.

### Preprocessing

Datasets were not missing any values, but did require some partitioning and changes to be prepared for regression. ‘Leastdeveloped’ and ‘Status’ were redundant categorical variables that contained no objective numerical information. Since the status of a country’s development carries no real word input that could rathe be thought of as a categorical output, these two features were removed from the analysis. Additionally, ‘Country’ was removed as it was a string that carried no relevant information for the regression.  
The remaining input variables were all continuous, however they had large variance in mean values, so all variables were standardized. This can be useful in determining the most impactful factors in a regression, as the largest coefficients will then indicate the most significant features (as long as p-value thresholds are met).

### Dataset Selction

Before running all models on a dataset for both outputs, it was important to pick a dataset between the two available that would best set up future models for success. To do this, I ran a simple *OLS Regression* using [statsmodel](https://www.statsmodels.org/stable/index.html) to determine if there was any significant information gain to be had by including the additional variables available in Dataset 1. Here a “significant factor” is a factor that has a p-value of < 0.06.

While the R-Squared score and the AIC/BIC is better for Dataset 1, Dataset 2 finds just as many or more “significant factors” than Dataset 1 which has more features. To avoid noise due to insignificant features, I chose to work with Dataset 2 as it has more countries which translates to more significant data overall.

## Model Selection
### Life Expectancy
Dataset 2 was split into Train and Test sets for model selection, with 20% of data stored in test. Out of the 5 models ran, Random Forest and XGBoost Regressors had the highest R-Squared scores and lowest Mean Squared Errors. These models agreed that ‘HIV.AIDS’ and ‘Healthexppercapita’ were important features, but disagreed on some others.

#### Random Forest
![image](https://user-images.githubusercontent.com/60637235/145888172-7d81839e-0995-4ac3-87fa-08d7773ca80d.png)
![image](https://user-images.githubusercontent.com/60637235/145888185-973339ca-fda6-4f36-9f70-39137f63a6e7.png)

#### XGBoost
![image](https://user-images.githubusercontent.com/60637235/145888208-1cd82173-cbfc-4ad2-975b-b47cdd2f75c6.png)
![image](https://user-images.githubusercontent.com/60637235/145888217-cfff6eb3-b584-485b-851f-bbead9bb0c7b.png)

### Fertility Rates
Out of the 5 models ran, Random Forest and RidgeCV Regressors had the highest R-Squared scores and lowest Mean Squared Errors.

#### Ridge CV
![image](https://user-images.githubusercontent.com/60637235/145888666-dea420af-68c3-4813-8032-7307648e0b80.png)
![image](https://user-images.githubusercontent.com/60637235/145888670-bc938105-7e45-4fe5-8c5f-ff1866379db3.png)

#### Random Forrest
![image](https://user-images.githubusercontent.com/60637235/145888678-bbd88e31-7b0e-4948-9190-24b5d9264b61.png)
![image](https://user-images.githubusercontent.com/60637235/145888684-dcb7ad05-0d31-4dfa-9bcb-1a768e8a0d8b.png)

## Conclusion
### Life Expectancy

Based on the models generated a country would be wise to focus on increasing health expenditure for each of its citizens (‘healthexppercapita’) and lowering the death rates from HIV or aids (‘HIV.AIDS’). According to the *Random Forest Model*, increasing the average number of years of schooling (‘Schooling’) and decreasing the rates of tuberculosis (‘tuberculosis’) are also significant factors, and according to the *XGBoost Model* improving water access (‘wateraccess’) is also important in increasing life expectancy. Both these models had a high R-squared score of 0.92 or higher, with the *Random Forest Model* having the best overall model evaluation metrics.

### Fertility Rates

Countries looking to lower the rate of births would want to improve water access (‘wateraccess’) and increase the average number of years of schooling (‘Schooling’). The R-squared score is 0.76 for the Random Forest and 0.70 for the Ridge CV model, so while these models are useful they are not as accurate as the life expectancy models.

### Model Evaluation

The *Random Forrest Model* proved to be the best suited model for this dataset, scoring the highest R-squared scores and lowest MSE for both Fertility Rate and Life Expectancy outputs. Due to the potential Heteroscedasticity of this dataset these models are likely not perfect, however given the high R-squared scores I would consider these to be accurate and useful evaluations overall.

#### Note

It is worth noting that this dataset is not nearly large enough to reap the full benefits of ML regression analysis. While the evaluation metrics were great for the life expectancy models and good for the fertility rates, it is highly likely that another year or several more years of data would have a large impact on feature importance and coefficients, so this should be considered when evaluating these models.
