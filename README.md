# Random Forest Regressor is Best Fit For Product Sales

Author: David Burns <br>
Date: 10/19/2022

## Business Problem
The goal of this project is to understand the data as it relates to the products and outlets that play crucial roles in increasing sales.  

### Data
The initial data contained 8,523 rows of data and 11 columns of variables that could impact sales and 1 column of target sales value data.   You can download the data using this [link](https://drive.google.com/file/d/1syH81TVrbBsdymLT_jl2JIf6IjPXtSQw/view).  The original source of the [data.](https://datahack.analyticsvidhya.com/contest/practice-problem-big-mart-sales-iii/)

## Methods
First, we made a copy of the original data.  We dropped duplicated rows.  We then visually inspected the data and discovered missing data points in product weight and outlet size.  We imputed the data for the missing rows "Item Weight" by taking the average of the weights of same product types.  We also imputed the values for outlet size as "Unknown" so as not to dirty the actually complete data points.  We looked at correlations between the various factors and the "Item_Outlet_Sales."  We then created univariate and multivariate visuals to understand the data. 

We did exploratory data visualization analysis of: 

1)  Correlation heat map between the numeric data points which yielded a high correlation between MRP and our target "item outlet sales"
2)  Box plot of MRP and product type 
3)  Bar chart of outlet type versus item outlet sales
4)  Scatter chart of MRP and item outlet sales with outlet type (and also outlet size) in color
5)  Many other visualizations with varying degree of usefullness

### Examples of the visualizations

**Box plot of MRP and product type**
<br><br>
![Screen Shot 2022-10-20 at 8 53 53 PM](https://user-images.githubusercontent.com/113855848/197107939-4158abae-4f8b-4fb7-8026-3fc061cd7f38.png)

Due to the clear importance of MRP and item outlet sales, we explored a box plot of MRP by product type
<br><br>
**Scatter chart of MRP and item outlet sales with outlet type in color**
<br><br>
![MRP_Sales (1)](https://user-images.githubusercontent.com/113855848/197106343-64c64a42-8714-42b4-bd96-45de3df2c74d.png)

This scatter plot shows MRP and item outset sales on the x and y axes, respectively, and outlet type in color

Based on our analysis, we determined that MRP and outlet size were the most important values.  However, we ran a regression with all the data and one hot encoded ("OHE") the object columns and scaled the numeric columns.  The results were a very poor fit with an astronomical error value largely because of the numerous columns generated by the OHE data.  Upon further analysis, we determined that the object columns 'Item_Identifier', 'Outlet_Identifier', 'Item_Weight' , 'Outlet_Establishment_Year' we not correlated with our target of item outlet sales and dropped them from the analysis.

We went back to the original data, dropped the uncorrelated columns, and then split the original data into a train data set and test data set.  Since we had dropped the item weight column we didn't have to impute the missing weights.  However, we did still fill in the missing outlet size data with "Unknown."  Since we had dropped the columns with many object fields, we one hot encoded the object data and scaled the numeric data for standardization.

 
## Model

We ran three regressions - a linear regression, simple regression tree, and random forest regression.  The random forest regression was the best fit when tuned with max depth of 5 and n-estimators of 270.  

The regression yielded an R2 of .603.  It did not overfit the data as measured by the difference in R2 between training and testing. 

The average target value of item outlet sales was ₹2,181 (values in rupees).  The mean absolute error ("MAE") for the test data set was ₹729 which represents 33.4%.    

The root mean squared error ("RMSE") was ₹1,047 which while a bit higher than the MAE indicated that there are not a lot of large outliers which would drive the RMSE higher. 

All in, the Random Forest Regressor was the best fit of the data compared to the simple linear regression and the simple regression tree.

## Limitation, Recommendations, and Next Steps

The Random Forest Regressor yielded good not great results.  As measured by R2, the model explains about 60% of the variation in the data.  With greater data preprocessing and more sophisticated data tools we can likely do better.  We should continue to look at other options and data processing for improvement.  We need to rub some funk on it! 

## Additional Information Concerning the Regression

We ran two additional regressions of the sales data. The first was a linear regression which yielded several top important features with the following coefficients:

#### Linear Regression

![Linreg](https://user-images.githubusercontent.com/113855848/215377220-f003e105-d349-4e06-8712-05456571a25c.png)

The most impactful features are the outlet type - Type 1, 2, or 3 and the sale price of the object. When predicting the sale of an item, the size and by implication the volume of products sold, have a large impact on the total sales of the product.

The coefficients for each item:

Outlet_Type_Supermarket Type3 3,365.88
Outlet_Type_Supermarket Type1 1,940.48
Outlet_Type_Supermarket Type2 1,657.92
Item_MRP 984.31

means that depending on the store where the product is sold, these are the baseline sales figures for the product.

The Item_MRP coefficient means that for every increase in the unit price of the item, the total sales volume increases by 984.

#### Random Forest Regressor

![forestreg](https://user-images.githubusercontent.com/113855848/215377549-fce3a2a0-5940-4c94-8fe4-4c205b9ce979.png)

Based on both 'importance' and 'permutation importance' the top five features per the Random Forest Regressor are item_MRP, outlet size (1, 2, and 3), and Item_Visibility.  

### For Further Information

For any additional questions, please contact me at dmburns@gmail.com


