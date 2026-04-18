### B1(a) Problem Formulation

The objective is to predict the number of items sold for a given store under a specific promotion.

- **Target Variable**:  
  `items_sold` (number of products sold)

- **Input Features (Candidate Predictors)**:  
  - Store characteristics: store_size, location_type  
  - Promotion details: promotion_type  
  - Temporal features: month, day_of_week, is_weekend, is_festival  
  - Market conditions: competition_density  
  - Store identifier: store_id  

- **Type of Machine Learning Problem**:  
  This is a **supervised learning regression problem**, as the target variable (`items_sold`) is continuous.

- **Justification**:  
  The goal is to estimate a numerical outcome (sales volume) based on historical data. Regression models are appropriate for predicting continuous values and can capture relationships between promotional strategies and sales performance.

  ### B1(b) Why Items Sold is a Better Target than Revenue

Using **items sold (sales volume)** as the target variable is more reliable than total revenue because revenue can be heavily influenced by pricing strategies, discounts, and product mix.

For example:
- A high discount promotion may increase items sold but reduce revenue per item.
- Premium product sales may increase revenue without increasing volume.

By focusing on **items sold**, the company can directly measure the effectiveness of promotions in driving customer demand and store traffic.

- **Broader Principle**:  
  This illustrates the importance of selecting a target variable that aligns closely with the business objective.  
  In real-world machine learning projects, the target variable should:
  - Reflect the actual goal of the business  
  - Be stable and less affected by external noise  
  - Enable meaningful and actionable insights  

In this case, maximizing items sold helps optimize promotion effectiveness more accurately than revenue.   

### B1(c) Alternative Modelling Strategy

Instead of using a single global model across all stores, a better approach is to use a **segmented or hierarchical modelling strategy**.

One effective approach is:
- **Cluster stores based on characteristics** such as location type, store size, and competition density
- Build **separate models for each cluster**

Alternatively:
- Use models that include **store-specific effects** (e.g., adding store_id as a feature or using advanced techniques like mixed models)

- **Justification**:  
  Stores in urban, semi-urban, and rural areas behave differently due to variations in customer demographics, competition, and footfall.

A single global model may fail to capture these differences, leading to poor predictions.

Segmented modelling allows:
- More accurate predictions  
- Better understanding of local behavior  
- More targeted and effective promotion strategies  

This approach aligns the model more closely with real-world business variability.    



### B2(a) Data Joining and Dataset Design

To build the modelling dataset, the four tables (transactions, store attributes, promotion details, and calendar) must be joined using appropriate keys.

- **Joining Strategy**:
  - Join **transactions** with **store attributes** using `store_id`
  - Join **transactions** with **promotion details** using `promotion_id` (or equivalent)
  - Join **transactions** with **calendar** using `transaction_date`

- **Grain of the Final Dataset**:
  The final dataset should be at a **store–date–promotion level**, meaning:
  
   **One row = one store on one date under a specific promotion**

- **Aggregations Before Modelling**:
  Since raw transactions may be at item-level or transaction-level, aggregation is required:
  - Total `items_sold` per store per day (target variable)
  - Total revenue (optional feature)
  - Average basket size
  - Number of transactions (footfall proxy)

### B2(b) Exploratory Data Analysis (EDA)

Before building the model, the following analyses would be performed:

1. **Promotion Type vs Items Sold (Bar Plot / Boxplot)**  
   - Purpose: Compare effectiveness of different promotions  
   - Insight: Identify which promotions drive higher sales  
   - Impact: Helps in feature importance and possible interaction features  

2. **Time Series Analysis (Line Plot of Sales over Time)**  
   - Purpose: Identify trends, seasonality, and spikes (festivals, weekends)  
   - Insight: Detect seasonal patterns or demand surges  
   - Impact: Justifies creating time-based features (month, weekday, festival flags)  

3. **Sales Distribution (Histogram / Boxplot)**  
   - Purpose: Understand distribution of `items_sold`  
   - Insight: Detect skewness, outliers  
   - Impact: May require transformation (e.g., log transformation) or robust models  

4. **Correlation Heatmap (Numerical Features)**  
   - Purpose: Identify relationships between features  
   - Insight: Detect multicollinearity or strong predictors  
   - Impact: Helps in feature selection and avoiding redundant variables  

5. **Location Type vs Sales (Boxplot)**  
   - Purpose: Compare performance across urban, semi-urban, rural stores  
   - Insight: Understand regional differences  
   - Impact: Supports segmentation or interaction features  

### B2(c) Handling Promotion Imbalance

If 80% of transactions occur without any promotion, the dataset is highly imbalanced.

- **Impact on Model**:
  - The model may become biased toward predicting non-promotion scenarios
  - It may underestimate the true effect of promotions
  - Poor learning of promotion-related patterns

- **Steps to Address the Issue**:
  - **Resampling techniques**: Oversample promotion data or undersample non-promotion data
  - **Feature engineering**: Create a binary feature indicating promotion vs no promotion
  - **Stratified analysis**: Evaluate model performance separately for promotion and non-promotion cases
  - **Use appropriate metrics**: Focus on segment-wise performance rather than overall metrics

This ensures the model properly learns the impact of promotions despite the imbalance.  


### B3(a) Train-Test Split and Evaluation Metrics

Since the data is time-ordered (monthly store-level data over three years), the train-test split should be **temporal (chronological)** rather than random.

- **Train-Test Strategy**:
  - Use the first ~80% of the timeline (earlier months) as the training set
  - Use the most recent ~20% (latest months) as the test set
  - Optionally, use rolling/forward validation for robustness
 **Evaluation Metrics**:
  1. **RMSE (Root Mean Squared Error)**  
     - Measures average prediction error with higher penalty for large errors  
     - Useful for identifying large mistakes in sales prediction  

  2. **MAE (Mean Absolute Error)**  
     - Measures average absolute difference between predicted and actual values  
     - Easier to interpret in business terms (e.g., “on average, predictions are off by X items”)  

- **Interpretation in Business Context**:
  - Lower RMSE and MAE indicate better prediction accuracy  
  - MAE helps understand typical error in sales forecasting  
  - RMSE highlights whether the model is making large costly mistakes during peak periods (e.g., festivals)
 
  - ### B3(b) Explaining Model Recommendations Using Feature Importance

The model recommends different promotions for the same store (Store 12) in December and March because the **input features differ across months**, leading to different predictions.

To investigate this:

- **Step 1: Analyze Feature Importance**
  Identify which features (e.g., month, festival flag, weekend patterns, promotion type) have the highest influence on predictions.

- **Step 2: Compare Input Features for Both Months**
  - December may include:
    - Festival season (higher demand)
    - Increased customer footfall
  - March may represent:
    - Regular demand patterns
    - No major festivals

- **Step 3: Interpretation**
  - In December, the model may favor **Loyalty Points Bonus** to maximize repeat purchases during high-demand periods.
  - In March, the model may favor **Flat Discount** to stimulate demand during relatively normal periods.

- **Communication to Marketing Team**:
  Explain that the model adapts recommendations based on **seasonality, customer behavior, and contextual factors**, rather than applying a one-size-fits-all strategy.

This demonstrates that the model is capturing real-world patterns and optimizing promotions dynamically.  

### B3(c) Model Deployment and Monitoring

To deploy the model for monthly recommendations, the following process can be used:

- **1. Model Saving**:
  - Save the trained pipeline (including preprocessing and model) using tools like `joblib` or `pickle`

- **2. Data Preparation (Monthly)**:
  - Collect new data for each store (store attributes, calendar info, promotion options)
  - Apply the same feature engineering steps (date features, encoding, scaling)
  - Ensure consistency with training data schema

- **3. Prediction Process**:
  - Load the saved model
  - Input new monthly data
  - Generate predicted `items_sold` for each promotion option
  - Select the promotion with the highest predicted sales for each store

- **4. Monitoring and Maintenance**:
  - Track model performance over time using metrics like RMSE and MAE
  - Monitor for **data drift** (changes in input distributions) and **concept drift** (change in relationships)
  - Compare predicted vs actual sales regularly

- **5. Retraining Strategy**:
  - Retrain the model periodically (e.g., every quarter) or when performance degrades significantly
  - Incorporate new data to keep the model updated with recent trends

