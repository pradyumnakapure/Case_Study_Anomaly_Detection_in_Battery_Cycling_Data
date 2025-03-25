Data Science Case Study 2025 by Pradyumna Kapure
Link of Case_Studey - https://onedrive.live.com/ redeem=aHR0cHM6Ly8xZHJ2Lm1zL2YvYy83YzU4Y2M3OGFjMzBiMmIxL0VsVlVSNUlLcmdsS3ZOdFFfN1JyZ3R3Qlh5THFlMTlsS2tQNlJuRFRzQzhPVUE%5FZT00amJsRnY&id=7C58CC78AC30B2B1%21s92475455ae0a4a09bcdb50ffb46b82dc&cid=7C58CC78AC30B2B1
Problem Statement
-----------------
You are provided with a battery cell cycling dataset in parquet format, which contains anomalies at both the point and cycle levels. Your task is to:
- Analyze and interpret the data to identify trends and patterns.
- Improve data quality by detecting and removing anomalies using statistical and machine-learning methods.
- Implement unit and integration tests for your anomaly detection methods.
- Document your code using Sphinx.
- Create an automated data pipeline using Airflow.
- Implement model and data versioning with MLFlow.
- Present the results in a dashboard using Power BI.

The solution must adhere to PEP8 guidelines, provide clear reasoning through comments and documentation, and ensure reproducibility.

Installation Procedures
-----------------------
1. Clone or Download the Project
   - Clone the repository or download the project folder to your local machine.

2. Install Dependencies
   - Create a `requirements.txt` file with the following libraries and versions:
     pandas==2.2.2
     numpy==1.26.4
     matplotlib==3.9.2
     seaborn==0.13.2
     scikit-learn==1.5.1
     scipy==1.14.1
     pyarrow==17.0.0  # For parquet file handling
     pytest==8.3.2
     sphinx==7.4.7
     apache-airflow==2.10.1
     mlflow==2.16.0
   - Install the dependencies using:
     pip install -r requirements.txt

3. Set Up the Dataset
   - Place the provided `case_study_sample_dataset.gzip` file into the `data/` directory of the project.

4. Configure Airflow
   - Initialize the Airflow database:
     airflow db init
   - Start the Airflow webserver and scheduler:
     airflow webserver --port 8080 &
     airflow scheduler &
   - Ensure the DAG file (`src/pipeline.py`) is in the Airflow `dags/` folder.

5. Set Up Sphinx Documentation
   - Navigate to the `docs/` folder and run:
     sphinx-quickstart
   - Update `docs/conf.py` to include:
     extensions = ['sphinx.ext.autodoc']
     sys.path.insert(0, os.path.abspath('../src'))

6. Start MLFlow UI
   - Launch MLFlow to track model versions and metrics:
     mlflow ui
   - Access the UI at http://localhost:5000.
7. Reasoning for the code :
   i. Why parquet?: The dataset is provided in parquet format, which is efficient for columnar data storage. pandas.read_parquet is used with pyarrow as the engine.
  ii. Exploration: Checking data types, missing values, and summary statistics ensures we understand the dataset’s structure. The initial plot mimics the example provided, helping identify    
      trends like voltage degradation over cycles.
 iii. Point Anomalies: Used spline fitting over Z-score or LOF because it respects the functional relationship between discharge capacity and voltage. Points deviating significantly from 
      the fitted curve (beyond 3 standard deviations of residuals) are flagged as anomalies.
  iv. Cycle Anomalies: Extracted features (e.g., max capacity, area under curve) capture cycle-level behavior. Isolation Forest was chosen for its effectiveness in unsupervised anomaly 
      detection on multivariate data, assuming 5% of cycles are anomalous (tunable parameter).
   v. Why not manual removal?: Automated methods ensure scalability and reproducibility, aligning with the task’s requirements.
  vi. Why these tests?: They verify that the anomaly detection correctly identifies outliers and that feature extraction computes expected values, ensuring reliability of the core methods.
 vii. Why Sphinx?: It integrates well with Python docstrings, automating documentation generation. The sample covers key functions, demonstrating the approach.
viii. Why Airflow?: It’s widely used for scheduling and monitoring workflows. Tasks are modular, with data passed via files due to Airflow’s design.
  ix. Why MLFlow?: It tracks model parameters, metrics, and artifacts, ensuring reproducibility and versioning of the anomaly detection model and cleaned data.
   x. Why Power BI?: It’s user-friendly for interactive dashboards, suitable for presenting trends and anomalies to stakeholders.
