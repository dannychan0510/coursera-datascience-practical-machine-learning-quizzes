# Practical Machine Learning Quiz 2

## Question 1
Load the Alzheimer's disease data using the commands:
```
library(AppliedPredictiveModeling)
library(caret)
data(AlzheimerDisease)
```
Which of the following commands will create training and test sets with about 50% of the observations assigned to each?

### Solution to Question 1
```
adData = data.frame(diagnosis,predictors)
trainIndex = createDataPartition(diagnosis, p = 0.50,list=FALSE)
training = adData[trainIndex,]
testing = adData[-trainIndex,]
```


## Question 2
