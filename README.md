# California WildFires

by Taravat Sharafat, Dhivya Ram, Shailee Madhani,  Anupriya Sarkar

## Project Outline

Researchers found that forest soils recover from disturbances slowly over many years — up to 80 years following a wildfire. California has lost over 4000 acres of forest land over a period of 7 years. Considering this alarming data, analyzing the trend and impacts of forest fire is very intriguing.

Problem statement is to understand how are weather conditions contributing to the increase in wildfires and how human negligence can lead to a great deal of harm to the nature. Our project provides a detailed analysis of all these factors using wildfire dataset that combines historical wildfire occurrences with relevant features extracted. This project is scalable to expand the research for other states that pose a threat of wildfires.

![pexels-sippakorn-yamkasikorn-3552472](https://user-images.githubusercontent.com/83181834/135688533-716eb442-3fef-4755-8795-e0adc67e67e8.jpg)

## Resources

- **Data Source :**
	1. [Kaggle data source](https://www.kaggle.com/ananthu017/california-wildfire-incidents-20132020)
	2. [CA gov data source](https://gis.data.ca.gov/datasets/CALFIRE-Forestry::recent-large-fire-perimeters-5000-acres/about)
	3. [Weather data](https://www.worldweatheronline.com/developer/api/)
	4. [Geographical coordinates](https://simplemaps.com/data/us-counties)

- **Data Analysis :** Pandas
- **Database :** PostgresSQL (Database name : wildfire_db)
- **Visualization :** Tableau, Seaborn Library

## Data Description

In our dataset, there are 1,636 data points which are the fire occurrences in years 2013 through 2020 for California. Each data point has Archive year, Fire name, Acres Burned, County name as primary incident indicators. Additional features of the dataset includes latitude, longitude, Structures Damaged, Injuries etc. 

**Data Example**

![image](https://user-images.githubusercontent.com/83181834/135734160-24242aa7-ac85-4a92-b402-ef61569395a8.png)


## Exploratory Data Analysis

### Data Preprocessing

**1. Data load :** We have used Python pandas to load data from the following sources into two different data frames. We have retrieved weather and other geographical data through API to get more information about the past weather conditions , so that we can make analyse and make better predictions.			   	
   
**2. Removal of data duplication :** We have consolidated the data from various sources by removing duplicates to maintain accuracy and to avoid misleading statistics. 

**3. Handling null values :** To maintain performance and accuracy of ML model we have replaced/dropped the null values with some appropriate values based on their column type.

**4. Treat missing values :** Filled in some missing values required for our analysis using weather API.

**5. Merge/Join Dataset :** We have combined data frames using pandas merge function based on common columns so that we can incorporate additional data and information in our final
                        dataset.
						
**6. Statistical Summary:** We have generated a descriptive statistics of our final data frame showing the statistical details like count, mean, standard deviation, min, max and
                        quartiles.
						
![image](https://user-images.githubusercontent.com/83181834/135734201-fdf76f8e-8db4-4122-bebd-d6c4b6a73467.png)

	
### Data Loading

We have chosen PostgreSQl relational database system to store data for our project. We are using psycopg2 as the adapter to connect the database with our Python code and using SQLAlchemy which is a Python SQL toolkit to facilitate the communication between pandas and the database.

Our database is named Wildfire_db that stores the static data in four different tables, for our use during the course of the project. 

Below is the entity relation diagrams, showing the relationship between the four tables and their columns:

![image](https://user-images.githubusercontent.com/83181834/135734262-b325dadb-1611-4701-bdd1-a78d4f84192f.png)


### Data Analysis

#### Number of fires per month vs Max Temperature

We are analyzing if the temperature of California is contributing more to the wildfire occurrences. As per below scatter plot, we have more wildfires in CA from June - August which is one of the hottest month. (Size of scatter = Number of fires)

<img width="348" alt="monthVsMaxTemp" src="https://user-images.githubusercontent.com/83181834/135734392-2de33780-c3a3-4ac4-acae-58ca1956e119.PNG">

#### Number of fires per month vs Precipitation

In this analysis, we wanted to find relation between lack of rain (precipitation) to wildfire occurrence. As per below plot, more fires occurred during the dry months when the precipitation is very low. (Size of scatter = Number of fires)

<img width="344" alt="monthVsPrecipitation" src="https://user-images.githubusercontent.com/83181834/135734442-89799a8a-4471-418d-acb6-9aeab77176a0.PNG">

#### Weather data - Box Plot

Here we are plotting important weather features using box plot to understand the weather pattern of CA and its impacts on Wildfires. Outliers are expected because of extreme weather conditions throughout the year in CA but overall the mean values are looking great. 

<img width="332" alt="boxPlot" src="https://user-images.githubusercontent.com/83181834/135734491-78e7d271-bd34-44c5-b20b-92788dcc7952.PNG">

						
## Machine Learning Predictive Analysis

### Machine Learning  - Selection of features and targets

As part of predictive analysis, our Machine learning model will predict whether a major fire can take place in the state of California based on weather conditions. Since we are using history data of 2013-2019 CA fires, we have trained our model to predict whether a major fire can occur. Below chart plots the number of wild fires happened per year which ultimately contribute to our predictive modeling

![Major_fires_per_month](https://user-images.githubusercontent.com/83181834/136138076-d26a0e0e-a13b-4724-84c4-ca1f059eceec.png)


**1. Model Choice :**

We have selected the Logistic Regression method to predict the major fire which is mainly dependent on the past weather parameters. Here are some of the statistical summary from ML model. 

**Reason for model selection:** 

* Our dependent variable is binary and the observation are independent of each other
* Logistic regression helps predicting the likelihood of events by looking at historical data points and is the best model for binary classification.
	
**2. ML features** 

* Feature Selection : Month of fire, Maximum temperature, Wind Speed, Precipitation
* Target : Major fire (Based on our analysis of fire data for all the months in the years 2013-2019 a major fire took place when the maximum temperature rose to **32°C** and more than **1000** acres were burned.)
* Encoding : Using label encoder, converting features to a numeric machine-readable form.

Below S curve shows the major fire relation wrt Maximum temperature

![2](https://user-images.githubusercontent.com/83181834/136060121-85e42e0d-dc9d-47b5-9366-3b08ceb583d2.PNG)

**3. Splitting of data into training and testing sets**

Using **Scikit-learn train_test_split method** we have split the data into training and testing data. Random state 42 has been used to produce same results across different runs along with the stratify parameter so that the training and test subsets that have the same proportions of class labels as the input dataset.


### Machine Learning - Results

Once we split the data into training and testing sets, we train the model and calculate predictions. Below are some of the results of machine learning.

**Accuracy :** Using month of fire and weather features,the model predicts future major fire with an accuracy of 71%

![image](https://user-images.githubusercontent.com/83181834/136060517-ae7b3219-a2a3-4b49-90fc-390baa0dea35.png)

**Classification report:** Precision and recall values for "0" results are very good, although precision for "1" is small the recall value is 81%

![image](https://user-images.githubusercontent.com/83181834/136060577-528accb5-a77d-42bf-94e0-a8b2169e53ce.png)


**Confusion Matrix:** We plotted our confusion matrix, overall it looks good except we have 58 major fire predicted as not major fire but it may be due to skewed weather data of California.

![1](https://user-images.githubusercontent.com/83181834/136060184-395ff308-f594-40d0-b6b8-88574dd840f2.PNG)

**Correlation Heatmap:** We also plotted correlation heat map for all the features to find the relation.

![5](https://user-images.githubusercontent.com/83181834/136060160-d69b693f-36ba-48fe-b4ef-c95afe4fd054.PNG)


### Logistics Regression Summary

**Benefits of Logistics Regression**
   
   - The logistic regression model is simple to understand, easy to implement, and efficient to train
   - Provides good accuracy for smaller datasets
   - Less prone to over fitting in low dimensional datasets
   - Can be extended to multi-class classification
   - It makes no assumptions about distributions of classes 
   
**Limitations**
   
   - We have less data points in our dataset as the counties where fire took place is only restricted to California. Later when we expand this to other  counties for other states,this limitation of a smaller dataset can be overcome.
   - The logistic regression will not be able to handle a large number of categorical features.
   - It is difficult to capture complex relationships using logistic regression model.
   - Non linear problems can't be solved with logistic regression since it has a linear decision surface.
   

## Dashboard and Visualization

### Seaborn - Python Library
We used Seaborn python library to depict relationship between our variables during Exploratory data analysis and Machine learning phase. All those charts helps during the analysis of the data and deciding machine learning model. 
 
### Tableau
We have used Tableau, a powerful visualization tool to showcase our findings using various charts , interactive design and dashboard story telling. 

**Dashboard of California Wildfires**

All the charts in this Dashboard are interactive with each other through common filter on YEAR. Each report included tool tips for more information on CA Wildfires.

![image](https://user-images.githubusercontent.com/83181834/135735443-1f64c7ae-fb82-424a-a3ff-276c4279cb37.png)


**California Wildfire Map**

California map is plotted based on latitude and longitude from data points and showing County name and total acres burned. Counties with acres burned are color coded.

![image](https://user-images.githubusercontent.com/83181834/135735556-90bcf382-712d-41db-8a1e-4b092191d173.png)


**Cause of Fire**

This is the simple map showing Causes of Fire contributing of Wildfires in last 7 years. "Unidentified" is the major reason of wildfire. 

![image](https://user-images.githubusercontent.com/83181834/135735546-1fb7a7cf-0d05-43f6-ae66-65b233b528ae.png)


**Maximum Temperature per month**

Bar chart is the best way to explain relation between two attributes - This bar chart is plotted against Fire started Month vs Maximum temperature. As expected from our previous analysis, Jun - Aug mark more number of fires. 

![3](https://user-images.githubusercontent.com/83181834/136060001-7dbe070b-9866-401d-8eae-4c41e8aa7559.PNG)


**Top Counties by Total Acres Burned**

This interactive Do-nut chart shows the top counties based on total acres burned every year.
 
![image](https://user-images.githubusercontent.com/83181834/135735990-5eb3163c-1181-4a78-aeb2-8e3f1a156da5.png)


## Summary

Using historic wildfire data from California Government, analyzing the dataset and building the machine learning model to predict occurrence of major fire. Prediction of fire activities in the forest guides the authorities to make optimal, efficient, and sound decisions in fire management. The goal is to demonstrate the methodology of building advanced machine learning models using historic wildfire data and hence doesn’t represent the best model performance, rather presents an approach to take a data-driven approach to predict wildfires.


## Project Execution Plan

Please follow these [STEPS](https://docs.google.com/document/d/14wAf0Yp-gkutAlfjsqX9qAkFR4eNIuhQAqZhpegT_bY/edit?usp=sharing) to execute California Wildfire deliverables. 

## Project Resources

- **Project Presentaton:** [View Project Presentation](https://docs.google.com/presentation/d/1gnMfB6wmNLj-aO9sgZWFiF82ZcbtqUYd51E1__3BVuY/edit?usp=sharing)
- **Tableau Dashbaord**. [View California Wildfire Dashboard](https://public.tableau.com/app/profile/taravat/viz/California_Wildfires_2013-2019/CaliforniaWildfire2013-2019#1)
