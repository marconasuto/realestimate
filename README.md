# Realestimate
A data-driven tool   to evaluate proprieties in King County, WA with <a href="https://andreanasuto.carto.com/builder/8a296e60-38d8-4fb2-b4e1-418884819758/embed">geospatial analysis (GIS) on housing affordability <a/>. **Performance close to Zillow's estimates for off-market houses.**
Performance close to Zillow's estimates for off-market houses.
![](/data/images/Screenshot-2020-06-30-at-19.32.02.png)
![](/data/images/starplot-performance-comparison-bestmodel-vs-zillow.png)

**Best model off-market performance**
<br/>
<br/>
![](/data/images/king-county-houses-best-model-performance.png)
<br/>
<br/>
**Zillow's off-market performance for King County**
<br/>
<br/>
![](/data/images/zestimates-kingcounty-performance.png)

## The task
*"For this project, you'll be working with the King County House Sales dataset. We've modified the dataset to make it a bit more fun and challenging. The dataset can be found in the file "kc_house_data.csv", in this repo.*

*The description of the column names can be found in the column_names.md file in this repository. As with most real world data sets, the column names are not perfectly described, so you'll have to do some research or use your best judgment if you have questions relating to what the data means.*

***You'll clean, explore, and model this dataset with a multivariate linear regression to predict the sale price of houses as accurately as possible***

 
***Based on the results of your models, your presentation should discuss at least two concrete features that highly influence housing prices****."

## Methodology 
For this project we first focused on Business Understanding. During this phase we:

- Got some domain knowledge on the real-estate business

- Formulated some questions of business relevance that we want to address with our regression analysis

 

For the regression analysis we followed the well-known methodology called Obtain Scrub Explore Model iNterpret - OSEMN

## Business Understanding
Very briefly: we know that the evaluation of a house when selling or buying it, depends on:

- The geographical feature (location) of the house

- Its physical features (square foot, number of bedrooms, bathrooms, floors etc.)

- Its grading

 

However, we don't know effectively how these features affect house prices analytically. We see this regression analysis as an opportunity to come up with recommendations for different stakeholders:

- **Buyers**

- **Sellers**

- **Realestimate as a startup** 

- **Urban developers**

<br/>

**4 main stakeholders, 6 questions to address**:

 

**Seller/Prediction**

1. “The agency gave me an estimate on that house I want to sell, but I’d love a second opinion. Where can I find it?”


**Buyer/Inference**

2. "I want to invest buying a new house. What are the features that matter the most to maximise my investment?"


**Realestimate as a startup**

3. “I want to do an MVP of a real-estate price predictor. Is it possible to reach an accuracy close to industry standards with just an MVP?“


**Urban developer**

4.1. Where and if is it affordable for a lower income class member (median lower income) to buy a house in King County?

4.2. What about for a middle income class member (median middle income)?

4.3. What about for an upper income class member (median upper income)?”

## Models
For learning reasons, we tried to map dozens of models in order to:

1. See the effect of certain techqniques and design choices 

2. Try to describe the phenomenon with models not the other way round (in other words, trying to be as agnostic as possible towards the phenomenon):  there are a lot of examples online of multilinear regression on predicting the price in King County using the same dataset we worked on. Since there are clear issues, as we will see, with outliers and non-linearity of pricing, the typical approach that we've seen, which we tried to avoid (although with limitations due to knowledge), is to linearise the problem, manipulating the dataset so that a multilinear regression "turns to be" adequate enough.

3. Select the best model based also on a good interpretrability (rather than just evaluating on standard metrics to assess regression models)

4. Select the best model based on accuracy

 

Metrics used to assess models:

 

- Adj. R^2

- Train and test RMSE

- 10-fold cross validation mean R^2

- AIC

- Residual standard error (RSE): the average amount that the response will deviate from the true regression line

 

On the other hand we wanted to go beyond the metrics and measure the linearity of the model with:

 

- qq-plots and residuals histograms (normality)

- Train and test residuals plots (heteroskedasticity)

- Partial-residual plots 

- Influence plots (outliers)

- Prediction intervals plots

 

When looking at qq-plots and heteroskedasticity, it is clear that the models show issues with linearity as we have:

- Heavy tails qq-plots

- Funnel-shaped heteroskedasticity

 

We adopted design patterns in order to "map" several models according to conditions. The hierarchy used is:

 

1. First split: 

   1.1. Dataset with outliers

   1.2 Dataset without ouliers

<br/> 

    2. Second split: for each dataset in 1)

       2.1. Dataset without multicolinearity removed

       2.2. Dataset with multicolinearity removed using correlation matrix

       2.3 Dataset with multicolinearity removed using VIF

 <br/>

       3. Third split: for each dataset in 2)

          3.1. Categorical-level-threshold < 29

          3.2 Categorical-level-threshold < 6


          4. Fourth split: for each dataset in 3) 

             4.1. Log-transformed continous variables

             4.2. Log-transformed + MinMax scaling

             4.3. Log-transformed + Robust Scaling

             4.4. Log-transfomred + Standard Scaling
## Data scrubbing
Check notebook "data_scrubbing" for descriptive statistics and full data scrubbing. 

## Business Recommendations

### 1. Seller/Prediction
*“The agency gave me an estimate on that house I want to sell, but I’d love a second opinion. Where can I find it?”* <br/>

For a typical house in King County, worth $450k, Realestimate predicts a price with an error of ±$40k around its real value.

### 2. Buyer/Inference
*"I want to invest buying a new house. What are the features that matter the most to maximise my investment?"*
![](/data/images/multilinear-feature_importance-barplot.png)
1. Clustered locations (zipcodes)

2. log-Median-Household-Income: how to interpret this variable - let's take the median household income by income tier in Washington (source: Pew data). 

 

      Lower income: $26,436.00

      Middle income: $80,615.00

      Upper income: $188,102.00



      Upper income/middle income = 2.33

      Upper income/ lower income =  7.11

      Middle income/ lower income = 3.05



      Median increase in housing price considering all other variables fixed:



      Lower income tract to middle income tract: $67,103  

      Lower income tract to upper income tract: $155,606

      Middle income tract to upper income tract: $88,464

 

 

3. Grade: an increase of 1 rank for a house means an increase of the median price of $106,233, considering all other variables fixed


4. log-bathrooms: an increase of 1 bathroom means a median increase of the price of $22458, considering all other variables fixed

5. View 2.0

6. log-sqft_living15: an increase of 50% of the sqft living surface of the 15 nearest houses around the selected proprietry, considering all other variables fixed, means a median increase of the price of $7,967

7. log-bedrooms: an increase of 1 bedroom means a median increase of the price of $11,392,  considering all other variables fixed 

8. floors: one extra floor means an increase of the median price of $12329,  considering all other variables fixed 

9. condition: an increase of 1 rank for a house means an increase of the median price of $11,035 , considering all other variables fixed
### 3. Realestimate as a startup
*“I want to do an MVP of a real-estate price predictor. Is it possible to reach an accuracy close to industry standards with just an MVP?“*
<br/>
Although doing a comparison with Zillow is not straight forward, we can still make some benchmarking. Let's take Zillow's official data for King County off-markets homes (available downloading the excel sheet on their official page).

We reported in the table below our model performance using Zillow official performance parameters defiitnions. 

As we can see, our model performance is very satisfying considering the difference in terms of technical and economical means, knowledge and experience of Zillow's data science team w.r.t mine. Our average median error (training and test set) is only 28.9% less accurate than Zillow's. As far as known at the time this report has been finalised (June 2020), it is one of the best performance available online for regression on King County Housing dataset.

**Best model off-market performance**
<br/>
<br/>
![](/data/images/king-county-houses-best-model-performance.png)
<br/>
<br/>
**Zillow's off-market performance for King County**
<br/>
<br/>
![](/data/images/zestimates-kingcounty-performance.png)

### 4. Urban developer

*4.1. Where and if is it affordable for a lower income class member (median lower income) to buy a house in King County?*

*4.2. What about for a middle income class member (median middle income)?*

*4.3. What about for an upper income class member (median upper income)?”*
<br/>
The rule of thumb for **PIR (Price-to-Income-Ratio)** is that the housing price should not exceed three times (3.0x) to the median gross annual household income (median multiplier) (*Suhaida et al., 2011;Sani and Rahim, 2015*).
<br/>
According to <a href="https://www.bloomberg.com/news/articles/2018-05-29/how-many-years-of-income-does-a-home-in-your-city-cost"> Bloomberg <a/>, it should be 2.6 years of income.
<br/>
<br/>
Based on this information, we selected 3 affordability levels per income class:

- Very affordable, if   0 <= PIR < 2.6

- Affordable, if  2.6 <= PIR < 3

- Not affordable, if PIR > 3 
<br/>
<br/>
Let's take the median household income by income tier in Washington (source: Pew data). 

- Lower median income: $26,436.00

- Middle median income: $80,615.00

- Upper median income: $188,102.00
<br/>
 

1. We mapped all proprietries by associating their Median Household Income value per tract which they belong to, using King County Census data.

2. We binned and labeled proprietries by income category, leveraging on the official income tiers above

3. For each proprietry, we obtained different PIR, so to obtain different points of views/filters on houses in King County:

   3.1. PIR relative to the Median Household Income of the tract which they belong to

   3.2 PIR relative to the Median Household Income of Lower class

   3.3. PIR relative to the Median Household Income of Middle class

   3.4.  the Median Household Income of Upper class

<br/>
4. We applied affordability thresholds from Bloomberg and Suhaida et al., 2011;Sani and Rahim, 2015, so to address our initial questions regarding affordabikity per income level

5. We obtained percentages and absolute number of proprietries binned to the different affordability levels explained above.

6. We plotted using CARTOdb our results

<br/>
**Affordability for lower income class**

Only 1 proprietry was classified as affordable. King County is not affordable for anybody who has a median lower income.

<br/> 

**Affordability for middle income class**

90.7% of the proprietries in King County are not affordble for anybody who has a median middle income.

<br/>

**Affordability for upper income class**

33.5% of the proprietries in King County are not affordable even for anybody who has a median upper income, although 56% of the houses are actually considerable very affordable for the same category.
**King County is definitely a place for rich and extremely-rich landlords**
