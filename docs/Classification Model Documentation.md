# Classification Model

## Section 1: Handling Data Set – Preparing and Preprocessing

In this project, I had the opportunity to work with an extensive dataset comprising an impressive 500 million rows, which far surpassed any dataset I had handled before. To provide a perspective, the most recognizable Titanic dataset consists of approximately 1000 records, emphasizing the scale of the data I managed.

For training the predictive models, I utilized historical flight data spanning the years 2017-2018, and to evaluate their performance, I employed the data from the year 2019. This division into training and testing datasets allowed me to ensure the robustness and accuracy of the models in real-world scenarios.

Handling such a vast amount of data was an exciting challenge, and it provided valuable insights into the complexities and nuances of the airline industry's demand patterns. As an intern, I was given an 'x-small' workspace on the cloud data platform 'Snowflake', my line manager tried to up my permissions, but to no avail.

I attempted to migrate the entire dataset to PostgreSQL, leaving my laptop overnight at the office for the task, but 500 million rows proved to be too much for PostgreSQL to handle.

Eventually, I resorted to using Snowflake programmatically, extracting data at the flight level. Even then, it was a large amount of data, so I had to perform manipulations on the dataframe level.

I carefully extracted relevant features from the dataset and encoded the target variable to represent distinct classes. I recognized that group attributes are particularly important, as their booking and cancellations reflect heavily. I merged the individual and group bookings into one. This way, revenue could be correctly reflected. I performed other pre-processing tasks like standard scaling, frequency encoding, and removing null values.

## Section 2: Model Selection and Evaluation

The first model I worked on aimed to classify whether the flight sells out 30 days prior to departure, which is not desirable for airline companies who sell perishable tickets.

I began by selecting a single Flight 0784 LOS-DXB, which had a reasonable amount of data (around 500 thousand records). I performed model selection on this test flight before implementing the process for all 1024 flights.

### Random Forest Classifier

First, I tried the Random Forest Classifier. I trained the model on the training data and then predicted the labels for the testing data. The results were promising.

To better visualize the model's performance, I utilized a confusion matrix to display the number of true positives, true negatives, false positives, and false negatives.

### Hyperparameter Tuning

To optimize the Random Forest Classifier further, I conducted hyperparameter tuning using GridSearchCV and RandomizedSearchCV. GridSearchCV involved exploring various combinations of hyperparameters, such as `n_estimators`, `max_features`, `max_depth`, and `max_leaf_nodes`, to find the optimal configuration. After identifying the best hyperparameters, I refitted the model and reevaluated its performance, resulting in improved accuracy and an ROC AUC score.

Taking a different approach, I employed RandomizedSearchCV for hyperparameter tuning. This technique allowed me to randomly sample hyperparameter values from a predefined parameter grid. After evaluating the model with these randomly selected hyperparameters, I determined the best combination and refitted the model.

### Exploring Other Models

I also tested a variety of other models, including XGBoost Classifier, Gaussian Naive Bayes, Decision Tree Classifier, and SVM, with a linear kernel. Each model was trained on the training data, and their respective label predictions were made for the testing data.

After thorough evaluation, I found XGBoost Classifier to be the best performing model. I performed additional hyperparameter tuning to find the best parameters to apply to my classifier.

### Model Creation Process

I followed the below steps to create models for each flight:

1. Import the necessary libraries, including pandas, create_engine from sqlalchemy, tqdm for progress tracking, gc for garbage collection, and relevant functions from scikit-learn for evaluation.

2. Establish a connection to the SQL database using the create_engine function from sqlalchemy.

3. Define an SQL query to retrieve distinct flight numbers (fltnum) from the database, sorted in descending order.

4. Set the chunksize, which determines the number of rows to be processed at a time. This helps handle large datasets efficiently.

5. Calculate the total number of rows and chunks to be processed.

6. Initialize a progress bar using tqdm to keep track of the processing progress.

7. Iterate through the chunks obtained from the SQL query and perform the following steps for each chunk:
   
   - Retrieve data for the specific flight from the database.
   - Preprocess the data, filling NaN values, and calculating a new feature 'sodp' (Sold-out Days-Prior).
   - Determine whether the flight is consistently fully booked, never fully booked, or requires class balancing due to imbalanced data.
   - If the flight requires class balancing, perform oversampling using the `resample` function to address the imbalance.
   - Encode categorical features using frequency encoding for modeling purposes.
   - Split the data into training and testing sets based on the snapshot date (time-based splitting).
   - Build an XGBoost classifier model and fit it to the training data.
   - Predict the labels for the test data and evaluate the model's performance using ROC AUC score and accuracy metrics.

Overall, the XGBoost Classifier demonstrated superior performance in classifying flight sell-outs, and the data preprocessing steps played a crucial role in ensuring accurate predictions.
