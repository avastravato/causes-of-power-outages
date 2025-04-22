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
To clean the data, I began by converting missing values from `'NA'` to `'nan'` representation for consistency across missing values. Then, I worked on conbining date & time columns into single Datetime columns. So, `OUTAGE.START.DATE` and `OUTAGE.START.TIME` are combined into `OUTAGE.START`, and `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` combine into `OUTAGE.RESTORATION`. I then chose to drop the original 4 columns from the dataset.

Then, I one hot encoded cause categories. We have seven possible cause categories, so an additional seven features were created (example column name: `cause==severe weather`). I kept the original column so we can one hot encode it again within our pipeline, but it's valuable for EDA and will help us impute values.

Next, I looked at the feature `NERC.REGION`. After some initial research, I learned there are six main NERC regions across the United States. However, collecting value counts from this feature showed that my dataset had 14 possible values. So, I investigated the region names I did not recognize, and learned that most subregions that belong to one of the six main regions. I strategically mapped these subregions to the main regions; this way, I'll be able to consider regional trends' impact on the cause of an outage.

Lastly, I had to handle missing values in the dataset. I began by dropping entries where both `OUTAGE.START` and `OUTAGE.RESTORATION` were missing, this only accounted for 9 entries (0.587% of the original data) so dropping these entries poses no concern for data generalization. After this, there were 49 remaining entries with missing values in `OUTAGE.DURATION` and `OUTAGE.RESTORATION`. I imputed these values with the following methods:
1. Compute mean duration for each cause category.
2. Iterate over the entire dataset, and fill missing duration values with the entry's cause category mean value.
3. Compute restoration time using `OUTAGE.START` and `OUTAGE.DURATION`.
See *imputation* for a visualization describing how these imputed values modify our dataset's statistics.

These data cleaning steps leave us with a dataset like this:
```
YEAR  MONTH U.S._STATE      CAUSE.CATEGORY  OUTAGE.DURATION        OUTAGE.START  OUTAGE.RESTORATION  cause==severe weather  cause==intentional attack  cause==system operability disruption  cause==equipment failure  cause==public appeal  cause==fuel supply emergency  cause==islanding NERC.REGION
0  2011    7.0  Minnesota      severe weather           3060.0 2011-07-01 17:00:00 2011-07-03 20:00:00                      1                          0                                     0                         0                     0                             0                 0         MRO
1  2014    5.0  Minnesota  intentional attack              1.0 2014-05-11 18:38:00 2014-05-11 18:39:00                      0                          1                                     0                         0                     0                             0                 0         MRO
2  2010   10.0  Minnesota      severe weather           3000.0 2010-10-26 20:00:00 2010-10-28 22:00:00                      1                          0                                     0                         0                     0                             0                 0         MRO
3  2012    6.0  Minnesota      severe weather           2550.0 2012-06-19 04:30:00 2012-06-20 23:00:00                      1                          0                                     0                         0                     0                             0                 0         MRO
4  2015    7.0  Minnesota      severe weather           1740.0 2015-07-18 02:00:00 2015-07-19 07:00:00                      1                          0                                     0                         0                     0                             0                 0         MRO
```

## Univariate Analysis

<iframe
src='assets/outages_region.html'
width='800'
height='600'
frameborder='0'
></iframe>
We see here that NERC regions WECC and RFC contribute almost 2/3 of the reported outages. We should investigate regional trends further, and determine if certain causes correspond to some region.

<iframe
src='assets/outages_month.html'
width='800'
height='600'
frameborder='0'
></iframe>
Here, we investigate seasonal trends. Observe that the most outages occur in the summer months (Jun-Aug), with additional peaks in the winter months (Dec-Feb). This tells us that there is a definite relationship between the month of the outage and the cause of the outage, and we should create features to clearly show this relationship.

## Bivariate Analysis
Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

<iframe
src='assets/outages_month_cause.html'
width='800'
height='600'
frameborder='0'
></iframe>
Here, we see that the cause category 'Sever Weather' is the most common category for each month, excluding March. This category dominates the visual, but I also notice other trends: the category 'Public Appeal' is most likely to occur in the summer months, and outages caused by Intentional Attacks are more common in the first half of the year.

## Interesting Aggregates
<iframe
src='assets/outages_region_cause.html'
width='800'
height='600'
frameborder='0'
></iframe>
Here, we gain a few more valuable insights. The region WECC is most likely to face outages due to Intentional Attacks, while region RFC is dominated by Severe Weather outages.

## Imputation
As mentioned in the data cleaning section, I chose to impute missing values for `OUTAGE.DURATION` using the mean duration values from outages with the same cause category. This table describes summary statistics for `OUTAGE.DURATION` before and after mean imputation.
```
Before Imputation  After Imputation  Change
count            1476.00           1525.00   49.00
mean             2625.40           2693.66   68.26
std              5942.48           5931.29  -11.20
min                 0.00              0.00    0.00
25%               102.25            108.00    5.75
50%               701.00            728.87   27.87
75%              2880.00           3000.00  120.00
max            108653.00         108653.00    0.00
```

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