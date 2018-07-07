---
title: "Machine-Learning-and-Econometrics-AEA-2018"
output:   
  html_document:
    keep_md: true
---

Machine Learning and Econometrics

Introduction Lecture: American Economic Association Continuing Education Program, January 2018

Course Description:
The course will cover topics on the intersection of machine learning and econometrics.
There will be particular emphasis on the use of machine learning methods for estimating
causal effects. In addition there will be some discussion of basic machine learning methods
that we view as useful tools for empirical economists.

### Summary



### Work

Clean environment, load packages


```r
rm(list=ls())

packages <- c("devtools"
  ,"randomForest" 
  ,"rpart" # decision tree
  ,"rpart.plot" # enhanced tree plots
  ,"ROCR"
  ,"Hmisc"
  ,"corrplot"
  ,"texreg"
  ,"glmnet"
  ,"reshape2"
  ,"knitr"
  ,"xtable"
  ,"lars"
  ,"ggplot2"
  ,"matrixStats"
  ,"plyr"
  ,"stargazer")


not_installed <- !packages %in% installed.packages()
if (any(not_installed)) install.packages(packages[not_installed])
lapply(packages,require,character.only=TRUE)
```

```
## Loading required package: devtools
```

```
## Loading required package: randomForest
```

```
## Warning: package 'randomForest' was built under R version 3.4.4
```

```
## randomForest 4.6-14
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```
## Loading required package: rpart
```

```
## Loading required package: rpart.plot
```

```
## Warning: package 'rpart.plot' was built under R version 3.4.4
```

```
## Loading required package: ROCR
```

```
## Warning: package 'ROCR' was built under R version 3.4.4
```

```
## Loading required package: gplots
```

```
## Warning: package 'gplots' was built under R version 3.4.4
```

```
## 
## Attaching package: 'gplots'
```

```
## The following object is masked from 'package:stats':
## 
##     lowess
```

```
## Loading required package: Hmisc
```

```
## Warning: package 'Hmisc' was built under R version 3.4.4
```

```
## Loading required package: lattice
```

```
## Loading required package: survival
```

```
## Loading required package: Formula
```

```
## Warning: package 'Formula' was built under R version 3.4.4
```

```
## Loading required package: ggplot2
```

```
## Warning: package 'ggplot2' was built under R version 3.4.4
```

```
## 
## Attaching package: 'ggplot2'
```

```
## The following object is masked from 'package:randomForest':
## 
##     margin
```

```
## 
## Attaching package: 'Hmisc'
```

```
## The following objects are masked from 'package:base':
## 
##     format.pval, units
```

```
## Loading required package: corrplot
```

```
## Warning: package 'corrplot' was built under R version 3.4.4
```

```
## corrplot 0.84 loaded
```

```
## Loading required package: texreg
```

```
## Warning: package 'texreg' was built under R version 3.4.4
```

```
## Version:  1.36.23
## Date:     2017-03-03
## Author:   Philip Leifeld (University of Glasgow)
## 
## Please cite the JSS article in your publications -- see citation("texreg").
```

```
## Loading required package: glmnet
```

```
## Warning: package 'glmnet' was built under R version 3.4.4
```

```
## Loading required package: Matrix
```

```
## Loading required package: foreach
```

```
## Warning: package 'foreach' was built under R version 3.4.4
```

```
## Loaded glmnet 2.0-16
```

```
## Loading required package: reshape2
```

```
## Loading required package: knitr
```

```
## Warning: package 'knitr' was built under R version 3.4.4
```

```
## Loading required package: xtable
```

```
## 
## Attaching package: 'xtable'
```

```
## The following objects are masked from 'package:Hmisc':
## 
##     label, label<-
```

```
## Loading required package: lars
```

```
## Loaded lars 1.2
```

```
## Loading required package: matrixStats
```

```
## Warning: package 'matrixStats' was built under R version 3.4.4
```

```
## Loading required package: plyr
```

```
## 
## Attaching package: 'plyr'
```

```
## The following object is masked from 'package:matrixStats':
## 
##     count
```

```
## The following objects are masked from 'package:Hmisc':
## 
##     is.discrete, summarize
```

```
## Loading required package: stargazer
```

```
## Warning: package 'stargazer' was built under R version 3.4.4
```

```
## 
## Please cite as:
```

```
##  Hlavac, Marek (2018). stargazer: Well-Formatted Regression and Summary Statistics Tables.
```

```
##  R package version 5.2.2. https://CRAN.R-project.org/package=stargazer
```

```
## [[1]]
## [1] TRUE
## 
## [[2]]
## [1] TRUE
## 
## [[3]]
## [1] TRUE
## 
## [[4]]
## [1] TRUE
## 
## [[5]]
## [1] TRUE
## 
## [[6]]
## [1] TRUE
## 
## [[7]]
## [1] TRUE
## 
## [[8]]
## [1] TRUE
## 
## [[9]]
## [1] TRUE
## 
## [[10]]
## [1] TRUE
## 
## [[11]]
## [1] TRUE
## 
## [[12]]
## [1] TRUE
## 
## [[13]]
## [1] TRUE
## 
## [[14]]
## [1] TRUE
## 
## [[15]]
## [1] TRUE
## 
## [[16]]
## [1] TRUE
## 
## [[17]]
## [1] TRUE
```

Setwd


```r
setwd("C:/Users/Gateway/Desktop/Dropbox/Data Science/ML Susan Athey/My Repository/Machine-Learning-and-Econometrics-AEA-2018")
```

Load data (large file is 156mb)
Sample is 45mb


```r
#social_full <- read.csv("C:/Users/Gateway/Desktop/ExperimentData-master/ExperimentData-master/Social/RawData/social/social.csv")
#colnames(social_full)

social <- read.csv("C:/Users/Gateway/Desktop/Dropbox/Data Science/ML Susan Athey/R tutorial/socialneighbor.csv")
colnames(social)
```

```
##  [1] "sex"                          "yob"                         
##  [3] "g2000"                        "g2002"                       
##  [5] "g2004"                        "p2000"                       
##  [7] "p2002"                        "cluster"                     
##  [9] "votedav"                      "dem"                         
## [11] "nov"                          "aug"                         
## [13] "city"                         "hh_id"                       
## [15] "hh_size"                      "totalpopulation_estimate"    
## [17] "percent_male"                 "percent_female"              
## [19] "median_age"                   "percent_under5years"         
## [21] "percent_5to9years"            "percent_10to14years"         
## [23] "percent_15to19years"          "percent_20to24years"         
## [25] "percent_25to34years"          "percent_35to44years"         
## [27] "percent_45to54years"          "percent_55to59years"         
## [29] "percent_60to64years"          "percent_65to74years"         
## [31] "percent_75to84years"          "percent_85yearsandolder"     
## [33] "percent_18yearsandolder"      "percent_21yearsandover"      
## [35] "percent_62yearsandover"       "percent_65yearsandover"      
## [37] "percent_white"                "percent_black"               
## [39] "percent_amindian_alaskan"     "percent_asian"               
## [41] "percent_nativeandother"       "percent_other_nativeandother"
## [43] "percent_hispanicorlatino"     "percent_race_other"          
## [45] "median_income"                "mean_income"                 
## [47] "employ_16"                    "unemploy_16"                 
## [49] "unemploy_20to64"              "employ_20to64"               
## [51] "employ_rename_20to64"         "hsorhigher"                  
## [53] "bach_orhigher"                "less9thgrade"                
## [55] "grade9to12"                   "highschool"                  
## [57] "somecollege"                  "assoc"                       
## [59] "bachelors"                    "grad"                        
## [61] "outcome_voted"                "treatment_dum"               
## [63] "treat_hawthorne"              "treat_civic"                 
## [65] "treat_neighbors"              "treat_self"                  
## [67] "randn"                        "oneperhh"                    
## [69] "p2004"
```

Generate 'noise covariates' and them bind to data
Noise covariates defined:


```r
set.seed(333)
noise.covars <- matrix(data = runif(nrow(social) * 13),
                       nrow = nrow(social), ncol = 13)
noise.covars <- data.frame(noise.covars)
names(noise.covars) <- c("noise1", "noise2", "noise3", "noise4", "noise5", "noise6",
                         "noise7", "noise8", "noise9", "noise10", "noise11", "noise12", "noise13")

working <- cbind(social, noise.covars)
```

Run on a subsample, this is already the main dataset in this practice


```r
set.seed(666)
working <- working[sample(nrow(social), 20000), ]

# Pick a selection of covariates
# If we have a lot of data and computation power, it is suggested that
# we include all covariates and use regularization. This suggestion is
# based on the observation that it's much easier to fix the overfitting 
# problem than to fix the underfitting problem.
covariate.names <- c("yob", "hh_size", "sex", "city", "g2000","g2002", "p2000", "p2002", "p2004"
                     ,"totalpopulation_estimate","percent_male","median_age", "percent_62yearsandover"
                     ,"percent_white", "percent_black", "median_income",
                     "employ_20to64", "highschool", "bach_orhigher","percent_hispanicorlatino",
                     "noise1", "noise2", "noise3", "noise4", "noise5", "noise6",
                     "noise7", "noise8", "noise9", "noise10", "noise11", "noise12","noise13")
```

The dependent (outcome) variable is whether the person voted, so let's rename "outcome_voted" to Y


```r
names(working)[names(working)=="outcome_voted"] <- "Y"
# Extract the dependent variable
Y <- working [["Y"]]
# The treatment is whether they received the "your neighbors are voting" letter
names(working)[names(working)=="treat_neighbors"] <- "W"
# Extract treatment variable & covariates
W <- working[["W"]]
covariates <- working[covariate.names]
```

Some algorithms require our covariates be scaled
The 'scale' function, with default settings, will calculate the mean and standard deviation of the entire vector, 
then "scale" each element by those values by subtracting the mean and dividing the standard deviation


```r
covariates.scaled <- scale(covariates)
processed.unscaled <- data.frame(Y, W, covariates)
processed.scaled <- data.frame(Y, W, covariates.scaled)
```

Some of the models in this practice will require training, validation, and test sets.
Set seed for reproducibility
Divide dataset into training and test set. 
I'll be using a 90-10 split, but you can change this as you feel necessary in the sample command


```r
set.seed(999)
smplmain <- sample(nrow(processed.scaled), round(9*nrow(processed.scaled)/10), replace = FALSE)

processed.scaled.train <- processed.scaled[smplmain, ]
processed.scaled.test <- processed.scaled[-smplmain, ]

y.train <- as.matrix(processed.scaled.train$Y, ncol = 1)
y.test <- as.matrix(processed.scaled.test$Y, ncol = 1)

# Create 45-45-10 sample
smplcausal <- sample(nrow(processed.scaled.train),
                     round(5*nrow(processed.scaled.train)/10), replace = FALSE)
processed.scaled.train.1 <- processed.scaled.train[smplcausal, ]
processed.scaled.train.2 <- processed.scaled.train[-smplcausal, ]
```

### Instructor's note
When we want to scale the variables with the presence of training, validation, and test sets, 
we usually use only the training set to obtain the mean and standard deviation. Then we scale all three sets
using the obtained mean and sd.
#### Creating Formulas
For many of the models, we will need a "formula"
This will be in the format Y ~ X1 + X2 + X3 + ...
For more info, see: http://faculty.chicagobooth.edu/richard.hahn/teaching/formulanotation.pdf


```r
print(covariate.names)
```

```
##  [1] "yob"                      "hh_size"                 
##  [3] "sex"                      "city"                    
##  [5] "g2000"                    "g2002"                   
##  [7] "p2000"                    "p2002"                   
##  [9] "p2004"                    "totalpopulation_estimate"
## [11] "percent_male"             "median_age"              
## [13] "percent_62yearsandover"   "percent_white"           
## [15] "percent_black"            "median_income"           
## [17] "employ_20to64"            "highschool"              
## [19] "bach_orhigher"            "percent_hispanicorlatino"
## [21] "noise1"                   "noise2"                  
## [23] "noise3"                   "noise4"                  
## [25] "noise5"                   "noise6"                  
## [27] "noise7"                   "noise8"                  
## [29] "noise9"                   "noise10"                 
## [31] "noise11"                  "noise12"                 
## [33] "noise13"
```


```r
sumx <- paste(covariate.names, collapse = " + ")  # "X1 + X2 + X3 + ..." makes substitution easier later
interx <- paste(" (",sumx, ")^2", sep="")  # "(X1 + X2 + X3 + ...)^2" makes substitution easier later

# Y ~ X1 + X2 + X3 + ...
linearnotreat <- paste("Y", sumx, sep=" ~ ")
linearnotreat <- as.formula(linearnotreat)
linearnotreat
```

```
## Y ~ yob + hh_size + sex + city + g2000 + g2002 + p2000 + p2002 + 
##     p2004 + totalpopulation_estimate + percent_male + median_age + 
##     percent_62yearsandover + percent_white + percent_black + 
##     median_income + employ_20to64 + highschool + bach_orhigher + 
##     percent_hispanicorlatino + noise1 + noise2 + noise3 + noise4 + 
##     noise5 + noise6 + noise7 + noise8 + noise9 + noise10 + noise11 + 
##     noise12 + noise13
```


```r
linear <- paste("Y", paste("W", sumx, sep=" + "), sep = " ~ ")
linear <- as.formula(linear)
linear
```

```
## Y ~ W + yob + hh_size + sex + city + g2000 + g2002 + p2000 + 
##     p2002 + p2004 + totalpopulation_estimate + percent_male + 
##     median_age + percent_62yearsandover + percent_white + percent_black + 
##     median_income + employ_20to64 + highschool + bach_orhigher + 
##     percent_hispanicorlatino + noise1 + noise2 + noise3 + noise4 + 
##     noise5 + noise6 + noise7 + noise8 + noise9 + noise10 + noise11 + 
##     noise12 + noise13
```

Y ~ W * (X1 + X2 + X3 + ...)
---> X*Z means to include these variables plus the interactions between them


```r
linearhet <- paste("Y", paste("W * (", sumx, ") ", sep=""), sep = " ~ ")
linearhet <- as.formula(linearhet)
linearhet
```

```
## Y ~ W * (yob + hh_size + sex + city + g2000 + g2002 + p2000 + 
##     p2002 + p2004 + totalpopulation_estimate + percent_male + 
##     median_age + percent_62yearsandover + percent_white + percent_black + 
##     median_income + employ_20to64 + highschool + bach_orhigher + 
##     percent_hispanicorlatino + noise1 + noise2 + noise3 + noise4 + 
##     noise5 + noise6 + noise7 + noise8 + noise9 + noise10 + noise11 + 
##     noise12 + noise13)
```

### We can now use these formulas to do linear regression and logit regressions
### Linear Regression


```r
lm.linear <- lm(linear, data = processed.scaled)
summary(lm.linear)
```

```
## 
## Call:
## lm(formula = linear, data = processed.scaled)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -0.7368 -0.3321 -0.2122  0.5315  1.0422 
## 
## Coefficients:
##                            Estimate Std. Error t value Pr(>|t|)    
## (Intercept)               0.2981038  0.0034471  86.479  < 2e-16 ***
## W                         0.0891277  0.0084792  10.511  < 2e-16 ***
## yob                      -0.0267036  0.0034734  -7.688 1.56e-14 ***
## hh_size                   0.0072574  0.0033282   2.181 0.029228 *  
## sex                      -0.0034809  0.0031563  -1.103 0.270107    
## city                      0.0441209  0.0033727  13.082  < 2e-16 ***
## g2000                    -0.0159098  0.0034453  -4.618 3.90e-06 ***
## g2002                     0.0329267  0.0034288   9.603  < 2e-16 ***
## p2000                     0.0488377  0.0032316  15.113  < 2e-16 ***
## p2002                     0.0578063  0.0032825  17.611  < 2e-16 ***
## p2004                     0.0717669  0.0032320  22.205  < 2e-16 ***
## totalpopulation_estimate  0.0181579  0.0040617   4.470 7.85e-06 ***
## percent_male             -0.0071904  0.0038105  -1.887 0.059180 .  
## median_age                0.0116518  0.0076335   1.526 0.126924    
## percent_62yearsandover    0.0012904  0.0075448   0.171 0.864198    
## percent_white             0.0121015  0.0081836   1.479 0.139221    
## percent_black             0.0161190  0.0073676   2.188 0.028695 *  
## median_income             0.0274884  0.0072751   3.778 0.000158 ***
## employ_20to64            -0.0089059  0.0044864  -1.985 0.047150 *  
## highschool                0.0245081  0.0114049   2.149 0.031653 *  
## bach_orhigher            -0.0068557  0.0125007  -0.548 0.583408    
## percent_hispanicorlatino -0.0027757  0.0042682  -0.650 0.515487    
## noise1                   -0.0066916  0.0031515  -2.123 0.033741 *  
## noise2                   -0.0028134  0.0031518  -0.893 0.372069    
## noise3                   -0.0060408  0.0031515  -1.917 0.055278 .  
## noise4                    0.0003293  0.0031510   0.105 0.916770    
## noise5                   -0.0003904  0.0031522  -0.124 0.901434    
## noise6                   -0.0022741  0.0031528  -0.721 0.470731    
## noise7                   -0.0006334  0.0031521  -0.201 0.840750    
## noise8                   -0.0015779  0.0031516  -0.501 0.616623    
## noise9                   -0.0011737  0.0031521  -0.372 0.709627    
## noise10                  -0.0027257  0.0031519  -0.865 0.387166    
## noise11                  -0.0022378  0.0031516  -0.710 0.477685    
## noise12                  -0.0007681  0.0031522  -0.244 0.807494    
## noise13                   0.0001813  0.0031512   0.058 0.954119    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.4453 on 19965 degrees of freedom
## Multiple R-squared:  0.07922,	Adjusted R-squared:  0.07766 
## F-statistic: 50.52 on 34 and 19965 DF,  p-value: < 2.2e-16
```


```r
lm.linearhet <- lm(linearhet, data = processed.scaled)
summary(lm.linearhet)
```

```
## 
## Call:
## lm(formula = linearhet, data = processed.scaled)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -0.7730 -0.3292 -0.2091  0.5290  1.0275 
## 
## Coefficients:
##                              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                 0.2980799  0.0034454  86.515  < 2e-16 ***
## W                           0.0885277  0.0084929  10.424  < 2e-16 ***
## yob                        -0.0255813  0.0037992  -6.733 1.70e-11 ***
## hh_size                     0.0096467  0.0036456   2.646  0.00815 ** 
## sex                        -0.0041477  0.0034541  -1.201  0.22984    
## city                        0.0436233  0.0036815  11.849  < 2e-16 ***
## g2000                      -0.0151126  0.0037896  -3.988 6.69e-05 ***
## g2002                       0.0344670  0.0037640   9.157  < 2e-16 ***
## p2000                       0.0506877  0.0035322  14.350  < 2e-16 ***
## p2002                       0.0568948  0.0035955  15.824  < 2e-16 ***
## p2004                       0.0684929  0.0035357  19.372  < 2e-16 ***
## totalpopulation_estimate    0.0204342  0.0044403   4.602 4.21e-06 ***
## percent_male               -0.0073278  0.0041525  -1.765  0.07763 .  
## median_age                  0.0091513  0.0083699   1.093  0.27425    
## percent_62yearsandover      0.0048100  0.0082368   0.584  0.55925    
## percent_white               0.0198658  0.0090061   2.206  0.02741 *  
## percent_black               0.0209896  0.0081277   2.582  0.00982 ** 
## median_income               0.0307541  0.0079508   3.868  0.00011 ***
## employ_20to64              -0.0136981  0.0048962  -2.798  0.00515 ** 
## highschool                  0.0339743  0.0124684   2.725  0.00644 ** 
## bach_orhigher               0.0033146  0.0136550   0.243  0.80821    
## percent_hispanicorlatino   -0.0003629  0.0046637  -0.078  0.93798    
## noise1                     -0.0054748  0.0034552  -1.585  0.11309    
## noise2                     -0.0008365  0.0034521  -0.242  0.80853    
## noise3                     -0.0037488  0.0034501  -1.087  0.27724    
## noise4                      0.0020763  0.0034435   0.603  0.54654    
## noise5                     -0.0019096  0.0034417  -0.555  0.57901    
## noise6                     -0.0032725  0.0034514  -0.948  0.34306    
## noise7                     -0.0031993  0.0034435  -0.929  0.35286    
## noise8                     -0.0008513  0.0034505  -0.247  0.80514    
## noise9                     -0.0019423  0.0034488  -0.563  0.57331    
## noise10                     0.0009840  0.0034521   0.285  0.77563    
## noise11                    -0.0013815  0.0034496  -0.400  0.68880    
## noise12                     0.0021180  0.0034448   0.615  0.53867    
## noise13                    -0.0004197  0.0034438  -0.122  0.90299    
## W:yob                      -0.0066130  0.0093780  -0.705  0.48072    
## W:hh_size                  -0.0150441  0.0089306  -1.685  0.09209 .  
## W:sex                       0.0049815  0.0085145   0.585  0.55852    
## W:city                      0.0028515  0.0091938   0.310  0.75645    
## W:g2000                    -0.0062835  0.0091127  -0.690  0.49050    
## W:g2002                    -0.0081149  0.0091312  -0.889  0.37418    
## W:p2000                    -0.0108902  0.0087474  -1.245  0.21316    
## W:p2002                     0.0054052  0.0088309   0.612  0.54049    
## W:p2004                     0.0189682  0.0087406   2.170  0.03001 *  
## W:totalpopulation_estimate -0.0152105  0.0110006  -1.383  0.16677    
## W:percent_male              0.0001264  0.0104862   0.012  0.99038    
## W:median_age                0.0102881  0.0204709   0.503  0.61527    
## W:percent_62yearsandover   -0.0164189  0.0205920  -0.797  0.42526    
## W:percent_white            -0.0467292  0.0216546  -2.158  0.03094 *  
## W:percent_black            -0.0285965  0.0193215  -1.480  0.13888    
## W:median_income            -0.0146889  0.0197814  -0.743  0.45776    
## W:employ_20to64             0.0293825  0.0122510   2.398  0.01648 *  
## W:highschool               -0.0580220  0.0308503  -1.881  0.06002 .  
## W:bach_orhigher            -0.0657108  0.0339544  -1.935  0.05297 .  
## W:percent_hispanicorlatino -0.0140987  0.0116109  -1.214  0.22466    
## W:noise1                   -0.0083524  0.0084371  -0.990  0.32221    
## W:noise2                   -0.0104238  0.0084649  -1.231  0.21818    
## W:noise3                   -0.0148297  0.0085031  -1.744  0.08117 .  
## W:noise4                   -0.0107384  0.0085604  -1.254  0.20970    
## W:noise5                    0.0080960  0.0085745   0.944  0.34508    
## W:noise6                    0.0064320  0.0084880   0.758  0.44859    
## W:noise7                    0.0161437  0.0085596   1.886  0.05931 .  
## W:noise8                   -0.0054401  0.0084812  -0.641  0.52125    
## W:noise9                    0.0027495  0.0085067   0.323  0.74654    
## W:noise10                  -0.0228278  0.0084616  -2.698  0.00699 ** 
## W:noise11                  -0.0050631  0.0084807  -0.597  0.55050    
## W:noise12                  -0.0174004  0.0085529  -2.034  0.04192 *  
## W:noise13                   0.0047081  0.0085376   0.551  0.58133    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.4451 on 19932 degrees of freedom
## Multiple R-squared:  0.08169,	Adjusted R-squared:  0.0786 
## F-statistic: 26.46 on 67 and 19932 DF,  p-value: < 2.2e-16
```

### Logistic Regression
#### see: http://www.ats.ucla.edu/stat/r/dae/logit.htm

This code below estimates a logistic regression model using the glm (generalized linear model) function


```r
mylogit <- glm(linear, data = processed.scaled, family = "binomial")
summary(mylogit)
```

```
## 
## Call:
## glm(formula = linear, family = "binomial", data = processed.scaled)
## 
## Deviance Residuals: 
##     Min       1Q   Median       3Q      Max  
## -1.7282  -0.8773  -0.6723   1.2134   2.3855  
## 
## Coefficients:
##                            Estimate Std. Error z value Pr(>|z|)    
## (Intercept)              -0.9353827  0.0180761 -51.747  < 2e-16 ***
## W                         0.4320998  0.0412556  10.474  < 2e-16 ***
## yob                      -0.1416047  0.0173571  -8.158 3.40e-16 ***
## hh_size                   0.0198581  0.0167536   1.185 0.235898    
## sex                      -0.0172625  0.0159417  -1.083 0.278873    
## city                      0.2119919  0.0163018  13.004  < 2e-16 ***
## g2000                    -0.0682720  0.0177151  -3.854 0.000116 ***
## g2002                     0.1971349  0.0189784  10.387  < 2e-16 ***
## p2000                     0.2343965  0.0156943  14.935  < 2e-16 ***
## p2002                     0.2829572  0.0162650  17.397  < 2e-16 ***
## p2004                     0.3613722  0.0163431  22.112  < 2e-16 ***
## totalpopulation_estimate  0.0902184  0.0206555   4.368 1.26e-05 ***
## percent_male             -0.0353626  0.0192544  -1.837 0.066269 .  
## median_age                0.0629920  0.0394324   1.597 0.110161    
## percent_62yearsandover    0.0007437  0.0385075   0.019 0.984591    
## percent_white             0.0643000  0.0422680   1.521 0.128198    
## percent_black             0.0839444  0.0376215   2.231 0.025662 *  
## median_income             0.1384857  0.0371342   3.729 0.000192 ***
## employ_20to64            -0.0435799  0.0226844  -1.921 0.054714 .  
## highschool                0.1212242  0.0576035   2.104 0.035339 *  
## bach_orhigher            -0.0357604  0.0632524  -0.565 0.571829    
## percent_hispanicorlatino -0.0152049  0.0218944  -0.694 0.487390    
## noise1                   -0.0334910  0.0159208  -2.104 0.035413 *  
## noise2                   -0.0139225  0.0159317  -0.874 0.382183    
## noise3                   -0.0310263  0.0159369  -1.947 0.051556 .  
## noise4                    0.0012227  0.0159248   0.077 0.938796    
## noise5                   -0.0013254  0.0159132  -0.083 0.933621    
## noise6                   -0.0116430  0.0159176  -0.731 0.464502    
## noise7                   -0.0031662  0.0159142  -0.199 0.842301    
## noise8                   -0.0074312  0.0159378  -0.466 0.641026    
## noise9                   -0.0056414  0.0159024  -0.355 0.722776    
## noise10                  -0.0131077  0.0159285  -0.823 0.410560    
## noise11                  -0.0113235  0.0159249  -0.711 0.477050    
## noise12                  -0.0032789  0.0159327  -0.206 0.836952    
## noise13                   0.0011919  0.0159465   0.075 0.940419    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 24854  on 19999  degrees of freedom
## Residual deviance: 23203  on 19965  degrees of freedom
## AIC: 23273
## 
## Number of Fisher Scoring iterations: 4
```

### LASSO variable selection + OLS
#### see https://web.stanford.edu/~hastie/glmnet/glmnet_alpha.html and also help(glmnet)

LASSO takes in a model.matrix
First parameter is the model (here it is linear, which we previously created)
Second parameter is the dataframe we want to create the matrix from


```r
linear.train <- model.matrix(linear, processed.scaled.train)[,-1]
linear.test <- model.matrix(linear, processed.scaled.test)[,-1]
linear.train.1 <- model.matrix(linear, processed.scaled.train.1)[,-1]
linear.train.2 <- model.matrix(linear, processed.scaled.train.2)[,-1]

# Use cross-validation to select the optimal shrinkage parameter Lambda and the non-zero coefficients
lasso.linear <- cv.glmnet(linear.train.1, y.train[smplcausal, ], alpha = 1, parallel = TRUE)
```

```
## Warning: executing %dopar% sequentially: no parallel backend registered
```

```r
lasso.linear  # prints the model, information overload, but you can see the mse and the non-zero variables and the cross-validation steps
```

```
## $lambda
##  [1] 0.0675649873 0.0615627001 0.0560936395 0.0511104352 0.0465699250
##  [6] 0.0424327812 0.0386631699 0.0352284404 0.0320988429 0.0292472701
## [11] 0.0266490233 0.0242815975 0.0221244873 0.0201590088 0.0183681380
## [16] 0.0167363633 0.0152495509 0.0138948228 0.0126604451 0.0115357260
## [21] 0.0105109239 0.0095771624 0.0087263537 0.0079511285 0.0072447721
## [26] 0.0066011666 0.0060147371 0.0054804044 0.0049935404 0.0045499281
## [31] 0.0041457250 0.0037774302 0.0034418537 0.0031360889 0.0028574874
## [36] 0.0026036361 0.0023723363 0.0021615844 0.0019695552 0.0017945854
## [41] 0.0016351594 0.0014898963 0.0013575380 0.0012369381 0.0011270519
## [46] 0.0010269277 0.0009356983 0.0008525734 0.0007768332 0.0007078215
## [51] 0.0006449405 0.0005876458 0.0005354410 0.0004878739 0.0004445325
## [56] 0.0004050415 0.0003690587 0.0003362725 0.0003063990 0.0002791794
## [61] 0.0002543778 0.0002317796 0.0002111890 0.0001924275 0.0001753328
## [66] 0.0001597567 0.0001455644 0.0001326328 0.0001208501 0.0001101141
## [71] 0.0001003319
## 
## $cvm
##  [1] 0.2164370 0.2150253 0.2132666 0.2114799 0.2098979 0.2085667 0.2073353
##  [8] 0.2061411 0.2050543 0.2040696 0.2032366 0.2025080 0.2019014 0.2013978
## [15] 0.2009796 0.2006374 0.2003680 0.2001445 0.1999499 0.1997793 0.1996389
## [22] 0.1995197 0.1994200 0.1993376 0.1992584 0.1991719 0.1990870 0.1990103
## [29] 0.1989422 0.1988880 0.1988471 0.1988191 0.1987949 0.1987741 0.1987543
## [36] 0.1987367 0.1987253 0.1987169 0.1987092 0.1986986 0.1986907 0.1986842
## [43] 0.1986819 0.1986828 0.1986861 0.1986904 0.1986968 0.1987034 0.1987104
## [50] 0.1987178 0.1987249 0.1987316 0.1987405 0.1987504 0.1987598 0.1987694
## [57] 0.1987783 0.1987861 0.1987935 0.1987995 0.1988055 0.1988098 0.1988147
## [64] 0.1988178 0.1988219 0.1988253 0.1988287 0.1988320 0.1988353 0.1988377
## [71] 0.1988402
## 
## $cvsd
##  [1] 0.001611549 0.001586998 0.001561894 0.001529922 0.001516629
##  [6] 0.001511596 0.001500523 0.001487773 0.001494992 0.001507803
## [11] 0.001520329 0.001506014 0.001495761 0.001489930 0.001487556
## [16] 0.001489292 0.001493582 0.001496523 0.001497255 0.001498128
## [21] 0.001498423 0.001497734 0.001497363 0.001497815 0.001498764
## [26] 0.001499193 0.001503289 0.001507515 0.001507461 0.001504945
## [31] 0.001503970 0.001503045 0.001502514 0.001501884 0.001501475
## [36] 0.001500601 0.001501109 0.001500888 0.001498229 0.001493660
## [41] 0.001490583 0.001488213 0.001485945 0.001484267 0.001483295
## [46] 0.001482469 0.001481891 0.001481424 0.001481237 0.001480942
## [51] 0.001480682 0.001480233 0.001479977 0.001479989 0.001479802
## [56] 0.001479651 0.001479840 0.001480282 0.001480389 0.001480733
## [61] 0.001481125 0.001481212 0.001481458 0.001481605 0.001482024
## [66] 0.001482186 0.001482324 0.001482520 0.001482621 0.001482668
## [71] 0.001482753
## 
## $cvup
##  [1] 0.2180486 0.2166123 0.2148285 0.2130098 0.2114145 0.2100783 0.2088358
##  [8] 0.2076289 0.2065492 0.2055774 0.2047569 0.2040140 0.2033972 0.2028877
## [15] 0.2024672 0.2021267 0.2018615 0.2016410 0.2014471 0.2012775 0.2011373
## [22] 0.2010175 0.2009174 0.2008354 0.2007572 0.2006711 0.2005903 0.2005178
## [29] 0.2004496 0.2003929 0.2003511 0.2003222 0.2002974 0.2002760 0.2002558
## [36] 0.2002373 0.2002264 0.2002178 0.2002074 0.2001923 0.2001813 0.2001724
## [43] 0.2001678 0.2001671 0.2001694 0.2001729 0.2001787 0.2001849 0.2001917
## [50] 0.2001987 0.2002056 0.2002119 0.2002205 0.2002304 0.2002396 0.2002490
## [57] 0.2002582 0.2002664 0.2002739 0.2002802 0.2002866 0.2002910 0.2002961
## [64] 0.2002994 0.2003039 0.2003075 0.2003110 0.2003145 0.2003179 0.2003204
## [71] 0.2003230
## 
## $cvlo
##  [1] 0.2148255 0.2134383 0.2117047 0.2099500 0.2083813 0.2070552 0.2058348
##  [8] 0.2046533 0.2035593 0.2025618 0.2017163 0.2010020 0.2004056 0.1999078
## [15] 0.1994921 0.1991481 0.1988744 0.1986479 0.1984526 0.1982812 0.1981405
## [22] 0.1980220 0.1979227 0.1978397 0.1977597 0.1976727 0.1975837 0.1975028
## [29] 0.1974347 0.1973830 0.1973432 0.1973161 0.1972923 0.1972722 0.1972528
## [36] 0.1972361 0.1972242 0.1972160 0.1972109 0.1972049 0.1972001 0.1971960
## [43] 0.1971959 0.1971986 0.1972028 0.1972079 0.1972149 0.1972220 0.1972292
## [50] 0.1972368 0.1972442 0.1972514 0.1972605 0.1972704 0.1972800 0.1972897
## [57] 0.1972985 0.1973058 0.1973132 0.1973188 0.1973244 0.1973286 0.1973332
## [64] 0.1973362 0.1973399 0.1973431 0.1973464 0.1973495 0.1973526 0.1973551
## [71] 0.1973575
## 
## $nzero
##  s0  s1  s2  s3  s4  s5  s6  s7  s8  s9 s10 s11 s12 s13 s14 s15 s16 s17 
##   0   2   3   4   4   4   6   6   7   7   7   7   7   7   7   7   7   7 
## s18 s19 s20 s21 s22 s23 s24 s25 s26 s27 s28 s29 s30 s31 s32 s33 s34 s35 
##   9   9   9  10  11  13  15  15  16  17  17  17  19  20  22  24  25  25 
## s36 s37 s38 s39 s40 s41 s42 s43 s44 s45 s46 s47 s48 s49 s50 s51 s52 s53 
##  25  25  26  26  26  26  26  27  27  28  31  31  31  31  31  31  32  32 
## s54 s55 s56 s57 s58 s59 s60 s61 s62 s63 s64 s65 s66 s67 s68 s69 s70 
##  33  33  33  33  33  33  33  33  33  34  34  34  34  34  34  34  34 
## 
## $name
##                  mse 
## "Mean-Squared Error" 
## 
## $glmnet.fit
## 
## Call:  glmnet(x = linear.train.1, y = y.train[smplcausal, ], parallel = TRUE,      alpha = 1) 
## 
##       Df     %Dev    Lambda
##  [1,]  0 0.000000 6.756e-02
##  [2,]  2 0.007614 6.156e-02
##  [3,]  3 0.016030 5.609e-02
##  [4,]  4 0.024590 5.111e-02
##  [5,]  4 0.031890 4.657e-02
##  [6,]  4 0.037940 4.243e-02
##  [7,]  6 0.043940 3.866e-02
##  [8,]  6 0.049850 3.523e-02
##  [9,]  7 0.054880 3.210e-02
## [10,]  7 0.059760 2.925e-02
## [11,]  7 0.063810 2.665e-02
## [12,]  7 0.067180 2.428e-02
## [13,]  7 0.069970 2.212e-02
## [14,]  7 0.072290 2.016e-02
## [15,]  7 0.074220 1.837e-02
## [16,]  7 0.075810 1.674e-02
## [17,]  7 0.077140 1.525e-02
## [18,]  7 0.078240 1.389e-02
## [19,]  9 0.079320 1.266e-02
## [20,]  9 0.080260 1.154e-02
## [21,]  9 0.081050 1.051e-02
## [22,] 10 0.081730 9.577e-03
## [23,] 11 0.082380 8.726e-03
## [24,] 13 0.082960 7.951e-03
## [25,] 15 0.083650 7.245e-03
## [26,] 15 0.084360 6.601e-03
## [27,] 16 0.084990 6.015e-03
## [28,] 17 0.085540 5.480e-03
## [29,] 17 0.086020 4.994e-03
## [30,] 17 0.086420 4.550e-03
## [31,] 19 0.086780 4.146e-03
## [32,] 20 0.087110 3.777e-03
## [33,] 22 0.087390 3.442e-03
## [34,] 24 0.087770 3.136e-03
## [35,] 25 0.088080 2.857e-03
## [36,] 25 0.088410 2.604e-03
## [37,] 25 0.088700 2.372e-03
## [38,] 25 0.088940 2.162e-03
## [39,] 26 0.089140 1.970e-03
## [40,] 26 0.089300 1.795e-03
## [41,] 26 0.089450 1.635e-03
## [42,] 26 0.089560 1.490e-03
## [43,] 26 0.089660 1.358e-03
## [44,] 27 0.089740 1.237e-03
## [45,] 27 0.089810 1.127e-03
## [46,] 28 0.089860 1.027e-03
## [47,] 31 0.089910 9.357e-04
## [48,] 31 0.089960 8.526e-04
## [49,] 31 0.089990 7.768e-04
## [50,] 31 0.090020 7.078e-04
## [51,] 31 0.090050 6.449e-04
## [52,] 31 0.090070 5.876e-04
## [53,] 32 0.090090 5.354e-04
## [54,] 32 0.090100 4.879e-04
## [55,] 33 0.090110 4.445e-04
## [56,] 33 0.090120 4.050e-04
## [57,] 33 0.090140 3.691e-04
## [58,] 33 0.090150 3.363e-04
## [59,] 33 0.090170 3.064e-04
## [60,] 33 0.090180 2.792e-04
## [61,] 33 0.090190 2.544e-04
## [62,] 33 0.090190 2.318e-04
## [63,] 33 0.090200 2.112e-04
## [64,] 34 0.090200 1.924e-04
## [65,] 34 0.090210 1.753e-04
## [66,] 34 0.090210 1.598e-04
## [67,] 34 0.090220 1.456e-04
## [68,] 34 0.090220 1.326e-04
## [69,] 34 0.090220 1.209e-04
## [70,] 34 0.090230 1.101e-04
## [71,] 34 0.090230 1.003e-04
## [72,] 34 0.090230 9.142e-05
## [73,] 34 0.090230 8.330e-05
## [74,] 34 0.090230 7.590e-05
## [75,] 34 0.090230 6.915e-05
## [76,] 34 0.090230 6.301e-05
## [77,] 34 0.090230 5.741e-05
## [78,] 34 0.090240 5.231e-05
## 
## $lambda.min
## [1] 0.001357538
## 
## $lambda.1se
## [1] 0.01389482
## 
## attr(,"class")
## [1] "cv.glmnet"
```


```r
plot(lasso.linear)
```

![](Metrics-ML_Part_1_files/figure-html/unnamed-chunk-17-1.png)<!-- -->


```r
lasso.linear$lambda.min
```

```
## [1] 0.001357538
```

```r
lasso.linear$lambda.1se
```

```
## [1] 0.01389482
```

Lambda.min gives min average cross-validated error
Lambda.lse gives the most regularized model such that error is within one standard error of the min; this value of Lambda is used here


```r
# List non-zero coefficients found. There are two ways to do this.
# coef(lasso.linear, s = lasso
```





