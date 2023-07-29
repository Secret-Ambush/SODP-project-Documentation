# Regression Models and Visualization

This Markdown documentation explains the purpose and functionality of the provided Python code. The code is designed to build 341 regression models for a sample flight (flight number '0772') by training XGBoost regressors for each day prior to departure. After training the models, the script visualizes the predictions and actual 'SODP' on a calendar heatmap for a particular day prior model.

## Section 1: Setup and Initialization

This section includes the necessary imports and configurations to set up the environment and establish connections to external services and data sources.

### Importing Libraries

The code imports required Python modules, as shown below

`pandas
os
sqlalchemy
snowflake
pickle
matplotlib
numpy
tqdm
xgboost
concurrent.futures
plotly.graph_objects`



#### Connecting to Snowflake

The script uses SQLAlchemy and Snowflake libraries to connect to a Snowflake database. The connection details are fetched from environment variables ('snowflake_conn').

#### Removing Proxy Variables

Two proxy variables ('HTTP_PROXY' and 'HTTPS_PROXY') are removed from the environment variables to ensure that local proxy settings are not used.

#### Creating SQLAlchemy Engine

An instance of sqlalchemy.engine is created using the Snowflake connection details.

### Section 2: Data Preprocessing and Feature Engineering

This section consists of data preprocessing and feature engineering steps. The primary DataFrame 'flight' likely contains flight-related data with various columns like 'fltdep' (Flight Departure Date), 'days_prior' (Days Prior to the Flight), 'sodp' (Snap-On Date Prior), 'bkd' (Bookings), 'cap' (Capacity), 'rev' (Revenue), and 'frc_unc' (Uncertain Forecast).

### Preparing DataFrame and Feature Engineering

The variable `distinct_flights` is assigned the same DataFrame 'flight', which will be used to process the data for model training and testing.

### Finding Maximum 'sodp' and Updating 'sodp'

The code groups the data by 'fltdep' and finds the maximum 'sodp' value for each group. The 'sodp' column is then updated with the maximum 'sodp' value for each respective 'fltdep' group. The 'max_sodp' column is dropped if not needed.

### Filtering and Sorting Data for Model Training

The code filters the data to include rows where 'days_prior' is greater than or equal to 100 and 'fltdep' is less than '2019-01-01' (target_date). The resulting DataFrame 'df' is sorted based on 'fltdep' in ascending order.

## Section 3: Machine Learning Model Training and Evaluation

This section focuses on training 341 XGBoost regression models, each corresponding to a specific number of days prior to the flight (count).

### Training and Saving Models

The code initiates a loop to create 341 models, each using a different subset of the data based on the 'count' value in the loop. For each iteration, a subset of data is prepared, and PCA (Principal Component Analysis) is applied for dimensionality reduction.

The function then performs Principal Component Analysis (PCA) on the training features. The data is standardized using `StandardScaler`, and PCA is applied to reduce the dimensionality to 3 principal components:

```python
scaler = StandardScaler()
scaled_train_features = scaler.fit_transform(train_features)

model = PCA(n_components=3).fit(scaled_train_features)
X_pc = model.transform(scaled_train_features)
```

The data is split into training and testing sets, and an XGBoost regressor is trained with hyperparameters. The Mean Absolute Error (MAE) is calculated for each model based on the test data. The trained models are saved to separate pickle files for each flight and 'count' combination.

## Section 4: Multi-threading for Model Training

The model training process is multi-threaded using `concurrent.futures.ThreadPoolExecutor`. It allows the function `train_model` to be executed concurrently for different `counts`, making the training process faster.

## Section 5: Visualization of Predicted 'SODP' on Calendar Heatmap

This section focuses on visualizing the predicted 'SODP' for the flight with flight number '0501' using a calendar heatmap with Plotly.

#### Calplot of Predicted 'SODP'

The code generates a calendar heatmap (calplot) of the predicted 'SODP' values for the flight '0501' in the year 2019. The color represents the predicted 'SODP' values.

#### Calplot of Actual 'SODP'

Similarly, a calendar heatmap is generated to visualize the actual 'SODP' values for the flight '0501' in the year 2019.

#### Calplot of Delta 'SODP'

Another calendar heatmap is generated to visualize the difference between the actual and predicted 'SODP' values for the flight '0501' in the year 2019.## Summary

The provided Python script builds 341 regression models for a sample flight with flight number '0772'. It trains XGBoost regressors for each day prior to departure and visualizes the predictions using calendar heatmaps. The script serves as a data analysis and predictive modeling tool for flight data, helping to forecast 'SODP' values and evaluate model performance for different days prior to flight departure.
