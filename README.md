# Bed Capacity Data Analysis for Ronald Reagan Medical Center from Q1 2022 to Q3 2024

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Motivation](#motivation)  
3. [Data Description](#data-description)  
   - [Data Acknowledgment](#data-acknowledgment)  
   - [Data Structure](#data-structure)  
   - [Feature Descriptions](#feature-descriptions)  
4. [Objective](#objective)  
5. [Methodology](#methodology)  
   - [Data Preprocessing](#data-preprocessing)  
   - [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)  
   - [Feature Engineering](#feature-engineering)  
   - [Model Development](#model-development)  
   - [Model Evaluation](#model-evaluation)  
6. [Results](#results)  
   - [Key Findings](#key-findings)  
   - [Performance Metrics](#performance-metrics)  
7. [Conclusion](#conclusion)  
   - [Insights](#insights)  
   - [Limitations](#limitations)  
   - [Future Work](#future-work)  
8. [Dependencies](#dependencies)  
9. [Project Structure](#project-structure)
10. [References](#references)
11. [Contact Information](#contact-information)  

## Project Overview

This project focuses on analyzing and forecasting bed capacity in the delivery ward of the OB/GYN department at Ronald Reagan Medical Center. By examining data from early 2022 through September 2024, we aim to provide actionable insights to enhance patient flow and optimize operational efficiency at UCLA Health, specifically to our stakeholder Scott Jahnke, Director of Operations.

Key Objectives:
- Perform exploratory data analysis (EDA) to uncover trends and patterns in bed usage within the delivery ward.
- Develop a robust time series forecasting model to predict bed capacity needs for the next few months.
- Utilize these predictions to support decision-making and improve resource allocation, thereby streamlining patient flow in the delivery ward.
  
Scope:
The analysis includes data on admission time, bed logistics, and other relevant factors impacting bed availability. A Random Forest model is applied to evaluate the relationship between key features, such as total admissions, and weekly bed capacity. An LSTM model is deployed to forecast delivery volumes, with the training data spanning the years 2022 and 2023; the model is then used to predict delivery outcomes from the beginning of 2024 through September 2024, allowing for a comparison with actual 2024 data.

Impact:
This project serves as a strategic tool for the OB/GYN department and UCLA Health's operations team to:

- Mitigate overcrowding risks, especially in the case of deliveries where the health of the baby and mother are crucial.
- Ensure smoother patient flow, reducing costs and increasing successful deliveries.
- Enhance overall service delivery for expectant mothers.

## Motivation

TO-DO

## Data Description

### Data Acknowledgment

The data used in this project was provided by UCLA Health DDR (De-identified Data Repository) under strict privacy and security regulations. Due to HIPAA and privacy policies, the raw data and scripts cannot be shared.

The dataset contains anonymized records related to weekly hospital admissions and bed capacity trends for the delivery ward in the OB/GYN department of Ronald Reagan Medical Center. All analyses and findings were conducted in compliance with data protection guidelines to ensure confidentiality.

### Data Structure

### Feature Descriptions

## Objective

## Methodology

### Data Preprocessing

### Exploratory Data Analysis (EDA)

### Feature Engineering

### Model Development

### Model Evaluation

## Results

### Key Findings

### Performance Metrics

## Conclusion

### Insights

### Limitations

- **Data Accessibility**: Due to HIPAA and privacy regulations set by UCLA Health, raw data, scripts, and notebooks could not be exported. There are still screenshots of the data structure and code available for observation.

- **Data Complexity**: Due to the intricate relationships between variables and possible confounding factors, models tended to overfit and consequently overperform, leading to inaccurate predictions.

### Future Work

TO-DO

## Dependencies

## Project Structure

project-name/
|
├── 
└── README.md  # Project documentation

## References



## Contact Information

For any questions, please contact:
- Name: Christopher Fu
- Email: christopherfuwas@gmail.com
