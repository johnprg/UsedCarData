# What drives the price of a car?

![](images/kurt.jpeg)

### Background
The client is a used car dealership, with an interest in better understanding what is important to a buyer, in order to better inform pricing and promotion.
The Jupyter notebook used for this analysis can be found at https://github.com/johnprg/UsedCarData/UsedCarData.ipynb .

### The Dataset
The original dataset from Kaggle contained information on 3 million used cars. The provided dataset contains information on 426K cars to ensure speed of processing.  We seek to understand what factors make a car more or less expensive.
### Business Objectives
Since we can't actually interview the client, here are some of the possible business objectives which this effort might achieve.
1. Sell more cars
2. Sell cars more quickly to reduce lot inventory costs and depreciation
3. Increase profit margin on sale
4. Decrease cost of purchase
5. Improve promotion effectiveness by advertising what buyers are looking for.
6. Improve customer satisfaction
7. Improve retention / repeat buyers
8. Increase sales agent productivity / retention

### Business success criteria
Possible metrics and goals associated with each business objective
1. Increase total #cars sold
2. Decrease average time on lot
3. Increase average profit per sale
4. Decrease average purchase price on intake
5. Improve promotional effectiveness (channel dependent metric)
6. Increase post sale customer satisfaction at +1 week, +1 month, +1 year
7. Increase percentage of buyers who repeat
8. Increase average revenue / profit / commission per agent.  Increase average agent tenure

### Data Objectives derived from Business Understanding

1. Determine the relative importance of factors (columns) affecting the prices of used cars in the USA.
2. Examine regional variation.  Develop hypotheses about any subsetting necessary to restrict the training set to increase relevance for this client.
2. Build and test models which can be used to predict the price of additional cars given the named factors.
3. Use the best predictive models to build hypotheses about consumer prefences.

## Summary of Findings
We found strong correlation between several of the columns in the dataset and sales price.
![price_vs_year](https://github.com/user-attachments/assets/cf33507e-e346-442b-9fa1-594e26505116)
![price_vs_odometer](https://github.com/user-attachments/assets/37677f48-673e-4ad5-9641-8ea4c2f0ab40)

Indeed, many of the features seemed to correlate with price.  Below is a heatmap showing the extent of correlation between different features.
![heatmap](https://github.com/user-attachments/assets/a38d5bd7-1221-47c2-bdbe-02b31e0095c0)

### Market Segments

![price_by_manufacturer_lineAll Years](https://github.com/user-attachments/assets/6539184b-a643-49be-ac8b-29546222d6c1)

Looking at the price by manufacturer and model year, we noticed that the data seemed to fall into three distinct markets:

1. **Vintage before around 1980**:  These are specific high priced vintage models which actually appreciate over time.  A high premium is put on condition and low odometer readings.
   
![price_by_manufacturer_scatterBefore 1980](https://github.com/user-attachments/assets/b935e42f-d09b-45c4-aa2d-dbbe13767f94)

2. **Luxury / Exotic outliers** : These are high end, high price models from exotic brands like Aston Martin, Ferrari, and Porsche.  These represent outliers in the recent year dataset, as they fall well outside of the average price model.  For this reason, we treat them as a separate market segment.

![price_by_manufacturer_scatterOutliers after 1980](https://github.com/user-attachments/assets/6ee73395-556c-4b2f-a9d4-bf8c33c341d2)

3. **Mainstream market**:  Non-exotic cars from 1980 to recent model years.  These exhibit a characteristic curve of increasing value with more recent model years, and were therefore chosen as the basis for creating our pricing models.

![price_by_manufacturer_lineCore after 1980](https://github.com/user-attachments/assets/0a6a71ac-7e5f-494c-9681-bf275c8c52da)

### Pricing Models

Several models were evaluated, and compared using mean absolute error as the primary criterion.  We chose this because it really focuses on pricing accuracy and is easy to understand.  The results for the models are shown below (you do not need to understand the model names, just focus on the resulting prediction accuracy).

<pre>
Average Car Price: $ 16480.33
                                       mean average                mean average
                                       error ($)                   error (%)
sequential(degree=2, n_features=6)     $4594.66                    27.88%
linear_scaled                          $4884.45                    29.64%
ridge_scaled (alpha=1.0)               $4884.45                    29.64%
ridge_scaled_best (alpha=100)          $4884.78                    29.64%
lasso_scaled (alpha=10)                $4884.85                    29.64%
lasso_scaled_best (alpha=100)          $4893.79                    29.69%
ridge_grid (alpha=1151.3953993264481)  $4892.35                    29.69%
</pre>

The winning model (Sequential Feature Selection with Polynomial Degree 2 and 6 chosen features) showed the following predictive performance for the test data which was not part of the training set.
![Sequential Feature Selector: Degree 2, 6 Features_predictions](https://github.com/user-attachments/assets/12da90b0-cee7-434a-9c0b-1b6539d7b120)

However, this model seems to overweight the year parameter, including it as part of six selected features.  For that reason, we selected the Lasso model with Alpha value 10, which weights the parameters and ends up including 10 parameters in the resulting model according to the coefficients below.

![Lasso Regression with Standard Scaler (alpha=10)_coefs](https://github.com/user-attachments/assets/dfb12e0e-53d2-43e3-a560-3977a8e6b953)

This means that the pricing model takes into account the factors that we know consumers consider, with strong correlation on year, type of car, odometer reading, and location of sale.  In spite of the slightly higher error on the test data, we suspect that this model is a better predictor of consumer preferences.

### Recommendations

1. The resulting model can be used to price newly acquired cars for sale.
2. Further work could be done to develop separate pricing models for each of the Vintage and Outliers markets.
3. Further analysis could be done on a specific market location to refine the pricing specifically for that location, though this is a factor in the chosen model.





