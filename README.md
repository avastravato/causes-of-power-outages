# Introduction

From 2000-2016, Purdue compiled a dataset documenting major power outages across the United States. Each row corresponds to an outage instance, including details like location, duration, cause category, and the affected NERC (North American Electric Reliability Corporation) region. Using these features, I will be investigating the following:

> ### What characteristics are most strongly associated with various causes of major power outages?

My dataset has **1534** entries, and the following **10** features will be relevant to my analysis:

- **`YEAR`**: indicates the year when the outage event occurred [2000,2016].
- **`MONTH`**: indicates the month when the outage event occurred [1,12].
- **`U.S._STATE`**: indicates the U.S. state where the outage event occurred.
- **`NERC.REGION`**: the NERC region involved in the outage event.
- **`OUTAGE.START.DATE`**: indicates the day of the year when the outage event started.
- **`OUTAGE.START.TIME`**: indicates the time of the day when the outage event started.
- **`OUTAGE.RESTORATION.DATE`**: indicates the day of the year when power was restored to all customers.
- **`OUTAGE.RESTORATION.TIME`**: indicates the time of the day when power was restored to all customers.
- **`CAUSE.CATEGORY`**: categories of all the events causing the major power outages.
- **`OUTAGE.DURATION`**: duration of an outage event (in minutes).

# Data Cleaning and Exploratory Data Analysis
## Data Cleaning
Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).

## Univariate Analysis
Embed at least one plotly plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions). Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present, and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one univariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

## Bivariate Analysis
Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

## Interesting Aggregates
Embed at least one grouped table or pivot table in your website and explain its significance.

## Imputation
If you imputed any missing values, visualize the distributions of the imputed columns before and after imputation. Describe which imputation technique you chose to use and why. If you didn’t fill in any missing values, discuss why not.

# Framing a Prediction Problem
Clearly state your prediction problem and type (classification or regression). If you are building a classifier, make sure to state whether you are performing binary classification or multiclass classification. Report the response variable (i.e. the variable you are predicting) and why you chose it, the metric you are using to evaluate your model and why you chose it over other suitable metrics (e.g. accuracy vs. F1-score).
Note: Make sure to justify what information you would know at the “time of prediction” and to only train your model using those features. For instance, if we wanted to predict your Final Exam grade, we couldn’t use your Final Project grade, because we (probably) won’t have the Final Project graded before the Final Exam! Feel free to ask questions if you’re not sure.

# Baseline Model
Describe your model and state the features in your model, including how many are quantitative, ordinal, and nominal, and how you performed any necessary encodings. Report the performance of your model and whether or not you believe your current model is “good” and why.
Tip: Make sure to hit all of the points above: many Final Projects in the past have lost points for not doing so.

# Final Model
State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process.
Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.
Optional: Include a visualization that describes your model’s performance, e.g. a confusion matrix, if applicable.