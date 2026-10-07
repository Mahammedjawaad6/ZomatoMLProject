ZOMATO RESTAURANT EDA AND RATING PREDICTION
===========================================

PROJECT OVERVIEW
----------------
This project explores a Zomato restaurant dataset, prepares a cleaned copy for analysis, examines restaurant and service characteristics, and builds a regression model to estimate ratings for restaurants that do not yet have a rating.

PROJECT FILES
-------------
- Zomato_Restaurant_EDA.ipynb: Main Jupyter notebook containing data preparation, exploratory analysis, visualizations, and rating prediction.
- Zomato Restaurant Dataset.csv: Input dataset used by the notebook.
- Zomato Restaurant Dataset Cleaned.csv: Cleaned dataset exported by the notebook.
- zomato_unrated_rating_predictions.csv: Predicted ratings for records whose aggregate rating is zero or missing, when predictions have been generated.

DATASET
-------
The input file contains restaurant records and fields such as restaurant ID and name, country code, city and address, coordinates, cuisines, average cost for two, currency, booking and delivery options, price range, aggregate rating, rating text and color, and votes. The notebook expects the input CSV to be in the same directory as the notebook.

NOTEBOOK WORKFLOW
-----------------
1. Import the analysis libraries and load the source CSV. The original data is retained in df_raw, while df is used for cleaning and analysis.
2. Clean and validate the data:
   - Trim text fields and make undecodable characters visible.
   - Normalize Yes/No service fields.
   - Convert numeric fields to numeric types, turning malformed values into missing values.
   - Remove exact duplicate rows and repeated restaurant IDs.
   - Replace missing cuisine labels with "Unknown".
   - Mark invalid ratings, price ranges, coordinates, costs, and vote counts as missing. Unusual but plausible costs are retained.
   - Export the cleaned data as a CSV file.
3. Explore data quality and restaurant patterns, including missing values, cities and countries, ratings and votes, costs by currency, price ranges, cuisine popularity, booking and delivery services, numeric relationships, and restaurant coordinates.
4. Train and evaluate a rating regression model. Restaurants with positive aggregate ratings are used for training and a holdout evaluation. Zero or missing ratings are treated as unrated.
5. Score unrated records and save their predicted ratings to a CSV file when any such records are present.
6. Print a final summary of the cleaned data, rating statistics, leading city and cuisine, model comparison when available, and output file paths.

RATING MODEL
------------
The notebook uses a scikit-learn Pipeline with a ColumnTransformer and RandomForestRegressor. Available inputs can include country code, city, cuisines, average cost for two, currency, table booking, online delivery, current delivery status, price range, longitude, and latitude.

Numeric inputs are imputed with their median. Categorical inputs are imputed with their most frequent value and one-hot encoded; unknown categories are supported. The model is evaluated on a 20% holdout split using mean absolute error (MAE), root mean squared error (RMSE), and R-squared (R²). A training-mean baseline is shown for comparison.

Rating text, rating color, votes, and restaurant ID are excluded from model features to reduce target leakage and avoid using identifiers. Cost is modeled together with currency because the dataset contains multiple currencies.

REQUIREMENTS
------------
Run the notebook in a Python environment with:
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn
- Jupyter Notebook or a compatible notebook environment

RUNNING THE PROJECT
-------------------
1. Keep the notebook and input CSV together in the same working directory.
2. Open Zomato_Restaurant_EDA.ipynb in Jupyter or a compatible IDE.
3. Run the notebook cells from top to bottom so imports, cleaning, analysis, and model evaluation are available to later cells.
4. The final summary cell can also load the cleaned CSV when run in a fresh kernel. Model comparison results require running the model cells first.

INTERPRETATION NOTES
--------------------
- A zero rating is treated as an unrated record by this project; it is not interpreted as a genuine zero-star review.
- Costs from different currencies should not be compared as if they share one scale. The notebook groups cost analysis by currency and includes currency in the prediction features.
- Predicted ratings are estimates from the available dataset and model. They are not actual user ratings or guarantees of future restaurant performance.
- Geographic and city-level patterns reflect the records in this dataset and may not represent current restaurant coverage.
