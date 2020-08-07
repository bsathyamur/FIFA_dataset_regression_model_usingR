# Football analytics based on FIFA 2019 players data

## Introduction
The goal of the project is to be able to build a good model and predict the overall rating of the FIFA players based on their skillset data. With the growing popularity of FIFA, in usa we thought that predicting the player rating based on their skillset, wages and physical factors such as age, height, weight etc., is very interesting.

Also the number of observations we have used is around 18k+ with around 46 predictors to make the model more interesting and at the same time give an accurate prediction. As the quote, “All models are wrong, some models are useful”, our goal is to find a useful model for this dataset which can be used for prediction.

As technology has advanced over the last number of years data collection has become more in-depth and can be conducted with relative ease. Advancements in data collection have allowed for sports analytics to grow as well, leading to the development of advanced statistics as well sport specific technologies that allow for things like game simulations to be conducted by teams prior to play, improve fan acquisition and marketing strategies, and even understand the impact of sponsorship on each team as well as its fans which motivated us to chose this topic for the project.

The dataset is acquired from kaggle website and below is the link: https://www.kaggle.com/karangadiya/fifa19

Some descriptions about the variables of the dataset:

Overall : overall rating of the player
Age : age of the player
heightInch : height of the player in inches
WeightLbs : weight of the player in lbs
value_k_eu : value of the player in thousands in euro
wage_k_eu : wage of the player in thousands in euro
rel_tot_value_k : release amount of the player in thousands in euro
PreferredFoot : Preferred foot of the player, it is a factor variable, will be used as dummy variable
WeakFoot : skillset of week foot
Position: position of the player, it is a factor variable
JerseyNumber : Jersey Number of the player

## Methods
We will split the dataset into training and test dataset. We will use 15% data as training data and 85% of data as test data.

### Initial large simple model (MODEL1): 
Initial large model with all variables and limited interactions between Age,heightInch and WeightLbs with all other variables

#### Step 1: 
The p-value is very small 8.649510^{-104} and at an α level of 0.01

#### Inference: 
We reject the null hypothesis and prefer the larger complex model.

#### Step 2: 
Check the variance and normality of the complex model

![plot1](https://github.com/bsathyamur/FIFA_dataset_regression_model_usingR/blob/master/plot1.png)

#### Inference: 
The Fitted vs. Residuals and Normal QQ plots are not ok.

#### Step 3
Perform BP Test and Shapiro Wilk Test
BP Test (p-value) = 4.195310^{-11}
Shapiro Wilk test (p-value) = 1.281610^{-10}

#### Inference:
Data is not from equal variance and normal distribution

Check the collinearity of the predictors to find best useful predictors using variance inflation factor(VIF). We will remove the predictors with high collinearity.

### Building a new simple model using VIF predicators - MODEL 2

#### Step 1
Perform ANOVA test between MODEL 1 and MODEL 2

![anova1](https://github.com/bsathyamur/FIFA_dataset_regression_model_usingR/blob/master/ANOVA-1.png)

#### Inference
Based on the anova test, the larger model after removing collinearity is significant

#### Step 2
We will again check the variance and normality of VIF MODEL - MODEL 2

#### Inference
The Fitted vs. Residuals and Normal QQ plots are not ok.

![plot2](https://github.com/bsathyamur/FIFA_dataset_regression_model_usingR/blob/master/plot2.png)

#### Step 3
Perform  BP Test and Shapiro Wilk Test

BP Test (p-value) = 6.940610^{-12}
Shapiro Wilk test (p-value) = 5.444810^{-7}

#### Inference
The data is not from equal variance and normal distribution

#### Step 4
checking for unusual observations using influential and outliers points and removing them from the dataset

influential_points = cooks.distance(model_init1) < 4 / length(cooks.distance(model_init1))
outliers = abs(rstandard(model_init1)) > 2
remove_points = c(influential_points,outliers)
fifa_trn_data2=  subset(fifa_trn_data,remove_points)     
nrow(fifa_trn_data2)

#### Inference
2674 data points removed from the data set

#### Step 5
Build the model (MODEL-2) using the dataset with influential and outliers points removed

#### Step 6
we will check the variance and normality of the model with the new dataset

![plot3](https://github.com/bsathyamur/FIFA_dataset_regression_model_usingR/blob/master/plot3.png)

#### Inference
The Fitted vs. Residuals and Normal QQ plots are better now.

#### Step 7
Perform BP Test and Shapiro Wilk Test

BP Test (p-value) = 2.169310^{-6}
Shapiro Wilk test (p-value) = 0.5202

#### Inference
Shapiro Wilk test is passed now which suggests normality whereas the BP test is getting failed. Since it is a large dataset, we are making an assumption based on the fitted vs. residual that its of equal variance.

#### Step 8
Performing ANOVA test for MODEL-2 (with influential and outlier data points) and MODEL-3 (without influential and outlier data points)

![anova1](https://github.com/bsathyamur/FIFA_dataset_regression_model_usingR/blob/master/ANOVA-2.png)
 
 #### Inference
 The anova test suggests the larger model is significant.
 
 #### Step 9
 we will find best predictors of this model using Akaike Information Criterion (AIC) and Bayesian Information Criterion (BIC) methods
 
 #### Inference
 Based on the AIC/BIC, AIC model is better for this dataset.
 
### Results
We will compare the result of bp-test, Shapiro-Wilk test, LOOCV-rmse between different models derived from above methods:


Based on the above model comparison, Model AIC is the best model since it has the higher adjusted R2, normal distribution and low RMSE LOOCV.

### Validation
We will validate our model on test data which we created as 85% of the dataset. We will calculate RMSE and check the result:


Based on RMSE we see that model AIC has lowest RMSE difference(-0.128) between training and test data between the model which passes normality. Next we will see few predicted overall on test data using model_aic.

original_Overall predicted_overall difference
 7962                67             65.92    1.07759
 1418                76             76.53   -0.53270
 2590                74             77.01   -3.01259
 1425                76             73.83    2.16804
 9984                65             63.87    1.13305
 678                 79             72.45    6.55054
 5513                70             67.43    2.56544
 2371                74             73.93    0.07488
 11585               64             67.67   -3.67017
 13875               62             61.47    0.52756


### CONCLUSION
We were able to find a model that is able to satisfy high adjusted R^2,Normality and low RMSE though the model didn’t satisfy the constant variance assumption. Given the fact the residual vs. fitted plot looks good for the model and also the dataset is very huge the constance variance is not getting satisfied in any model so we were able to find a model that can only satisfy R^2, Normality and LOOCV RMSE.

We had to apply a variance stabilizing transformation, but we have normality and closer to constant variance. Also We don’t have issues with collinearity as we have removed the colinear variables from the model. We feel the constant variance is not satisfied due to lot of noise data in the data set.We removed the NA,influential and outliers data points from the data set by still the noise data is causing the constant variance to be not satisfied.Overall we feel the above model is a good predictive model for the football analystics dataset and our test validation of train vs. test RMSE results look good.

The theme of this class has been, "All models are wrong, some are useful". This summarizes what we discovered in this project.




