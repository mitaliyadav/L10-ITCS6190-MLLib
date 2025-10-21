# Ride Sharing Analytics Using Spark Streaming and Spark SQL.
---
## **Prerequisites**
Before starting the assignment, ensure you have the following software installed and properly configured on your machine:
1. **Python 3.x**:
   - [Download and Install Python](https://www.python.org/downloads/)
   - Verify installation:
     ```bash
     python3 --version
     ```

2. **PySpark**:
   - Install using `pip`:
     ```bash
     pip install pyspark
     ```

3. **Faker**:
   - Install using `pip`:
     ```bash
     pip install faker
     ```

---

## **Setup Instructions**

### **1. Project Structure**

The project directory follows the structure below:

```
ride-sharing-analytics/
├── data_generator.py
├── task4.py
├── task5.py
├── task4_screenshot.png
├── task5_screenshot.png
├── data_generator.py
├──training-dataset.csv
├──models/
└── README.md
```

- **data_generator.py/**: generates a constant stream of input data of the schema (trip_id, driver_id, distance_km, fare_amount, timestamp)  
- **README.md**: Assignment instructions and guidelines.
  
---

### **2. Running the Analysis Tasks**

You can run the analysis tasks either locally.

1. **Execute Each Task** : The data_generator.py should be continuosly running on a terminal. open a new terminal to execute each of the tasks.
   ```bash
     python data_generator.py
     python task4.py
     python task5.py
   ```

2. **Verify the Outputs**:
   Check the terminal for the results
---
# **Task4**
## **Overview**

This task demonstrates how to build a real-time fare prediction pipeline using PySpark’s Structured Streaming and MLlib.
The goal is to train a simple Linear Regression model that predicts taxi fares (fare_amount) based on trip distance (distance_km), and then use that trained model to make real-time predictions on streaming data.

**The pipeline consists of two main phases:**

**Offline Model Training**

* Loads historical training data from training-dataset.csv.

* Casts the columns distance_km and fare_amount to DoubleType for numerical processing.

* Uses a VectorAssembler to convert distance_km into a features vector required by Spark MLlib models.

* Trains a Linear Regression model using distance_km as the feature and fare_amount as the label.

* Saves the trained model to models/fare_model for reuse during streaming inference.

**Streaming Inference**

* Reads live trip data as JSON from a socket stream (e.g., via localhost:9999).

* Parses the incoming stream using a defined schema.

* Loads the pre-trained Linear Regression model from disk.

* Uses the same VectorAssembler transformation on the streaming data to ensure consistency with the training phase.

* Generates real-time fare predictions using the model.

* Calculates the deviation between the actual fare_amount and the predicted fare using an absolute difference (abs_diff).

* Outputs a live stream of results to the console.

## Key Features

* End-to-End Workflow: Integrates offline model training and real-time inference in a single pipeline.

* Consistent Feature Engineering: Ensures the same preprocessing steps (VectorAssembler) are applied in both training and inference.

* Streaming Data Processing: Reads and processes JSON-formatted data in real time using Spark Structured Streaming.

* Model Reusability: Automatically detects existing models to avoid unnecessary retraining.

* Real-Time Evaluation: Computes a live deviation metric between predicted and actual fares to monitor model accuracy.

* Scalable Design: Can easily be extended to include more features or advanced ML models.
  
## *Sample Output*
<img width="2128" height="842" alt="Screenshot 2025-10-21 123151" src="https://github.com/user-attachments/assets/a0023ea8-d5fa-468e-acd2-932b51201411" />

---


# **Task5**

## **Overview**

This project demonstrates offline model training and real-time streaming inference for predicting average taxi fares using PySpark’s Structured Streaming and MLlib. The workflow aggregates fare data into 5-minute windows and trains a Linear Regression model using time-based features.

**The pipeline consists of two main phases:**

**Offline Model Training**

* Loads historical fare data (training-dataset.csv).

* Aggregates fare data into 5-minute windows.

* Extracts hour-of-day and minute-of-hour features from the window start time.

* Trains a Linear Regression model to predict average fare.

* Saves the trained model to models/fare_trend_model_v2.

**Streaming Inference**

* Reads real-time fare data from a socket stream in JSON format.

* Applies the same 5-minute windowed aggregation.

* Extracts the same time-based features for prediction.

* Loads the pre-trained model and outputs:

   * Window start and end times

   * Actual average fare

   * Predicted next average fare

* Prints results to the console in real time.

## Key Features

* Uses PySpark MLlib for regression modeling.

* Handles streaming data using Structured Streaming.

* Extracts time-based features from aggregated windows.

* Predicts average fare trends in real time.

* Automatically checks for existing trained models to save computation time.

## *Sample Output*
<img width="1665" height="354" alt="Screenshot 2025-10-21 123056" src="https://github.com/user-attachments/assets/8002417c-7ac0-4efd-9a58-d5f07b12506a" />

---
