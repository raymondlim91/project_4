# Project 4 - Kaggle Competition - Starter

## Problem Statement

West Nile virus is most commonly spread to humans through infected mosquitos. Around 20% of people who become infected with the virus develop symptoms ranging from a persistent fever to serious neurological illnesses that can result in death.
Due to the recent epidemic of West Nile Virus in the Windy City, the Department of Public Health has set up a surveillance and control system to monitor the number of mosquitos and West Nile Virus. 

As data scientists working at the Disease And Treatment Agency, division of Societal Cures In Epidemiology and New Creative Engineering (DATA-SCIENCE), we had been tasked to use the information collected by the Department of Public Health, combined with the data collected by Chicago’s weather stations to learn about the spread of West Nile Virus.
Our goal is to develop a classification model that could predict outbreaks of West Nile virus in mosquitos within the area of Windy City. We will also conduct cost-benefit analysis, and suggest a cost-efficient and effective method of deploying pesticides within the area.

---

## Executive Summary

Initially, we started with analysis on the four datasets provided which are train, test, weather, and spray data.
There are five main species of mosquito within Wind city. The top two most common mosquito species are Culex Pipiens and Culex Restuans. 

Based on the weather dataset, there are two different stations collecting the weather data. Hence, new feature [‘Station’] is created on trap dataset to map the weather station which is nearer to the trap for further analysis of how weather will affect the number of mosquitos.
After the datasets are cleaned, the weather and trap datasets are merged by using based on date and the weather stations.

The columns were further pre-processed for modeling. New feature columns such as ‘long_lat’, 'daylight', 'relative humidity', 'dew point depression', 'wet bulb depression' was created. Columns were modified using dummy variables, polynomial with degree 2 to better expose the important relationships between input features and the target.
 
The training dataset is heavily imbalanced. Up to 95% of the class shows WNV was not present and only 5% of the data shows the WNV was present. To resolve the imbalanced dataset, Synthetic Minority Over-sampling Technique(SMOTE) is used to oversample the minority class before fitting into the model.
 
Train models are then split into 75% for the training model and 25% as the unseen test set to evaluate the model performance. Six different types of Machine Learning Classifiers were used to build the model which includes:
<br>
1. Logistic Regression
2. GradientBoosting
3. RandomForest
4. AdaBoost
5. ExtraTrees
6. Support Vector Classier

---

## Conclusions
Due to the target variable is heavily imbalanced, judging the model's performance based on accuracy is not reliable. Instead, other metrics we can look at is the precision, recall, and F1-score. We decided to use F1-score as the deciding metrics for the model performance. By judging using F1-score able to achieve a balance between low false positives and false negatives. 

This is to lower the cost of spraying by spraying at the the location with false positives cases and to prevent missing out on spraying where the virus was actually present, which is the false negatives.

ROC-AUC score is also useful to judge a model. It is useful to measure of how much the models are capable of distinguising the classes of 0 and 1. 
The higher the AUC, the better the model is at distinguishing between mosquito traps with Wnv present and Wnv NOT present. 

Based on the ROC-AUC graph above, we observed four models has high ROC-AUC which are the `Random Forest`, `Extra Trees`, `AdaBoost`, and `GradientBoosting` with AUC score around 0.8. 

Based on the model, `AdaBoost` was selected as the best performing models based on it's high F1-score at 0.5871, and AUC score around 0.81


## Cost Analysis

Further [research](https://www.chicago.gov/content/dam/city/depts/cdph/CDPH/Healthy%20Chicago/SprayZone_T220_08252021.pdf) showed that the government is spraying approximate 1120 acres per exercise. Assuming at a cost of \\$87 per gallon, and with 1.5 fluid ounces per acre used during each exercise, we worked out to approximate \$1141.88 per exercise. 

By using our model to predict the number of cases, we were able to estimate the medical cost that will be incurred to take care of the population, should they be infected. [Research](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322011/) shown that the cost to treat an infected patient suffering from the more serious West Nile neuroinvasive disease (WWND) (excluding the economic cost) is approximately \\$33,000, while the cost to treat West Nile Fever (WNF) is approximately \$302. 

Under the assumption that all patients suffered only from West Nile Fever (WNF), we estimated the medical cost as follows:

| Year | Cases | Medical Cost|
|-|-|-|
| 2008 | 1230 | \$371,460 |
| 2010 | 1916 | \$578,632 |
| 2012 | 4427 | \$1,336,954 |
| 2014 | 1639 | \$491,958 |

We can see that the possible incurred medical cost is way higher than the cost of each spray exercise. In fact, the medical cost did not factor in the potential economic cost. This suggests that we should choose one of the vector control methods, such as spraying of insecticide to save future costs impacted by the existence of the West Nile Virus. 

It is also worth noting that mosquito populations can bring about other vector borne diseases such as dengue and malaria. The department should continue the efforts to educate the population on preventive measures that can be taken to prevent the breeding of mosquitos, starting from their own homes.

---

## Recommendation/Further improvements

Through our cost benefit analysis, it is clear the benefits of spraying pesticides supercede any other cost concerns. Knowing how an outbreak can possibly drain the city's resources, it is crucial that our model's predictions be adopted to identify where and when to spray. 
Should there be any budget limitations that disallow us from spraying the entire Chicago, we recommend using our model's  predictions to target specifically the areas marked out as high WNV probabilities and to spray each area on a rotational basis so that each area gets sprayed thrice a year minimally.
On top of this, we also recommend launching education campaigns and public reminders for hotspot neighbourhoods to prevent the breeding of these mosquitoes and be on high alert to self protect especially during months that the mosquito population is expected to peak. 

Beyond this, we would also like to refine our predictions by collecting more data to improve our accuracy, carry out more feature engineering to better our model's score, gather regular data to track the effectiveness of the spraying efforts and also to get our hands on more detailed cost information that includes logistics/ manhour costs involved in the spraying of the pesticides. 

---

## Kaggle Results:
![Submission](./images/kaggle_submission.png)



---
## Data Dictionary

train.csv, test.csv - the training and test set of the main dataset. The training set consists of data from 2007, 2009, 2011, and 2013, while in the test set you are requested to predict the test results for 2008, 2010, 2012, and 2014.
```
- Id: the id of the record
- Date: date that the WNV test is performed
- Address: approximate address of the location of trap. This is used to send to the GeoCoder.
- Species: the species of mosquitos
- Block: block number of address
- Street: street name
- Trap: Id of the trap
- AddressNumberAndStreet: approximate address returned from GeoCoder
- Latitude, Longitude: Latitude and Longitude returned from GeoCoder
- AddressAccuracy: accuracy returned from GeoCoder
- NumMosquitos: number of mosquitoes caught in this trap
- WnvPresent: whether West Nile Virus was present in these mosquitos. 1 means WNV is present, and 0 means not present.
- day: Day of Date
- month: Month of Date
- year: Year of Date
```
spray.csv - GIS data of spraying efforts in 2011 and 2013
```
- Date, Time: the date and time of the spray
- Latitude, Longitude: the Latitude and Longitude of the spray
- day: Day of Date
- month: Month of Date
- year: Year of Date
```

weather.csv - weather data from 2007 to 2014. Column descriptions in `noaa_weather_qclcd_documentation.pdf`.
