# Electricity Price Prediction Project

## Introduction

There are two CSV files:
1. Electricity price (from multiple European countries)
2. Load values (how much energy available from power plant)

Data resolution:
- Some data are with hourly resolution (60mins)
- Some are quarterly (15mins)

Prediction target: "AT-day_ahead_price-15min"

## Step 1: Exploratory Data Analysis (EDA)

1. Check missing values (percentage) of CSVs
2. Check statistics of each price feature column:
   - Mean
   - Median
   - Max
   - Min
   - What do you observe?
3. Visualize distribution (histogram) of "AT-day_ahead_price-15min"
4. Calculate Pearson and Spearman correlation between "AT-day_ahead_price-15min" and "DE_LU-day_ahead_price-15min"
   - What is the difference between Pearson and Spearman?
   - How high is considered a high correlation?
5. Visualize scatter plot between "AT-day_ahead_price-15min" and "AT-Actual Load"
   - Is there correlation?
6. Remove feature columns from both CSV files if they are not quarterly
7. Fill missing values for remained data using interpolation
   - What is interpolation? 
   - What is the difference compared to BFill and FFill used in Python?
8. Merge the preprocessed CSVs into 1 CSV
9. Scale data using min-max scaler
   - What is the difference between min-max scaler and standard scaler?

## Step 2: Rolling Window Design

Goal: Predict the next quarterly "AT-day_ahead_price-15min" using the previous data with all feature columns

- What is the difference between univariate and multivariate time series?

Write a function called `rolling_window`:
- Input: cleaned and processed CSV from step 1
- Output: 3D numpy array with the dimension of (samples, timestamp, features)

Example:
- timestamp = 12, features = 4 means using the previous 12 ROWS (12 x 15mins = 3 hours) or 3 hours data to predict the next quarterly "AT-day_ahead_price-15min" price with 4 COLUMNS
- The function should apply a rolling window to loop through all rows and append them into 3D numpy array

Define timestamp = 48 for this step

## Step 3: Model Training and Optimization

1. Data splitting:
   - Training data: 2022 and 2023
   - Validation data: 2024.1-2024.3
   - Testing data: 2024.3-2024.6

2. Use LSTM and simple RNN (both in Keras)
   - Hyperparameters: 
     - Number of layers
     - Number of neurons
     - Dropout rate
     - Learning rate
   - What is dropout rate?

3. Apply Optuna to optimize the hyperparameters with validation data (metric: MSE)
   - Save the best model
   - Which hyperparameter combination gives you the lowest validation error?

4. Use the saved best model to calculate the RMSE, MAE, MAPE with testing data
   - Why do researchers dislike MAPE?

5. Which model gives you lower testing loss, LSTM or simple RNN?

## Step 4: Visualization

Draw a matplotlib figure to show predicted prices vs true prices on testing data ("AT-day_ahead_price-15min"):
- x-axis: time
- y-axis: price
