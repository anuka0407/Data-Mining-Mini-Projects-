Data Mining Final Project
================
Anuka Revi
4/13/2021

``` r
#load data 
xsale_train <- read.csv("C:/Users/anuka/Desktop/Spring 2021/ECO 395M - DATA MINING/Data/train.csv")
xsale_test <- read.csv("C:/Users/anuka/Desktop/Spring 2021/ECO 395M - DATA MINING/Data/test.csv")
```

# **Health Insurance Cross Sell Prediction**

## **Abstract**

summary of our questions , methods, results, and conclusions in a few
100 words

## **Introduction**

  - We are looking at the company that specializes in health as well as
    vehicle insurance. The goal of this project is to predict whether
    current customers (health insurance policyholders) would be
    interested to also purchase a vehicle insurance. Since Maria and I
    both worked at insurance firms, we know that there is a tremendous
    effort by insurance companies to reach out to their customers and
    cross-sell various other products.There is a huge cost in sending
    out paper mail, meetings customers to go over and offer products
    they might not be willing to buy at all. Building a model to predict
    the probability that the person would be interested to buy the
    product, such as a vehicle insurance, companies will be able to
    optimize their business strategies and target the right customers to
    successfully cross-sell to increase profits .

## **Methods**

  - describe your data sets and methods u ll be using to analyze it

Now, in order to predict, whether the customer would be interested in
Vehicle insurance, you have information about demographics (gender, age,
region code type), Vehicles (Vehicle Age, Damage), Policy (Premium,
sourcing channel) etc. Evaluation metric will be ROC curve and test set
RMSE.

|      Variable       |                                                         Definition                                                          |
| :-----------------: | :-------------------------------------------------------------------------------------------------------------------------: |
|         id          |                                                 Unique ID for the customer                                                  |
|       Gender        |                                                   Gender of the customer                                                    |
|         Age         |                                                     Age of the customer                                                     |
|  Driving\_License   |                                 0 : Customer does not have DL, 1 : Customer already has DL                                  |
|    Region\_Code     |                                         Unique code for the region of the customer                                          |
| Previously\_Insured |                   1 : Customer already has Vehicle Insurance, 0 : Customer doesn’t have Vehicle Insurance                   |
|    Vehicle\_Age     |                                                     Age of the Vehicle                                                      |
|   Vehicle\_Damage   |     1 : Customer got his/her vehicle damaged in the past. 0 : Customer didn’t get his/her vehicle damaged in the past.      |
|   Annual\_Premium   |                                   The amount customer needs to pay as premium in the year                                   |
| PolicySalesChannel  | Anonymized Code for the channel of outreaching to the customer ie. Different Agents, Over Mail, Over Phone, In Person, etc. |
|       Vintage       |                                Number of Days, Customer has been associated with the company                                |
|      Response       |                                 1 : Customer is interested, 0 : Customer is not interested                                  |

## **Results**

tables figs and texst that illustrate your findings. (4 -6 figures and
tables do not incuudefog or table if you do not discuss it in a text)

## **Conclusion**

interpret what you founds, man lessons we should take away from your
report

## **Appendix**

optional. any details figs

-----

\#\#\#Data Worfklow

``` r
library(tidyverse)
library(janitor)
library(xgboost)
library(skimr)



xsale_train=xsale_train%>%
   janitor::clean_names() %>% 
  janitor::remove_empty(which = "rows")

dim(xsale_train)
```

    ## [1] 381109     12

``` r
names(xsale_train)
```

    ##  [1] "id"                   "gender"               "age"                 
    ##  [4] "driving_license"      "region_code"          "previously_insured"  
    ##  [7] "vehicle_age"          "vehicle_damage"       "annual_premium"      
    ## [10] "policy_sales_channel" "vintage"              "response"

``` r
skim(xsale_train)
```

|                                                  |              |
| :----------------------------------------------- | :----------- |
| Name                                             | xsale\_train |
| Number of rows                                   | 381109       |
| Number of columns                                | 12           |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |              |
| Column type frequency:                           |              |
| character                                        | 3            |
| numeric                                          | 9            |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |              |
| Group variables                                  | None         |

Data summary

**Variable type: character**

| skim\_variable  | n\_missing | complete\_rate | min | max | empty | n\_unique | whitespace |
| :-------------- | ---------: | -------------: | --: | --: | ----: | --------: | ---------: |
| gender          |          0 |              1 |   4 |   6 |     0 |         2 |          0 |
| vehicle\_age    |          0 |              1 |   8 |   9 |     0 |         3 |          0 |
| vehicle\_damage |          0 |              1 |   2 |   3 |     0 |         2 |          0 |

**Variable type: numeric**

| skim\_variable         | n\_missing | complete\_rate |      mean |        sd |   p0 |   p25 |    p50 |    p75 |   p100 | hist  |
| :--------------------- | ---------: | -------------: | --------: | --------: | ---: | ----: | -----: | -----: | -----: | :---- |
| id                     |          0 |              1 | 190555.00 | 110016.84 |    1 | 95278 | 190555 | 285832 | 381109 | ▇▇▇▇▇ |
| age                    |          0 |              1 |     38.82 |     15.51 |   20 |    25 |     36 |     49 |     85 | ▇▃▃▂▁ |
| driving\_license       |          0 |              1 |      1.00 |      0.05 |    0 |     1 |      1 |      1 |      1 | ▁▁▁▁▇ |
| region\_code           |          0 |              1 |     26.39 |     13.23 |    0 |    15 |     28 |     35 |     52 | ▃▂▇▃▃ |
| previously\_insured    |          0 |              1 |      0.46 |      0.50 |    0 |     0 |      0 |      1 |      1 | ▇▁▁▁▇ |
| annual\_premium        |          0 |              1 |  30564.39 |  17213.16 | 2630 | 24405 |  31669 |  39400 | 540165 | ▇▁▁▁▁ |
| policy\_sales\_channel |          0 |              1 |    112.03 |     54.20 |    1 |    29 |    133 |    152 |    163 | ▅▁▁▃▇ |
| vintage                |          0 |              1 |    154.35 |     83.67 |   10 |    82 |    154 |    227 |    299 | ▇▇▇▇▇ |
| response               |          0 |              1 |      0.12 |      0.33 |    0 |     0 |      0 |      0 |      1 | ▇▁▁▁▁ |
