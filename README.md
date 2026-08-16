# Flower Species Prediction Model

A simple end-to-end machine learning project that trains a classification model on the Iris dataset and exposes the trained model through a Flask API. The project also includes Docker support and a GitHub Actions CI pipeline to automate dependency installation, testing, and Docker image builds.

## Overview

The goal of this project is to demonstrate a basic machine learning workflow from **model training to deployment**.

The training script loads the Iris dataset from scikit-learn, splits the data into training and testing sets, trains a Logistic Regression model, evaluates its accuracy, and saves the trained model as an artifact.

The trained model can then be used for predictions through either a command-line script or a Flask REST API.

## Project Workflow

```text
Iris Dataset
     |
     v
Data Preparation
     |
     v
Train/Test Split
     |
     v
Logistic Regression
     |
     v
Model Evaluation
     |
     v
Save Model Artifact
     |
     +------------------+
     |                  |
     v                  v
CLI Prediction      Flask REST API
                        |
                        v
                    Docker
                        |
                        v
                 CI with GitHub Actions
```

## Features

* Uses the built-in **Iris dataset** from scikit-learn
* Trains a **Logistic Regression** classification model
* Splits data into training and testing sets
* Saves the trained model using **Joblib**
* Stores model evaluation metrics as JSON
* Provides command-line prediction support
* Exposes a Flask REST API for predictions
* Includes a `/health` endpoint for basic service health checks
* Containerized using Docker
* Includes a GitHub Actions CI pipeline
* Automatically builds the Docker image during CI

## Technologies Used

* **Python 3.9**
* **Scikit-learn** – machine learning and model training
* **Pandas** – data processing
* **NumPy** – numerical operations
* **Joblib** – model serialization
* **Flask** – REST API
* **Docker** – application containerization
* **GitHub Actions** – CI automation

## Machine Learning Model

The project uses **Logistic Regression** from scikit-learn for classification.

The Iris dataset is split into:

* **80% training data**
* **20% testing data**

The model is trained using the training set and evaluated against the test set. The resulting accuracy is stored in:

```text
artifacts/metrics.json
```

The trained model is saved as:

```text
artifacts/model.pkl
```

The training configuration uses a fixed random state to make the train/test split reproducible.

## Running the Project Locally

### 1. Clone the Repository

```bash
git clone https://github.com/NRavuri/FlowerSpeciesPredectionModel.git
cd FlowerSpeciesPredectionModel
```

### 2. Create a Virtual Environment

```bash
python3 -m venv venv
```

Activate it on Linux/macOS:

```bash
source venv/bin/activate
```

On Windows:

```bash
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

## Train the Model

Run:

```bash
python train.py
```

This will:

1. Load the Iris dataset
2. Split the data into training and testing sets
3. Train the Logistic Regression model
4. Evaluate the model
5. Save the trained model to `artifacts/model.pkl`
6. Save the accuracy to `artifacts/metrics.json`

## Run a Prediction

After training the model, predictions can be generated from the command line:

```bash
python run_model.py --input "[5.1,3.5,1.4,0.2]"
```

Example response:

```json
{
  "prediction": [0]
}
```

The four input values represent the standard Iris dataset features:

* Sepal length
* Sepal width
* Petal length
* Petal width

## Run the Flask API

Start the application:

```bash
python app.py
```

The API will run on:

```text
http://localhost:5001
```

### Health Check

```bash
curl http://localhost:5001/health
```

Expected response:

```json
{
  "status": "ok"
}
```

### Prediction API

Send a POST request to `/predict`:

```bash
curl -X POST http://localhost:5001/predict \
  -H "Content-Type: application/json" \
  -d '{"features":[5.1,3.5,1.4,0.2]}'
```

The API returns the predicted class:

```json
{
  "prediction": 0
}
```

## Docker

The application includes a Dockerfile based on the lightweight `python:3.9-slim` image. The container installs the required dependencies, copies the application code, exposes port `5001`, and starts the Flask application.

### Build the Image

```bash
docker build -t flower-species-prediction-model .
```

### Run the Container

```bash
docker run -p 5001:5001 flower-species-prediction-model
```

The API can then be accessed at:

```text
http://localhost:5001
```

## CI Pipeline

The project uses **GitHub Actions** to automate basic CI tasks whenever changes are pushed to the `main` branch or a pull request is opened against it.

The pipeline:

1. Checks out the repository
2. Sets up Python 3.9
3. Installs project dependencies
4. Runs the available tests
5. Builds the Docker image

This provides an automated validation step before changes are merged.

## Project Structure

```text
FlowerSpeciesPredectionModel/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── artifacts/
│   ├── model.pkl
│   └── metrics.json
│
├── app.py
├── train.py
├── run_model.py
├── Dockerfile
├── requirements.txt
└── .gitignore
```

## What I Learned

This project helped me work through the complete lifecycle of a small machine learning application:

* Training and evaluating a classification model
* Saving and loading ML model artifacts
* Building a REST API around a trained model
* Containerizing an ML application with Docker
* Automating validation and Docker builds with GitHub Actions
* Separating model training from model inference

## Future Improvements

Some possible improvements for the project include:

* Add automated unit and API tests
* Add model versioning and experiment tracking with MLflow
* Add input validation and better API error handling
* Add model performance monitoring
* Deploy the container to AWS
* Add automated Docker image publishing through GitHub Actions
* Add a simple web UI for interacting with the prediction API

## Author

**Nikhil Ravuri**

GitHub: https://github.com/NRavuri
