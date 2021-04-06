**Q1 - What Causes What?**
==========================

1.  When we try to understand how more cops in the street affect crime,
    we run into the problem of correlation vs causation: Do more cops
    get assigned to high crime zones, or does presence of more cops
    decrease crime rate.(In other words: Is it a crime effect on size of
    police, are they simply correlated or police effect on crime rate
    which explains causality)

2.  In order to isolate this effect researchers at Upenn tried to find
    example of having a lot of police for reasons unrelated to crime and
    they found that terrorism alert system in D.C. would work well for
    this experiment, since assigning larger size of police due to high
    terror alert level is unrelated to street crimes. This allowed them
    to look at the effect of larger size of police on street crime and
    helped them establish a causal relationship between more police and
    less crime in D.C. Specifically daily total \# of crime goes down by
    7.316 (unspecified units).Researchers also tried to see whether
    there are less tourists visiting D.C. on high terror alert days
    which would create fewer victims and decreased crime rate by looking
    at ridership level on the Metro system and they found that redirship
    was not dimisnihed on high terror days and that number of victims
    were largely unchanged - 6.046 (se of 2.537) decrease while
    controlling for metro ridership vs 7.316 (2.877) when
    not-controlling for the ridership.

3.  They controlled for metro ridership to test fewer tourist hypothesis
    on high terrorism alert days in D.C. If there were decreased amount
    of tourists during high alert days, it would naturally create fewer
    victims and crimes and would weaken the causality of increased
    police size on decreased crime rate. Controlling for ridership
    diminished the effect (-6.046 vs -7.316) slight but the effect
    remained statistically significant.

4.  The model researchers are trying to estimate is linear model with
    interactions with crimes incidents in the first police district and
    in other districts. &gt;Based on figure 2, daily total number of
    crimes in D.C. decreased by 2.621 in District 1 and the effects is
    significant, but it only decreased by 0.571 in other districts
    (effect is not significant). This makes sense since increased police
    force is used in District 1 on high alert days, which again
    strengthens the causality effect of increased police on reduced
    crime rate.

*C**r**i**m**e* = *b*<sub>0</sub> + *b*<sub>1</sub> \* (*H**i**g**h**A**l**e**r**t* \* *D**i**s**t**r**i**c**t*<sub>1</sub>) + *b*<sub>2</sub> \* (*H**i**g**h**A**l**e**r**t* \* *D**i**s**t**r**i**c**t*<sub>0</sub>) + *b*<sub>3</sub> \* *l**o**g*(*m**i**d**d**a**y*<sub>*r*</sub>*i**d**e**r**s**h**i**p*) + *e*

**Q2 - Predictive Model Building: Green Certification**
=======================================================

In this project, we are trying to build the best predictive model for
revenue per square foot per calendar year (prediction target:
rev\_sqft\_year given its features such as size, age, class, renovation
and so on), and to use this model to quantify the average change in
rental income per square foot associated with green certification,
holding all else fixed. We will use RMSE as the evaluation metric as we
will be using Random Forest model which is immune to outliers.

I use Random forests because it is easy to prepare, requires no scaling
or normalization for categorical, numerical or binary features & it is a
good indicator of the most important features.

Fig1 shows the distribution for *revenue / sq.ft.* variable for green vs
non-green buildings. There are more non-green buildings than green
buildings within each revenue category and the distribution is skewed to
the right.

Simple Random forest model I used includes all variables and has RMSE of
on the test data that is 734. This is the number we want to improve on
(make lower) by feature engineering.

Fig2 shows relative importance of each feature in calculating revenue
per square foot per calendar year. We observe that the most important
characteristics of revenue per sq foot/year are age and size; size makes
intuitive sense since bigger space translates into higher price. Based
on the fig 2, I decided to include variables that are at least 10 and up
on importance (X scale of fig2).

Using above selection renders a better prediction results. Test set RMSE
is 697 which is lower than the initial model. Figure 3 shows the
relative importance of the updated model that better predicts the
revenue per square foot and figure 4 plots predicted vs actual outcomes.

In conclusion, Random Forest model that includes *“age”, “size”,
“stories”, “city\_market\_rent”, “amenities”, “cs\_property\_id”,
“class\_a”, “class\_b”, “cd\_total\_07”,
“electricity\_costs”,“renovated”, “cluster”, “precipitation”,
“empl\_gr”, “green\_certified”,“rev\_sqft\_yr”* is a good predictor for
the revenue per square foot per year.

![Figure1](DM_HW3_files/figure-markdown_strict/unnamed-chunk-2-1.png)

![Figure2](DM_HW3_files/figure-markdown_strict/unnamed-chunk-5-1.png)

![Figure3](DM_HW3_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    ## [1] 2409.763

![Figure4](DM_HW3_files/figure-markdown_strict/unnamed-chunk-8-1.png)

**Q3 - Predictive Model Building: California Housing**
======================================================

In this project we are trying to build the best predictive model for
medianHouseValue for California Housing. The data in CAhousing.csv
containts data at the census-tract level on residential housing in the
state of California. Each row is a census tract, and the columns are as
follows: longitude, latitude, housingMedianAge, population, households,
totalRooms, totalBedrooms, medianIncome & median house value. I will be
using forward\_backward stepwise model with model selection criteria AIC
and adj. R^2. We are trying to minimize AIC and maximize R-squared to
get the best model.

Fig1 below displaces a plot of the original data, using a color scale to
show medianHouseValue versus longitude (x) and latitude (y). Map shows
that houses located inland have lower median house value, while coastal
houses have higher values. Also around cities like Los Angeles and Santa
Cruz houses attain the given maximum value of $500 000.

Table1 shows that out of 3 initial models (linear models using all
variables, 2-way interactions and 3-way interactions) the best (lowest)
AIC score was for a 3-way interaction model, however this model has 93
coefficients that makes a very complex model. To mitigate complexity we
used stepwise selection for a 2-way model as our final model for price
predictions since it has far lower coefficients (33 vs 93) and only
slighty worse performace to the 3-way model (only 0.23% decrease in
AIC).

In order to evaluate how well our final model(step\_2way\_model)
performs on test set data, we use the RMSE measures of errors, which
gives us more accurate results. RMSE for the test set is 65296.4. Fig 2
shows the predicted house values vs longitude and latitude and fi3 shows
the errors ( how much we underestimated or overestimated) the value.
From the final map (fig3), we see that our model correctly estimated
most of the prices for inland housing in California. We do have under
estimated prices along the shores around Los Angeles and San Diego &
overestimated around Santa Barbara.

![Fig1](DM_HW3_files/figure-markdown_strict/California%20Map-1.png)

<table>
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:center;">
Total.Predictors
</th>
<th style="text-align:right;">
AIC
</th>
<th style="text-align:center;">
Adj.R.Squared
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
Full Model
</td>
<td style="text-align:center;">
9
</td>
<td style="text-align:right;">
368297.4
</td>
<td style="text-align:center;">
0.6350148
</td>
</tr>
<tr>
<td style="text-align:left;">
Two-Way Int. Model
</td>
<td style="text-align:center;">
37
</td>
<td style="text-align:right;">
366161.5
</td>
<td style="text-align:center;">
0.6798397
</td>
</tr>
<tr>
<td style="text-align:left;">
Three-Way Int. Model
</td>
<td style="text-align:center;">
93
</td>
<td style="text-align:right;">
365312.5
</td>
<td style="text-align:center;">
0.6969088
</td>
</tr>
</tbody>
</table>

![fig2](DM_HW3_files/figure-markdown_strict/unnamed-chunk-16-1.png)

![Fig3](DM_HW3_files/figure-markdown_strict/unnamed-chunk-18-1.png)
