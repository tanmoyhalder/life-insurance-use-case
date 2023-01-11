Author: Tanmoy Halder
Date: 09/01/2023
Updated: 10/01/2023

### Pre-requisite

You should already have mlflow installed in your environment. If not, please run `pipenv install mlflow` (or you can use conda /pip as well)

### Create a experiment tracking folder

In that folder create a folder called **data** and store your input data
Also create another empty folder called **models**. The models will be stored here 
Copy the `persistency_base_model - modified data2.ipynb` notebook and rename as per your convenience. I have renamed to `persistency-prediction.ipynb`

### Set Backend database

To store models, artifacts, parameters and other metadata and specifically to store models, we need some backend database. To do that

- In your notebook, write in a cell

`import mlflow`

`mlflow.set_tracking_uri("sqlite:///mlflow.db")`
`mlflow.set_experiment("persistency-prediction-experiment")`  -- you can give your own experiment name

### Launch the mlflow ui

- Ensure you are in the newly created folder
- While launching the mlflow ui thorugh CLI run `mlflow ui --backend-store-uri sqlite:///mlflow.db`

### Follow code

`persistency_first_experiment`
`persistency_second_experiment - mlflow + hyperopt + xgboost.ipynb`
`persistency_third_experiment - mlflow + hyperopt + xgboost - new data.ipynb` -- this is the final experiment code


