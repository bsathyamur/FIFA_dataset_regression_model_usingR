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

##   Res.Df   RSS Df Sum of Sq  F Pr(>F)    
## 1   2704 25416                           
## 2   2642 19483 62      5933 13 <2e-16 ***

#### Inference
Based on the anova test, the larger model after removing collinearity is significant

#### Step 2
We will again check the variance and normality of VIF MODEL - MODEL 2

#### Inference
The Fitted vs. Residuals and Normal QQ plots are not ok.

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

#### Building a new model using the dataset with influential and outliers points removed - MODEL 3




