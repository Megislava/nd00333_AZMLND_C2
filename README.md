# Operationalizing Machine Learning

This project centers on a bank marketing campaign and aims to predict whether a client will subscribe to a term deposit based on data from direct marketing activities, specifically phone calls. An automated machine learning (AutoML) solution in Azure is used to address this task. The dataset, sourced from the UCI Machine Learning Repository, is uploaded to Azure Machine Learning Studio, where an AutoML job is executed to identify the most suitable predictive model. The selected model is then deployed as a REST endpoint for real-time inference. In addition, a machine learning pipeline is implemented to automate the workflow from data preprocessing through model training to deployment, ensuring repeatability, scalability, and maintainability. Overall, the project demonstrates the complete lifecycle of a machine learning solution, from data preparation and model development to deployment and integration into a production environment.

## Architectural Diagram
1. Authentication
2. Automated ML Experiment
3. Deploy the best model
4. Enable logging
5. Swagger Documentation
6. Consume model endpoints
7. Create and publish a pipeline
8. Documentation

<img width="396" height="188" alt="image" src="https://github.com/user-attachments/assets/3a7ecdc5-a1b8-4072-a7e3-4d065f04663b" />

## Key Steps
1. Authentication

The project is completed using the provided lab environment. As a result, this step is skipped because the user is not authorized to create a service principal within the lab subscription.

<img width="884" height="608" alt="Screenshot 2026-01-28 104420" src="https://github.com/user-attachments/assets/751aa798-cb97-4e81-93a8-7b8d1cc82087" />


2. Automated ML Experiment

The Azure Machine Learning Studio graphical user interface is used to configure and execute an Automated Machine Learning (AutoML) experiment.
The dataset is registered by uploading the CSV file obtained from the UCI Machine Learning Repository ([https://archive.ics.uci.edu/dataset/222/bank+marketing](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv)).

<img width="1120" height="569" alt="image" src="https://github.com/user-attachments/assets/ebdc2c39-f520-437b-acba-5405b4f60c36" />


The task type is set to classification, and the registered dataset is selected when creating the AutoML job.

<img width="1034" height="465" alt="Screenshot 2026-01-28 105349" src="https://github.com/user-attachments/assets/e4f917f5-bb70-45f2-8aaa-17eb3e232e6c" />
<img width="949" height="416" alt="Screenshot 2026-01-28 110209" src="https://github.com/user-attachments/assets/5cb5a022-335b-428c-abc0-8153b2e02a44" />

The target column is specified as y. Maximum concurrent trials: 5, in accordance with project objectives
A compute cluster is required to run the AutoML job. The Standard_D2S_v3 virtual machine size is selected to provide optimal performance.
After reviewing all AutoML configuration settings, the job is submitted for execution. Upon completion of the AutoML run, the best-performing model is identified as a VotingEnsemble, based on the selected evaluation metric.

<img width="1052" height="516" alt="image" src="https://github.com/user-attachments/assets/3c653d66-b802-4cb4-945f-dfc5a1f484b6" />



3. Deployment of the Best Model

The best model is deployed as a web service with the following settings:
The deployment completes successfully. By default, Application Insights is disabled.

<img width="866" height="507" alt="image" src="https://github.com/user-attachments/assets/ecfd8bd9-388b-428a-bb22-9ffb38848742" />
<img width="1035" height="407" alt="image" src="https://github.com/user-attachments/assets/0441a38c-43b0-48c9-ac33-681d71680e30" />



4. Enabling Logging

Application Insights is enabled to monitor operational data such as request traces, response times, exceptions, and failure rates. This functionality is essential for assessing the health and performance of the deployed service and for diagnosing runtime issues in production environments. The deployment name in log.py is updated to name of the deployed model. The following command is added:
`service.update(enable_app_insights=True)`
After executing log.py, Application Insights is successfully enabled.

<img width="590" height="470" alt="Screenshot 2026-01-28 122110" src="https://github.com/user-attachments/assets/d57ed574-64c1-495c-977b-8c72dd972505" />

5. Model Consumption

This section describes how the deployed model is accessed and tested using its REST API endpoint.
After deployment, Azure Machine Learning automatically generates a swagger.json file describing the REST API interface. This file includes endpoint definitions, request and response schemas, and input data formats.
The swagger.json file is downloaded from Azure Machine Learning Studio and can be used with tools such as Swagger UI to validate the endpoint and verify request formatting.
The swagger.sh script launches a local Swagger UI instance to interactively explore the deployed endpoint. The default port is changed from 80 to 9001.
The script is executed in Git Bash using: `bash swagger.sh`. The serve.py script starts a simple HTTP server that exposes the local directory, allowing Swagger UI to load the swagger.json file from localhost. The script is executed using: `python serve.py`.
Swagger UI becomes accessible at: `http://localhost:9001`
To load the API definition, the following URL is entered: `http://localhost:8000/swagger.json`.

<img width="726" height="291" alt="Screenshot 2026-01-28 125234" src="https://github.com/user-attachments/assets/330106c2-764b-4a37-a6d4-f42d252184c5" />

Once loaded, all available endpoints and their input and output schemas can be explored. The endpoint.py script is used to send inference requests to the deployed endpoint. 

<img width="555" height="92" alt="image" src="https://github.com/user-attachments/assets/92b04beb-c9cc-43df-94ee-58f95a13d330" />


6. Creating and Publishing a Pipeline

The Jupyter notebook aml-pipelines-with-automated-machine-learning-step.ipynb is used to create and publish an Azure Machine Learning pipeline. Using the Python SDK, the pipeline automates model training, selection, deployment, and consumption for the bank marketing dataset, enabling repeatable and scalable execution of the end-to-end workflow. Several fields are updated to match the existing workspace configuration.
The script for loading the test dataset is replaced because the original public dataset path is inaccessible. Instead, the dataset is loaded directly from the workspace.

<img width="934" height="295" alt="Screenshot 2026-01-28 130949" src="https://github.com/user-attachments/assets/e1bc70c7-9006-496a-9071-cce4497a79c5" />
<img width="1084" height="530" alt="image" src="https://github.com/user-attachments/assets/1f456cf7-0088-4e99-a752-17f535084213" />



## Screen Recording
Due to company policy restrictions, screen recording is not permitted in the working environment. As a result, a video recording of the project execution cannot be provided. Instead, the project’s functionality and workflow are demonstrated through detailed screenshots and comprehensive step-by-step explanations included throughout this README.

## Standout Suggestions
To improve model accuracy performance, several approaches could be considered, including experimenting with additional feature engineering techniques or increasing the size of the training dataset. Deep learning was not enabled in this run in order to reduce execution time; however, it may be enabled in future runs to further explore potential performance gains.

