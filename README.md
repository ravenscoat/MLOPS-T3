# MLOPS-assignment1
House Price Prediction Service
Overview
This project involves building, testing, and deploying a machine learning model as a service for predicting house prices based on input data. The solution incorporates modern MLOps best practices, including Continuous Integration (CI) and Continuous Deployment (CD), to create a robust pipeline from development to production. The service is exposed via a Flask API and includes a frontend interface for user interaction.

Project Components
Machine Learning Model
Model: Predicts house prices based on various input features.
Training: The model is trained on historical housing data to provide accurate predictions.
Backend API
Flask: Provides an API endpoint for predicting house prices. The API accepts input data and returns predicted prices.
Frontend Interface
HTML/CSS: A simple web interface where users can input data and receive predictions from the model.
Deployment and CI/CD
GitHub: Used for version control, including branching, pull requests, and issue tracking.
GitHub Actions: Automates the CI pipeline, including running tests and building the application.
Vercel: Deploys the model to production and makes the API accessible. The deployment process is managed through GitHub Actions, ensuring smooth transitions across development, staging, and production environments.