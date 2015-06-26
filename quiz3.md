# Practical Machine Learning Quiz 3

## Question 1
Load the cell segmentation data from the AppliedPredictiveModeling package using the commands:
```
library(AppliedPredictiveModeling)
data(segmentationOriginal)
library(caret)
```
1. Subset the data to a training set and testing set based on the Case variable in the data set. 
2. Set the seed to 125 and fit a CART model with the rpart method using all predictor variables and default caret settings. 
3. In the final model what would be the final model prediction for cases with the following variable values:
a. TotalIntench2 = 23,000; FiberWidthCh1 = 10; PerimStatusCh1=2 
b. TotalIntench2 = 50,000; FiberWidthCh1 = 10;VarIntenCh4 = 100 
c. TotalIntench2 = 57,000; FiberWidthCh1 = 8;VarIntenCh4 = 100 
d. FiberWidthCh1 = 8;VarIntenCh4 = 100; PerimStatusCh1=2 

### Solution to Question 1
```
data <- segmentationOriginal
set.seed(125)
inTrain <- data$Case == "Train"
trainData <- data[inTrain, ]
testData <- data[!inTrain, ]
cartModel <- train(Class ~ ., data = trainData, method = "rpart")
cartModel$finalModel

> cartModel$finalModel
n= 1009 

node), split, n, loss, yval, (yprob)
      * denotes terminal node

1) root 1009 373 PS (0.63032706 0.36967294)  
  2) TotalIntenCh2< 45323.5 454  34 PS (0.92511013 0.07488987) *
  3) TotalIntenCh2>=45323.5 555 216 WS (0.38918919 0.61081081)  
    6) FiberWidthCh1< 9.673245 154  47 PS (0.69480519 0.30519481) *
    7) FiberWidthCh1>=9.673245 401 109 WS (0.27182045 0.72817955) *
    
plot(cartModel$finalModel, uniform=T)
text(cartModel$finalModel, cex=0.8)
# a. TotalIntench2 = 23,000; FiberWidthCh1 = 10; PerimStatusCh1=2 => PS
# b. TotalIntench2 = 50,000; FiberWidthCh1 = 10;VarIntenCh4 = 100 => WS
# c. TotalIntench2 = 57,000; FiberWidthCh1 = 8;VarIntenCh4 = 100 => PS
# d. FiberWidthCh1 = 8;VarIntenCh4 = 100; PerimStatusCh1=2 => Not possible to predict 
```


## Question 2
If K is small in a K-fold cross validation is the bias in the estimate of out-of-sample (test set) accuracy smaller or bigger? If K is small is the variance in the estimate of out-of-sample (test set) accuracy smaller or bigger. Is K large or small in leave one out cross validation?

### Solution to Question 2
```
The bias is larger and the variance is smaller. Under leave one out cross validation K is equal to the sample size.
```


## Question 3
Load the olive oil data using the commands:
```
library(pgmm)
data(olive)
olive = olive[,-1]
```
(NOTE: If you have trouble installing the **pgmm** package, you can download the `olive` dataset here: olive_data.zip. After unzipping the archive, you can load the file using the `load()` function in R.)
These data contain information on 572 different Italian olive oils from multiple regions in Italy. Fit a classification tree where Area is the outcome variable. Then predict the value of area for the following data frame using the tree command with all defaults
```
newdata = as.data.frame(t(colMeans(olive)))
```
What is the resulting prediction? Is the resulting prediction strange? Why or why not?

### Solution to Question 3

