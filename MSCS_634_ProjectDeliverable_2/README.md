MSCS 634 – Project Deliverable 2
Regression Modeling and Performance Evaluation
Course: MSCS 634 – Advanced Big Data and Data Mining
Project: Online Retail Customer Spending Prediction
Student: Rajasekhar Reddy Kayyam
Project Overview
This project extends Deliverable 1's Online Retail data cleaning and exploratory analysis by building regression models to predict future customer spending from earlier purchase behavior. It compares ordinary linear regression with Ridge and Lasso regularization and evaluates generalization using a held-out test set and five-fold cross-validation.
Dataset
The project uses the Online Retail transaction dataset and the cleaned file online_retail_cleaned.csv produced in Deliverable 1. Relevant columns include CustomerID, InvoiceNo, StockCode, InvoiceDate, and LineRevenue. Deliverable 1 removed duplicate and canceled transactions and excluded records without usable customer identifiers for customer-level analysis. See Deliverable 1 for full preprocessing details.
Prediction Target and Feature Engineering
For each customer with purchase history before September 1, 2011, the target is the sum of transaction LineRevenue from September 1 through November 30, 2011. Customers with no purchases in that future window receive a target of zero.

Features are calculated only from transactions before September 1, 2011:
- PastSpend: historical revenue sum
- PurchaseLines: historical transaction-line count
- InvoiceCount: number of distinct historical invoices
- UniqueProducts: number of distinct historical products
- AvgLineRevenue: mean historical revenue per transaction line
This chronological separation is important: calculating features from the prediction period would leak information about the target. The notebook uses log1p on future spending for training, with predictions converted back to GBP using expm1 for test metrics. The log transformation requires nonnegative target values; the notebook should verify this before fitting.
Models
1. Linear Regression — unregularized baseline.
2. Ridge Regression — L2 regularization to reduce sensitivity to correlated predictors.
3. Lasso Regression — L1 regularization that can shrink some feature coefficients to zero.
All models use standardized predictors via scikit-learn pipelines, so scaling is fitted only on training data. The example notebook uses Ridge alpha=10 and Lasso alpha=0.1; these are starting settings, not necessarily optimal values. Prefer selecting alpha with training-only cross-validation.
Model Evaluation
The data is divided into an 80% training set and a 20% held-out test set (random_state=42). Models are compared with:
- MAE: average absolute prediction error
- MSE: average squared prediction error
- RMSE: square root of MSE, expressed in GBP
- R²: fraction of variation explained relative to a constant-mean baseline
- Five-fold cross-validation: stability across training folds
The initial notebook example calculates test metrics on the original GBP scale and cross-validation R² on the log-transformed target scale. Those R² values are not directly comparable; identify the scale when reporting them. For the final report, ideally calculate cross-validation metrics on the GBP scale as well, using a suitable target-transform wrapper.
Results
To complete after executing the notebook: Replace the placeholders below with actual output. Do not report assumed or Lab 4 Diabetes-dataset results for this retail project.

Model	Test MAE (£)	Test MSE (£²)	Test RMSE (£)	Test R²	Mean CV R² (log target)
Linear Regression	TBD	TBD	TBD	TBD	TBD
Ridge Regression	TBD	TBD	TBD	TBD	TBD
Lasso Regression	TBD	TBD	TBD	TBD	TBD


Best model: TBD — select based primarily on held-out RMSE, then discuss cross-validation consistency and R².
Visualizations
Include the following in the executed notebook and optionally save them under screenshots/:
- RMSE comparison across models
- Actual versus predicted future customer spending
- Cross-validation score comparison
- Optional Ridge/Lasso coefficient comparison
Key Insights and Discussion
The project tests whether historical purchase frequency, total spend, product diversity, and typical line revenue help predict later customer spend. Ridge and Lasso may improve stability when features are correlated, but regularization is not guaranteed to outperform ordinary least squares. Interpret the observed results only after running the notebook.

Future spending is often skewed and may include many zero-spend customers. The log target transformation helps reduce the influence of very large spenders. This is a regression exercise, not a causal analysis, and performance depends on the chosen historical and prediction windows.
Challenges and Decisions
- Target leakage: prevented by separating historical features from future transactions.
- Correlated features: compared Ridge and Lasso with ordinary linear regression.
- Uneven spending: used a log transformation for the training target.
- Generalization: used a held-out test set and five-fold cross-validation.
- Reproducibility: used fixed random seeds and model pipelines.
How to Run
1. Place MSCS_634_ProjectDeliverable_2.ipynb and online_retail_cleaned.csv in the same directory.
2. Install requirements:
   python -m pip install notebook pandas numpy matplotlib scikit-learn
3. Start Jupyter:
   jupyter notebook MSCS_634_ProjectDeliverable_2.ipynb
4. Run all cells, inspect metrics and charts, and update the Results table above.