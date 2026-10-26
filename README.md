<div align="center">

![Weather Intelligence Dashboard Banner](PASTE_URL_GAMBAR_DARI_GITHUB_ISSUES_DISINI)

# 🌦️ Weather Intelligence Dashboard

### Real-Time Forecasting · Machine Learning · Weather Pattern Analysis

A weather dashboard powered by **Google Apps Script**, **Google Sheets**, **Open-Meteo**, and a Python machine-learning pipeline developed in **Google Colab**.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Open%20Dashboard-2563eb?style=for-the-badge&logo=googlechrome&logoColor=white)](https://script.google.com/macros/s/AKfycbyJA6grCm47XUp_tQ3tCCjSmTuSNRfoYzdlvb4KLMlnS8qj5bCPM0RiguX5k4eYG3zJ/exec)<br>
[![Google Apps Script](https://img.shields.io/badge/Google%20Apps%20Script-Web%20App-4285F4?style=flat-square&logo=googleappsscript&logoColor=white)](https://developers.google.com/apps-script)
[![Python](https://img.shields.io/badge/Python-ML%20Pipeline-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Open-Meteo](https://img.shields.io/badge/Weather%20Data-Open--Meteo-0B8F87?style=flat-square)](https://open-meteo.com/)

[Overview](#-overview) · [Features](#-features) · [Architecture](#-architecture) · [Model Results](#-model-evaluation) · [Setup](#-setup-and-deployment) · [Limitations](#-limitations)

</div>

---

## 📌 Overview

**Weather Intelligence Dashboard** is a web application for exploring current weather conditions, hourly forecasts, and machine-learning predictions for a selected city or coordinate. It combines Open-Meteo weather data with a locally evaluated **Gradient Boosting** model and **K-Means clustering** to make forecast patterns easier to inspect.

The current `weather-v2` model was developed for **Surabaya, East Java, Indonesia** (approximately `-7.2575, 112.7521`). Results for other locations have not been established.

### Live demo

Open the deployed dashboard here:

<div align="left">

[![Launch Weather Intelligence Dashboard](https://img.shields.io/badge/Launch-Weather_Intelligence_Dashboard-0056b3?style=for-the-badge)](https://script.google.com/macros/s/AKfycbyJA6grCm47XUp_tQ3tCCjSmTuSNRfoYzdlvb4KLMlnS8qj5bCPM0RiguX5k4eYG3zJ/exec)

</div>

## ✨ Features

| Area | Features |
|---|---|
| **Current weather** | Temperature, feels-like temperature, humidity, precipitation, wind speed, and pressure. |
| **Hourly forecast** | Explore weather trends across the next 24 hours. |
| **ML predictions** | Estimate temperature and rain probability for **+1, +3, +6, and +24 hours**. |
| **Forecast comparison** | Compare model predictions with the Open-Meteo forecast baseline. |
| **K-Means clustering** | Group the 24 hourly forecast points into three weather-pattern clusters. |
| **Historical evaluation** | Track temperature absolute error and rain-probability Brier score when historical reference data becomes available. |
| **Experiment tracking** | Record model experiments with MLflow and SQLite in the Colab environment. |
| **Data logging** | Store weather snapshots, predictions, model versions, and evaluation results in Google Sheets. |

### Prediction targets

- **Temperature:** the model predicts a temperature change, which is added to the current temperature: `T(t+h) = T(t) + predicted_delta`.
- **Rain probability:** estimates the probability that precipitation at the target hour is at least **0.1 mm**. It is not the cumulative probability of rain over the full forecast horizon.

K-Means cluster descriptions are interpretations of the current forecast pattern, not official weather warnings. Cluster IDs can change between runs.

## 🧱 Architecture

### Model training and export

```mermaid
flowchart TD
    A[Open-Meteo Historical Weather API] --> B[Colab: data preparation and hourly alignment]
    B --> C[38-feature engineering]
    C --> D[Chronological train / validation / test split]
    D --> E[Model selection and evaluation]
    E --> F[MLflow tracking with SQLite]
    E --> G[weather_model.json and parity fixtures]
    G --> H[Private Google Drive file]
