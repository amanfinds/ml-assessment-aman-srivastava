# Machine Learning


##  Repository Structure

```
ml-assessment-<your-name>/
│
├── part_a/
│   ├── q1_supervised.ipynb
│   ├── q2_unsupervised.ipynb
│   └── q3_feature_engineering.ipynb
│
├── part_b/
│   └── business_analysis.md
│
└── data/
    ├── q1_heart_disease.csv
    ├── q2_customers.csv
    └── q3_retail_promotions.csv
```


##  Part A: Python Coding

###  Q1: Supervised Learning

* Built classification models to predict heart disease
* Models used:

  * Decision Tree
  * Random Forest
  * Gradient Boosting
* Evaluated using:

  * Confusion Matrix
  * Precision, Recall, F1-score
* Performed hyperparameter tuning using GridSearchCV

---

###  Q2: Unsupervised Learning

* Performed customer segmentation using K-Means clustering
* Used Elbow Method to determine optimal number of clusters
* Applied PCA for dimensionality reduction
* Visualized clusters in 2D space

---

###  Q3: Feature Engineering & Regression Pipeline

* Engineered time-based features from transaction data
* Implemented temporal train-test split
* Built pipelines using:

  * ColumnTransformer
  * StandardScaler
  * OneHotEncoder
* Trained models:

  * Linear Regression
  * Random Forest Regressor
* Evaluated using:

  * RMSE
  * MAE
* Analyzed feature importance

---

##  Part B: Business Case Analysis

* Formulated the promotion effectiveness problem as a regression task
* Designed data joining and EDA strategy
* Proposed modeling improvements for store-level differences
* Explained model evaluation, deployment, and monitoring strategy

---

##  Technologies Used

* Python
* Pandas, NumPy
* RandomForest
* Matplotlib, Seaborn
* Google Colab

---

##  Key Learnings

* Importance of proper data preprocessing and feature engineering
* Model selection and evaluation using appropriate metrics
* Handling real-world challenges like data imbalance and temporal splits
* Translating machine learning results into business insights

---



## Author

**Aman Srivastava**


