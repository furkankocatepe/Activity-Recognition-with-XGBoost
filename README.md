# Activity-Recognition-with-XGBoost

# INTRODUCTION

## DATASET OVERVIEW:
Link to the dataset: https://archive.ics.uci.edu/dataset/366/activity+recognition+system+based+on+multisensor+data+fusion+arem
 - The dataset has 480 sequences, for a total of 42,240 instances. Each activity contains 15 RSS data temporal segments. It also takes into account two forms of bending exercise. Supplementary figures depict the sensor node placements and associated identifiers.
 - The raw data is processed to obtain time-domain characteristics. This process employs a 250-millisecond epoch time and collects 5 RSS samples from each pair of WSN nodes. Each reciprocal RSS reading's mean value and variance are among the attributes.
 - The visual below illustrates the positions of the sensors.

 <img src=".\Dataset\sensorsPlacement.png" alt="sensorsPlacement" width="300"/>

## PROBLEM OVERVIEW:
 - The dataset is intended for classifying user activity based on time-series data collected by a Wireless Sensor Network (WSN). This network measures the RSS (Received Signal Strength) of beacon packets transferred across WSN devices that have been altered by user movements. These IRIS nodes are worn on the user's chest and ankles.
 - The dataset is distributed to 7 folders, each folder contains data from measurements specific to each activity. The number of files in these folders range between 7 and 15. Each of the files contain data collected from the sensors in 120 seconds. By using this dataset, the project aims to create a model that will correctly classify the user's activity. 
 - The target activities are; bending1, bending2, cycling, lying, sitting, standing, and walking. The visual below shows the description of two bending activities:

 <img src=".\Dataset\bendingType.png" alt="bendingType" width="600"/>

 ## SUMMARY OF STEPS

### 1. DATA WRANGLING AND TRANSFORMATION:
 - The data was imported from each file using a for loop. 
 - After that, a general data cleaning process has been done. Specifically, one of the files is space delimited, unlike other files. Therefore, data obtained from that file was inaccurate. This problem was solved as well. 
 - A random sample was taken from the dataset to be used for out of sample prediction at the end.

### 2. EXPLORATORY DATA ANALYSIS:
 - First three visuals were created for analyzing the distribution of features as well as the target classes in the dataframe.
 - The last visual was created for searching for strong correlations between features. 

### 3. FEATURE ENGINEERING:
 - The target column was encoded using LabelEncoder in order to convert the values into numeric values so that it can be used when creating the machine learning models.
 - Outliers were removed by using 95th percentile as a threshold. 
 - The modified dataset was saved into a CSV file. 
 - Two degrees Polynomial Transformation with only interactions were performed. However, this step was included in the Pipeline, therefore, it is not visible under Feature Engineering title. In order to decide if Polynomial Transformation is necessary, an early model building stage was conducted. pipe_report() method was used for building the model with no transformation, and pipe_poly_report() method was used for building the model with transformation. Since the accuracy with Polynomial Transformation with 2 degrees and interaction only is highest, this strategy was followed.
 - As explained above, the data is imbalanced. SMOTE was used to solve this issue as a last step of Feature Engineering. 

### 4. MODEL BUILDING:
 - Using pipe_poly_report() method, 7 classification models were created to compare which one performs better. 
 - XGBClassifier model performed best with an accuracy score over 85%. 

### 5. HYPERPARAMETER TUNING AND CROSS VALIDATION:
 - Two rounds of three fold cross validation was conducted by using GridSearchCV. However, the accuracy did not improve significant enough to justify the computational cost. 
 - The best parameters turned out to be; learning_rate=0.1, colsample_bytree=0.7, n_estimators=1000, max_depth=7

### 6. PREDICTIONS AND REPORT:
 - Predictions were made by using the best model found in the previous step. 
 - Classification report and a brief results analysis is included in this step.

### 7. OUT OF SAMPLE PREDICTION:
 - The random sample taken in the earlier steps were used for out of sample prediction. 
 - The classification report for this prediction can be found under this step. 

### 8. ANALYZING RESUTLS:
 - Overall, the model performed good when classifying activities. 
 - Classes 0 (bending1), 1 (bending2) and 3 (lying) have extremely good precision, recall, and F1-score (all greater than 0.97). This indicates that the model is good at predicting these classes, with few false positives or false negatives.
 - Classes 2 (cycling), 4 (sitting), 5 (standing), and 6 (walking) have lower scores than the preceding classes. Class 2 has the lowest scores, with precision, recall, and F1-scores all around 0.70. This implies that the model is having trouble correctly classifying this class, as demonstrated by a higher frequency of false positives and false negatives. Classes 4, and 5 have intermediate scores, indicating that they are performing moderately. 
 - As can be seen in the confusion matrix above, the model inaccurately classified walking as cycling and vice versa. 
 - The model's overall accuracy is 0.87, implying that 87% of the total instances were properly classified. The macro and weighted averages for precision, recall, and F1-score are also 0.87, showing that performance is balanced across all classes (weighted average).