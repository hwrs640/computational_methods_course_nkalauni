# HWRS640 - Assignment 2: Regression, ODE solvers, and optimization

## Due date: Friday, February 20th at 11:59 PM

## Problem 1: Shuffled complex evolution (SCE) optimization (25 points)
Read the original paper by Duan et al. (1992) on the Shuffled Complex Evolution (SCE) optimization algorithm (https://doi.org/10.1007/BF00939380). Summarize the main steps of the SCE algorithm in your own words. Additionally discuss the problems that the SCE algorithm is designed to solve, and how it compares to other optimization algorithms (e.g., gradient descent, genetic algorithms).

## Problem 2: Regression for streamflow prediction (25 points)
In `/data/LeafRiverDaily.csv` you have been given daily temperature, precipitation, and streamflow data for the Leaf River in Mississippi. Your task is to build a regression model to predict daily streamflow based on the temperature and precipitation data. Perform the following steps:

1. Load the data into a pandas DataFrame and perform any necessary preprocessing (e.g., handling missing values, feature scaling). Consider a sample as a 90 day history of temperature and precipitation data, and the target variable as the streamflow on the 91st day.
2. Split the data into training and testing sets (e.g., 80% training, 20% testing) to evaluate the performance of your regression model. 
3. Train a linear regression model on the training data and evaluate its performance on the testing data using appropriate metrics (e.g., R-squared, mean absolute error). Use a sample size where inputs are the previous 90 days of temperature and precipitation data, and the target variable is the streamflow on the 91st day. You may need to reshape your data accordingly to fit this format.

## Problem 3: Calibration of a simple hydrological model (25 points)
For this problem you will implement and calibrate a "simple" nonlinear hydrological model using the SCE optimization algorithm. The model is defined as follows:

$$
Q(t) = a \cdot P(t) + b \cdot T(t) +
c \cdot Q(t-1) + d
$$

Where:
- $Q(t)$ is the streamflow at time $t$ (the target variable).
- $P(t)$ is the precipitation at time $t$ (a feature).
- $T(t)$ is the temperature at time $t$ (a feature).
- $Q(t-1)$ is the streamflow at time $t-1$ (a feature).
- $a$, $b$, $c$, and $d$ are the model parameters that we need to calibrate.

Perform the following steps:
1. Implement the model and integration using scipy's `odeint` function. You will need to define a function that computes the derivatives of the state variables (in this case, just $Q$) based on the model equations.
2. Define an objective function that computes the mean squared error between the observed streamflow and the model predictions for a given set of parameters.
3. Use the SCE optimization algorithm to find the optimal parameters $a$, $b$, $c$, and $d$ that minimize the objective function using the SCE-UA algorithm as implemented with the `spotpy` library.

## Problem 4: Comparison of linear regression and SCE optimization (25 points)
Compare the performance of the linear regression model and the SCE optimization algorithm for predicting streamflow from the previous problems based on the same dataset. Perform the following steps:

1. Calculate the Overall NSE (Nash-Sutcliffe Efficiency) for both models on the testing data. The NSE is defined as:
$$
NSE = 1 - \frac{\sum_{i=1}^{n} (Q_{obs,i} - Q_{pred,i})^2}{\sum_{i=1}^{n} (Q_{obs,i} - \bar{Q}_{obs})^2}
$$
Where $Q_{obs,i}$ is the observed streamflow, $Q_{pred,i}$ is the predicted streamflow, and $\bar{Q}_{obs}$ is the mean of the observed streamflow.
2. Create a figure that compares the observed streamflow with the predictions from both models over time. Include appropriate labels, legends, and titles to clearly distinguish between the observed data and the predictions from the linear regression and SCE models.
3. Discuss the results: Which model performs better in terms of NSE? What are the strengths and weaknesses of each model? How might you improve the performance of each model? 


