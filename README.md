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

## Data scrubbing
Check notebook "data_scrubbing" for descriptive statistics and full data scrubbing. 

## Business Recommendations

### 1. Seller/Prediction
*“The agency gave me an estimate on that house I want to sell, but I’d love a second opinion. Where can I find it?”* <br/>

For a typical house in King County, worth $450k, Realestimate predicts a price with an error of ±$40k around its real value.
### 2. Buyer/Inference
*"I want to invest buying a new house. What are the features that matter the most to maximise my investment?"*
![](/data/images/multilinear-feature_importance-barplot.png)

### 3. Realestimate as a startup
*“I want to do an MVP of a real-estate price predictor. Is it possible to reach an accuracy close to industry standards with just an MVP?“*


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
