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
Load the cement data using the commands:
```
library(AppliedPredictiveModeling)
data(concrete)
library(caret)
set.seed(975)
inTrain = createDataPartition(mixtures$CompressiveStrength, p = 3/4)[[1]]
training = mixtures[ inTrain,]
testing = mixtures[-inTrain,]
```
Make a histogram and confirm the SuperPlasticizer variable is skewed. Normally you might use the log transform to try to make the data more symmetric. Why would that be a poor choice for this variable?


### Solution to Question 2
```
The Community TAs have reported some problems with Question 2 of Quiz 2 resulting from a code change in createDataPartition. As a result, the question has been nullified, which means that it has been removed from the quiz and that people who have already taken the quiz will not have the question included in the calculation of their grade. Everyone will be awarded 3 points for the question.
```

## Question 3
Load the Alzheimer's disease data using the commands:
```
library(caret)
library(AppliedPredictiveModeling)
set.seed(3433)
data(AlzheimerDisease)
adData = data.frame(diagnosis,predictors)
inTrain = createDataPartition(adData$diagnosis, p = 3/4)[[1]]
training = adData[ inTrain,]
testing = adData[-inTrain,]
```
Find all the predictor variables in the training set that begin with IL. Perform principal components on these variables with the preProcess() function from the caret package. Calculate the number of principal components needed to capture 80% of the variance. How many are there?

### Solutiont to Question 3
```
> preProc <- preProcess(training[, grep("^IL", names(testing))], method = "pca", thresh = 0.8)
> preProc$rotation
                      PC1           PC2           PC3          PC4         PC5
IL_11         -0.06529786  0.5555956867  0.2031317937 -0.050389599  0.73512798
IL_13          0.27529157  0.3559427297 -0.0399010765  0.265076920 -0.25796332
IL_16          0.42079000  0.0007224953  0.0832211446 -0.082097273  0.04435883
IL_17E        -0.01126118  0.5635958176  0.3744707126  0.302512329 -0.38918707
IL_1alpha      0.25078195 -0.0687043488 -0.3008366900  0.330945942  0.16992452
IL_3           0.42026485 -0.0703352892 -0.1049647272 -0.065352774  0.02352819
IL_4           0.33302031  0.0688495706 -0.1395450144  0.165631691 -0.14268797
IL_5           0.38706503 -0.0039619980  0.0005616126 -0.224448981  0.08426042
IL_6           0.05398185 -0.4248425653  0.6090821756  0.417591202 -0.00165066
IL_6_Receptor  0.21218980  0.1005338329  0.2920341087 -0.659953479 -0.29654048
IL_7           0.32948731  0.0806070090 -0.1966471906  0.165544952  0.11373532
IL_8           0.29329723 -0.1883039842  0.4405255221  0.002811187  0.28608600
                       PC6         PC7
IL_11         -0.102014559  0.20984151
IL_13         -0.068927711  0.58942516
IL_16         -0.007094672 -0.06581741
IL_17E         0.221149380 -0.46462692
IL_1alpha      0.742391473  0.12787035
IL_3          -0.165587911 -0.09006656
IL_4          -0.297421293  0.19661173
IL_5           0.153835977 -0.16425757
IL_6          -0.166089521  0.21895103
IL_6_Receptor  0.138000448  0.22657846
IL_7          -0.405698338 -0.42065832
IL_8           0.184321013 -0.14833779
```


## Question 4
Load the Alzheimer's disease data using the commands:
```
library(caret)
library(AppliedPredictiveModeling)
set.seed(3433)
data(AlzheimerDisease)
adData = data.frame(diagnosis,predictors)
inTrain = createDataPartition(adData$diagnosis, p = 3/4)[[1]]
training = adData[ inTrain,]
testing = adData[-inTrain,]
```
Create a training data set consisting of only the predictors with variable names beginning with IL and the diagnosis. Build two predictive models, one using the predictors as they are and one using PCA with principal components explaining 80% of the variance in the predictors. Use method="glm" in the train function. What is the accuracy of each method in the test set? Which is more accurate?


### Solution to Question 4
```
> training <- data.frame(training$diagnosis, training[, grep("^IL", names(testing))])
> 
> modelFit1 <- train(training$training.diagnosis ~ ., method = "glm", data = training)
> confusionMatrix(testing$diagnosis, predict(modelFit1, testing))
Confusion Matrix and Statistics

          Reference
Prediction Impaired Control
  Impaired        2      20
  Control         9      51
                                         
               Accuracy : 0.6463         
                 95% CI : (0.533, 0.7488)
    No Information Rate : 0.8659         
    P-Value [Acc > NIR] : 1.00000        
                                         
                  Kappa : -0.0702        
 Mcnemar's Test P-Value : 0.06332        
                                         
            Sensitivity : 0.18182        
            Specificity : 0.71831        
         Pos Pred Value : 0.09091        
         Neg Pred Value : 0.85000        
             Prevalence : 0.13415        
         Detection Rate : 0.02439        
   Detection Prevalence : 0.26829        
      Balanced Accuracy : 0.45006        
                                         
       'Positive' Class : Impaired       
                                         
> 
> modelFit2 <- train(training$training.diagnosis ~ ., method = "glm", data = training, preProcess = "pca", trControl = trainControl(preProcOptions = list(thresh = 0.8)))
> confusionMatrix(testing$diagnosis, predict(modelFit2, testing))
Confusion Matrix and Statistics

          Reference
Prediction Impaired Control
  Impaired        3      19
  Control         4      56
                                          
               Accuracy : 0.7195          
                 95% CI : (0.6094, 0.8132)
    No Information Rate : 0.9146          
    P-Value [Acc > NIR] : 1.000000        
                                          
                  Kappa : 0.0889          
 Mcnemar's Test P-Value : 0.003509        
                                          
            Sensitivity : 0.42857         
            Specificity : 0.74667         
         Pos Pred Value : 0.13636         
         Neg Pred Value : 0.93333         
             Prevalence : 0.08537         
         Detection Rate : 0.03659         
   Detection Prevalence : 0.26829         
      Balanced Accuracy : 0.58762         
                                          
       'Positive' Class : Impaired  
```
