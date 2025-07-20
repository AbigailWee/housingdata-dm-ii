# Data Pre-processing, Feature Engineering, and Hypothesis Testing for King County Housing Data

#### (PART II)

## 📚 Introduction

An extension of the work done in [PART I](https://github.com/Weetynn/housingdata-dm-i.git), detailing the data preparation steps for further analysis of the King County housing dataset. This includes data pre-processing, feature engineering, and testing of five key hypotheses to explore how attributes—namely square footage, bedrooms, house age, lot utilization, and view ratings—influence house prices.

## 🗒️ Main Areas of Discussion

### 1.0 Related Work

#### 📌 1.1 Literature Review

    ▪️ The literature review examines key factors influencing real estate prices, focusing on both internal and external factors.  
    
    ▪️ Internal factors include structural features of the property, such as the number of bedrooms, bathrooms, total square footage, condition, and age of the property.  
    
    ▪️ External factors include location, proximity to amenities like schools and public transport, and neighborhood characteristics such as population density and social status.  
    
    ▪️ Previous studies are referenced to highlight how housing attributes affect property prices.  
    
    ▪️ The findings from the literature are used as benchmarks for validating the hypothesis testing results in this report.

---

### 2.0 Data Pre-processing

#### 📌 2.1 Attribute Data Type Corrections, Year Data Extraction & Renaming of Attributes

Four attributes were corrected:

    1. "bathrooms" was initially categorized as a character data type and was converted to numeric. 
    
    2.  "bedrooms" was also incorrectly categorized as character and changed to numeric. 
    
    3.  "yr_renovated" was originally a numeric type but was transformed into a binary indicator (0 for no renovation, 1 for renovated) and renamed to "renovation" for better clarity.  
    
    4.  "date" was originally recorded in string format and was processed to extract the year, creating a new numeric attribute, "year_of_sale."

#### 📌 2.2 Inconsistency Resolution

    ▪️ "bedrooms" values of 0 were replaced with the mode (3 bedrooms).

#### 📌 2.3 Creation of the "house_updated" Dataset

    ▪️ A new dataset called "house_updated" was created by dropping the "id" attribute, as it lacked meaningful information. The "zipcode" attribute was also removed due to its limited explanatory and predictive power. Instead, the geographical attributes "lat" (latitude) and "long" (longitude) were retained, as they provided more useful information.

#### 📌 2.4 Handling Missing Values

Missing values were handled through imputation:

    ▪️ Categorical variables: "waterfront", "bedrooms", "bathrooms", "floors", "view", "grade", "condition", "yr_built", "renovation", and "year_of_sale".  
    
    ▪️ Continuous variables: "price", "sqft_living", "sqft_lot", "sqft_above", "sqft_basement", "lat", "long", "sqft_living15", and "sqft_lot15" were imputed using their median due to skewed distributions.

#### 📌 2.5 Outlier Treatment

    ▪️ The "sqft_lot" outlier was adjusted by calculating the average ratio between "sqft_lot" and "sqft_lot15" (the average lot size of the 15 nearest neighbors).  
    
    ▪️ This ratio was then used to impute a more reasonable value, reducing the outlier from 533,610 to approximately 254,996. However, this new value was still too large given the small interior living space of 800 sqft.  
    
    ▪️ As a result, the observation with the outlier was ultimately removed from the dataset to maintain data integrity.

---

### 3.0 Exploratory Data Analysis (EDA)

#### 📌 3.1 Metadata Inspection

![Screenshot](https://github.com/user-attachments/assets/d81ad2e1-4174-4173-92ff-7ebda1fedcea)  

    ▪️ Reviewed the dataset’s structure after data pre-processing, confirming that it now contains 19 variables and that all are assigned the correct data types.

#### 📌 3.2 Descriptive Statistics

![Screenshot](https://github.com/user-attachments/assets/94a1aa6f-533b-4593-8cc5-b0f0335c0a3c)  

    ▪️ Generated summary statistics for all 19 attributes.
    
    ▪️ The average house price was $543,406, with most homes having 3 bedrooms, 2 bathrooms, and a living area of 2,061 sqft.
    
    ▪️ Few homes had waterfront access or high view ratings. Most were in average condition (3.41) with above-average grade (7.65). 
    
    ▪️ The average above-ground area was 1,771 sqft, and basements averaged 288.9 sqft. Most homes were built around 1970, with minimal renovations.  
    
    ▪️ Properties were clustered near coordinates (47.56, -122.21), and neighboring homes averaged 1,983 sqft of living space with 12,465 sqft lots.

#### 📌 3.3 Missing Value Check

![Screenshot](https://github.com/user-attachments/assets/bdc3c534-ff45-4886-937b-26521815ea57)  

    ▪️ Ensured that no missing values remained in the dataset after imputation and cleaning.

#### 📌 3.4 Univariate Visualizations

    ▪️ Created individual bar plots for ordinal variables.
    
    ▪️ Findings: Most homes lacked waterfront views, had not been renovated, and received low view ratings. They were generally in moderate condition (rated 3) with a grade of 7. Most had 3 bedrooms and 2.5 bathrooms.

#### 📌 3.5 Continuous Variables Visualization

    ▪️ Plotted histograms for continuous variables.  
    
    ▪️ Findings: Most variables were positively skewed, indicating that high-value houses and large properties were relatively rare. Latitude and longitude revealed clustering of homes in specific geographical bands, possibly indicating denser neighborhoods.

#### 📌 3.6 Bivariate Relationships

    ▪️ Used scatterplots to examine potential relationships between key attributes.  
    
    ▪️ Findings: Larger homes, more bathrooms, and higher view ratings were positively correlated with higher prices. Newer homes and those in neighborhoods with larger living spaces also tended to have higher prices. Lot size showed a mild positive relationship, while bedroom count had a mixed impact on prices.

#### 📌 3.7 Correlation Analysis

    ▪️ Computed correlations for all continuous and ordinal variables.  
    
    ▪️ Findings: A strong positive correlation was observed between "price" and "sqft_living" (0.7 or higher). Moderate correlations were found between "price" and "view", "sqft_living15", "sqft_above", and "grade", suggesting these variables contribute to price but not as strongly as living space.

---

### 4.0 Feature Engineering

#### 📌 4.1 Date Extraction

    ▪️ APreviosuly discussed in earlier sections.

#### 📌 4.2 Feature Creation

    ▪️ "house_age": Created by subtracting "yr_built" from "year_of_sale" to determine house age.  
    
    ▪️ "bed_bath_ratio": Ratio of bedrooms to bathrooms to assess layout convenience.  
    
    ▪️ "lot_utilization": Ratio of "sqft_living" to "sqft_lot" to assess land use.

#### 📌 4.3 Outlier Detection for Created Features

    ▪️ Detected outliers for "house_age", "bed_bath_ratio", and "lot_utilization".  
    
    ▪️ No outliers were found for "house_age". Outliers in "bed_bath_ratio" were genuine after validation.  
    
    ▪️ Some "lot_utilization" values exceeded 1 (impractical), indicating the need for filtering.  
    
    ▪️ Created a new column "living_lot_diff" to track differences and removed invalid records.

#### 📌 4.4 Transformation

    ▪️ Applied logarithmic transformations on skewed variables: "sqft_lot", "sqft_lot15", "price", "sqft_basement", "bed_bath_ratio", "sqft_above", "sqft_living", "lot_utilization", and "sqft_living15". 
    
    ▪️ This normalized the distributions for predictive modeling.

#### 📌 4.5 Scaling

    ▪️ Standardized continuous variables: "lat", "long", "log_bed_bath_ratio", "house_age", "log_sqft_lot", "log_sqft_lot15", "log_sqft_above", "log_sqft_basement", "log_sqft_living", "log_sqft_living15", and "log_lot_utilization" to a mean of 0 and SD of 1.

#### 📌 4.6 One-Hot Encoding

    ▪️ Applied one-hot encoding to "view" and stored the output in "house_transformed_with_dummies".  
    
    ▪️ This step enabled use of categorical variables in regression models.

---

### 5.0 Hypothesis Testing

    All hypothesis tests were conducted at a significance level (α) of 0.05.

#### 📌 5.1 House Age and Price

    ▪️ Hypothesis: Older houses tend to have lower prices.  
    
    ▪️ Method: Linear regression  
    
    ▪️ Result: Significant negative relationship observed.

#### 📌 5.2 Neighborhood Living Space and Price

    ▪️ Hypothesis: Houses in neighborhoods with larger living spaces tend to have higher prices.  
    
    ▪️ Method: Linear regression using "sqft_living15"  
    
    ▪️ Result: Positive correlation confirmed.

#### 📌 5.3 Bedroom-to-Bathroom Ratio and Price

    ▪️ Hypothesis: Higher bedroom-to-bathroom ratios result in lower prices.  
    
    ▪️ Method: Linear regression using "bed_bath_ratio"  
    
    ▪️ Result: Negative relationship observed.

#### 📌 5.4 Lot Utilization and Price

    ▪️ Hypothesis: Higher lot utilization leads to higher prices.  
    
    ▪️ Method: Regression between "lot_utilization" and "price"  
    
    ▪️ Result: Positive correlation observed.

#### 📌 5.5 View Rating and Price

    ▪️ Hypothesis: Higher view ratings are associated with higher prices.  
    
    ▪️ Method: ANOVA with dummy variables for view ratings 
    
    ▪️ Result: Homes with higher view ratings had significantly higher prices.

---

### 6.0 Conclusion

#### 📌 6.1 Key Findings

    ▪️ House age negatively affects price, while neighborhood living space has a positive impact.  
    
    ▪️ Higher bed_bath_ratios reduce prices, whereas better lot utilization and higher view ratings increase property values.
