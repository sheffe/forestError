## forestError: Individualized Prediction Errors and Intervals for Random Forests
Benjamin Lu and Johanna Hardin

### Overview
The `forestError` package estimates conditional mean squared prediction errors and conditional prediction intervals for random forests predictions as introduced in Lu and Hardin (2019) (in preparation). Because these estimates are conditional on the test observations' covariate values, each estimate is individualized; in other words, each squared error and interval estimate is specific to each test observation. Simulation results suggest that these prediction intervals are narrower than those obtained from quantile regression forests and conformal inference and substantially less computationally expensive than prediction intervals obtained via conformal inference.

This package builds on top of the popular `randomForest` package.

### Installation

Running the following lines of code in `R` will install this package from Github:

```{r}
library(devtools)
devtools::install_github(repo="benjilu/forestError")
```  

### Instructions
See `documentation.pdf` for information on how to use this package. A portion of the example given in the documentation is reproduced below for convenience.

```{r}
# load data
data(airquality)

# remove observations with missing predictor variable values
airquality <- airquality[complete.cases(airquality), ]

# get number of observations and the response column index
n <- nrow(airquality)
response.col <- 1

# split data into training and test sets
train.ind <- sample(1:n, n * 0.9, replace = FALSE)
Xtrain <- airquality[train.ind, -response.col]
Ytrain <- airquality[train.ind, response.col]
Xtest <- airquality[-train.ind, -response.col]
Ytest <- airquality[-train.ind, response.col]

# fit random forest to the training data
rf <- randomForest::randomForest(Xtrain, Ytrain, nodesize = 10, ntree = 500,
                                 keep.inbag = TRUE)

# get conditional mean squared prediction errors and prediction
# intervals for the test observations
test.preds <- quantForestError(rf, Xtrain, Xtest, alpha = 0.05)
```

### License
See `DESCRIPTION` for information.

### References
* B. Lu and J. Hardin. Individualized prediction errors and intervals for random forests. In preparation, 2019+.
* J. Lei, M. G’Sell, A. Rinaldo, R.J. Tibshirani, and L. Wasserman. Distribution-free predictive inference for regression. Journal of the American Statistical Association, 113: 1094-1111, 2018.
* N. Meinshausen. Quantile regression forests. Journal of Machine Learning Research, 7:983–999, 2006.
