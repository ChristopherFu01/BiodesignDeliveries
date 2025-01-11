# Bed Capacity Data Analysis for Ronald Reagan Medical Center from Q1 2022 to Q3 2024

## Table of Contents

1. [Project Overview](#project-overview)
   - [Team Acknowledgments](#team-acknowledgments)
3. [Motivation](#motivation)  
4. [Data Description](#data-description)  
   - [Data Acknowledgment](#data-acknowledgment)  
   - [Data Structure](#data-structure)  
   - [Feature Descriptions](#feature-descriptions)  
5. [Objective](#objective)  
6. [Methodology](#methodology)  
   - [Data Preprocessing](#data-preprocessing)  
   - [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)  
   - [Feature Engineering](#feature-engineering)  
   - [Model Development](#model-development)  
   - [Model Evaluation](#model-evaluation)  
7. [Results](#results)  
   - [Key Findings](#key-findings)  
   - [Performance Metrics](#performance-metrics)  
8. [Conclusion](#conclusion)  
   - [Insights](#insights)  
   - [Other Notes](#other-notes)  
9. [Dependencies](#dependencies)  
10. [Project Structure](#project-structure)
11. [References](#references)
12. [Contact Information](#contact-information)  

## Project Overview

This project focuses on analyzing and forecasting bed capacity in the delivery ward of the OB/GYN department at Ronald Reagan Medical Center. By examining data from early 2022 through September 2024, we aim to provide actionable insights to enhance patient flow and optimize operational efficiency at UCLA Health, specifically to our stakeholder Scott Jahnke, Director of Operations.

Key Objectives:
1. Perform exploratory data analysis (EDA) to uncover trends and patterns in bed usage within the delivery ward.
2. Develop a robust time series forecasting model to predict bed capacity needs for the next few months.
3. Utilize these predictions to support decision-making and improve resource allocation, thereby streamlining patient flow in the delivery ward.
  
Scope:
The analysis includes data on admission time, bed logistics, and other relevant factors impacting bed availability. A Random Forest model is applied to evaluate the relationship between key features, such as total admissions, and weekly bed capacity. An LSTM model is deployed to forecast delivery volumes, with the training data spanning the years 2022 and 2023; the model is then used to predict delivery outcomes from the beginning of 2024 through September 2024, allowing for a comparison with actual 2024 data.

Impact:
This project serves as a strategic tool for the OB/GYN department and UCLA Health's operations team to:

- Mitigate overcrowding risks, especially in the case of deliveries where the health of the baby and mother are crucial.
- Ensure smoother patient flow, reducing costs and increasing successful deliveries.
- Enhance overall service delivery for expectant mothers.

Recommendations:
Finetuning the LSTM model further is required in order for a proper deployment and application in operational planning and resource allocation within the hospital. Next steps would be the following:

1. Incorporating Patients with Predicted Due Dates
2. Incorporating Scheduled C-Sections
3. Larger-Scale Hyperparameter Optimization
4. Stacking or Ensembling with Regression-Based Models

### Team Acknowledgments

I would like to extend my gratitude to everyone who contribued to this project:

- Courtney Rauchman
- Adi Pillai
- Troy Sisson
- Waleed Khan
- Ronan MacRunnels

## Motivation

Obstetrics (OB) and maternity wards are vital for the health and safety of mothers and their babies. Despite significant advancements in childbirth over the past century, challenges such as complications, errors, and opportunities for improvement persist,1 particularly in protecting patients within the OB department. This is especially critical for populations with historically higher complication rates and strained relationships with the healthcare system. Each statistic represents a mother, a baby, and a family navigating one of the most vulnerable periods of their lives, underscoring the urgency of addressing challenges in OB care. As technology evolves rapidly in this sector, establishing robust infrastructure and conducting rigorous experimentation is essential to ensure these tools effectively benefit OB patients.

Compounding these challenges, California’s labor and delivery departments are closing at a rate exceeding the national average, with 56 closures since 2012 and eight more planned for 2024 alone.2 This trend disproportionately impacts residents in rural areas, patients of color, and those from lower socioeconomic backgrounds. With fewer OB care settings and reduced staffing, it is essential for hospital systems to optimize patient flow within the OB ward, ensuring that the maximum number of patients can receive adequate prenatal and perinatal care, regardless of their identity, background, or geographic location.

As advanced computational techniques and technologies proliferate, healthcare researchers are increasingly leveraging Electronic Health Record (EHR) data to study and optimize patient flow.3 At UCLA Health, clinical research teams are using Artificial Intelligence (AI) to enhance patient care across various settings, with the Office of Health Information & Analytics (OHIA) developing a real-time monitoring dashboard for patient flow.4

Predicting patient flow is a complex challenge that encompasses numerous subcomponents, such as location and department capacity, Cost of Treatment (CoT), patient inflow, and waiting times.5 Unit capacity, which estimates the proportion of beds occupied on a given day, is particularly relevant to UCLA Health. With over 280 clinics alongside its main medical centers and hospitals—including Ronald Reagan Medical Center, Mattel Children’s Hospital, the Stewart and Lynda Resnick Neuropsychiatric Hospital, Santa Monica Medical Center, and West Valley Medical Center—the expansive healthcare ecosystem presents both remarkable opportunities for patient care and challenges related to scale and capacity.6 Different patients are best suited to different locations and services, but imbalances and extreme overcapacity at certain sites raise significant concerns for hospital management and operations. Concentrated patient volumes can burden both patients and clinicians, potentially leading to poorer clinical outcomes. While many capacity modeling efforts have focused on the Emergency Department and critical care settings to manage immediate patient needs and wait times,7 there is substantial potential for similar approaches in the OB setting. Capacity analysis could enable UCLA to allocate hospital resources more effectively, enhance the quality of maternal care during the ante-, intra-, and postpartum periods, and reduce costs for patients.

This research study analyzed key hospital capacity data from the UCLA Deidentified Data Repository (DDR), focusing on the labor and delivery site(s) at UCLA’s Ronald Reagan Medical Center. More than two years of delivery data were extracted for retrospective analysis and predictive modeling. A subset of relevant capacity predictors (e.g. seasonality, staffing availability, and significant events) was evaluated for clinical relevance and predictive power. The study employs commonly used techniques to determine feature importance and provides an analytical overview of factors contributing to capacity.8 It offers insights into which clinical variables merit further investigation within the UCLA OB context and presents a preliminary model for predicting delivery capacity. Ultimately, this research could enhance future operational practices, such as strategically scheduling cesarean sections to avoid peak times and optimizing staff allocation around holidays. By initiating research in this area, we aim to contribute valuable insights that will inform future efforts to predict unit capacity more accurately and guide resource optimization in OB across the UCLA Health system.

## Data Description

### Data Acknowledgment

The data used in this project was provided by UCLA Health DDR (De-identified Data Repository) under strict privacy and security regulations. Due to HIPAA and privacy policies, the raw data and scripts cannot be shared.

The dataset contains anonymized records related to weekly hospital admissions and bed capacity trends for the delivery ward in the OB/GYN department of Ronald Reagan Medical Center. All analyses and findings were conducted in compliance with data protection guidelines to ensure confidentiality.

### Data Structure

Due to privacy constraints, there will not be any photos shown of the data as it appears in Azure Data Studio. However, to describe what it may appear to look like in the software, the DDR comprises of many different sets of tables, usually with the prefix CDM (Clinical Data Management) or DIM (Dimensional Table). The majority of the columns would comprise of an identifier such as the Patient ID or Encounter/Event ID, temporal data including the time a patient was admitted and discharged from the hospital, and unique information pertaining to the subject of that table (e.g. OB/GYN table would contain information about bed logistics such as bed name and group).

### Feature Descriptions

**Independent Variables**
1. Temporal Factors
- Time of day
- Day of week
- Season
- Holiday periods

2. Clinical Characteristics
- Delivery method (vaginal, cesarean)
- Anesthesia requirements
- Multiple births
- Patient obstetric history

3. Operational Factors
- Department occupancy
- Concurrent deliveries
- Staff availability periods

**Dependent Variables**
1. Resource Utilization
- Bed occupancy rates
- Length of stay (hospital discharge time - hospital admission time)
- Room turnover times

2. Capacity Metrics 
- Peak occupancy periods
- Resource bottlenecks
- Department-specific utilization


## Objective

Key Objectives:
1. Perform exploratory data analysis (EDA) to uncover trends and patterns in bed usage within the delivery ward.
   - Our EDA comprises of bar graphs and line plots of bed usage by week and by department, deliveries by hour of the day and day of the week, and weekly bed capacity.
2. Develop a robust time series forecasting model to predict bed capacity needs for the next few months.
  - We would accomplish this by creating an LSTM model trained on 2022-2023 data to predict trends for 2024, and using the actual 2024 data for comparison in order to evaluate its accuracy prior to deployment.
3. Utilize these predictions to support decision-making and improve resource allocation, thereby streamlining patient flow in the delivery ward.

## Methodology

### Data Preprocessing

Data for this study were extracted from the Deidentified Data Repository (DDR) within UCLA Health’s Electronic Health Record (EHR). The data warehouse schema consists of ‘fact’ tables containing transactional data (encounters, deliveries, procedures, etc.) and ‘dim’ tables containing descriptive attributes (departments, dates, categorical lookups, etc.). All protected health information was removed prior to analysis, and the study was conducted in accordance with UCLA Office of Health Information & Analytics (OHIA) data governance policies. The study pool included all deliveries in the Ronald Reagan Medical Center’s primary delivery ward (‘RR 5DR’) from January 1st, 2022 to September 1st, 2024. 


Fig. 1: Data representation in Python

We developed a comprehensive SQL query in Azure Data Studio, integrating multiple EHR tables to create an analytical dataset. Key data elements were sourced from:

- Encounter records (patient visits, admissions)
   - ClinicalDM > rpt > CDM_EncounterFact
   - ClinicalDM > rpt > CDM_EpisodeFact
- Delivery episodes (labor progression, delivery methods)
   - ClinicalDM > rpt > CDM_DeliveryEpisodeFact
- Department and bed utilization records
   - ClinicalDM > rpt > DIM_DepartmentDim
   - ClinicalDM > rpt > DIM_DateDim
- Patient obstetric histories
   - ClinicalDM > rpt > CDM_OBHistoryEpisodeCountsFact
- Temporal attributes (dates, times, seasons)
   - DeliveryDate BETWEEN ‘2022-01-01’ AND ‘2024-09-01’

The resulting dataset included timestamps for patient flow (admission, delivery, discharge), clinical characteristics (delivery method, anesthesia type), facility utilization metrics (bed assignments, department locations), and historical risk factors (previous pregnancies, outcomes).

### Exploratory Data Analysis (EDA)

### Feature Engineering

### Model Development

### Model Evaluation

## Results

### Key Findings

### Performance Metrics

## Conclusion

### Insights

The results demonstrate that the LSTM model, optimized for a 2-week lookback window and trained with carefully tuned hyperparameters, effectively predicts weekly capacity across OBGYN at Ronald Regan. This approach highlights the importance of hyperparameter optimization, including the use of GridSearchCV and systematic evaluation of lookback windows. The achieved performance metrics suggest potential for deploying this model in operational planning and resource allocation within the hospital.

LSTM models have limitations, particularly in the context of healthcare forecasting. They rely on a predefined lookback window, which may miss long-term trends or overfit to noise. Additionally, LSTMs are black-box models, making their predictions hard to interpret, and they adapt poorly to sudden real-time changes like surges in admissions. Addressing these gaps requires incorporating external features like bed utilization and scheduled events, using hybrid models, as well as improving interpretability through tools like SHapley Additive exPlanations (SHAP). 

Additionally, some challenges stem from data availability rather than the model itself. For example, the absence of real-time bed utilization data or explicit inclusion of pre-determined due dates and scheduled C-sections limits the model’s ability to make accurate predictions. Future work may explore integrating these additional features. Specific next steps include:

1. **Incorporating Patients with Predicted Due Dates:** Integrating patient-level data such as predicted due dates will allow the model to anticipate demand more accurately.
2. **Incorporating Scheduled C-Sections:** Adding information about scheduled C-sections will improve the model’s ability to account for known capacity demands.
3. **Larger-Scale Hyperparameter Optimization:** Expanding the range of hyperparameters, such as lookback window size, learning rate, and number of LSTM units, will further refine the model.
4. **Stacking or Ensembling with Regression-Based Models:** Exploring the inclusion of regression-based models in a stacked or ensemble framework could enhance performance, particularly after incorporating additional features.
   
These enhancements aim to improve both the accuracy and utility of the model for real-world application in hospital resource planning.
The overall utility of a capacity model lies in its ability to assist in operational planning for the labor and delivery unit(s). By accurately forecasting weekly capacity, the model can help schedule C-sections more effectively, ensuring that resources and staff are optimally allocated. This proactive planning can enhance patient care and streamline hospital operations, ultimately contributing to more efficient healthcare delivery.

### Other Notes

- **Data Accessibility**: Due to HIPAA and privacy regulations set by UCLA Health, raw data, scripts, and notebooks could not be exported. There are still screenshots of the data structure and code available for observation.

- **Data Complexity**: Due to the intricate relationships between variables and possible confounding factors, models tended to overfit and consequently overperform, leading to inaccurate predictions.

- **Scope Changes**: Due to time constraints and problem restructuring, there were revisions to our project that resulted in some EDA and model building being excluded. Most notably, check out the OBGYN.pdf in the project folder to see insights that were not included in the final report.

## Dependencies

- numpy
- pandas
- matplotlib
- seaborn
- datetime
- statsmodels
- sklearn
- xgboost

## Project Structure
```plaintext
BiodesignDeliveries/
|
├── Delivery.pdf                         # pdf of Python code focused on EDA and model building in the delivery ward
├── LSTM.pdf                             # pdf of Python code focused on LSTM model for weekly bed capacity
├── OBGYN.pdf                            # pdf of Python code focused on EDA and model building for Ronald Reagan's OB/GYN department
├── Patient Flow Plan Presentation.pdf   # pdf of presentation shown In-Person MSDSB/Biodesign Event at UCLA
├──  README.md                           # Project documentation
└── SQL Query for OB Delivery.png        # picture of query for assembling dataset in Azure Data Studio
```
## References

1. Division of Reproductive Health, National Center for Chronic Disease Prevention and Health Promotion, CDC. Achievements in Public Health, 1900-1999: Healthier Mothers and Babies—United States, 1999 October 1 [Internet]. [cited 2024 Dec 2]. Available from: https://www.cdc.gov/mmwr/preview/mmwrhtml/mm4838a2.htm
2. Hwang K, Yee E, Ibarra AB. California’s Maternity Care Crisis is worsening as Newsom decides on bills to slow closures [Internet]. 2024 [cited 2024 Nov 4]. Available from: https://calmatters.org/health/2024/09/new-maternity-care-closures/
3. CADTH. Artificial Intelligence for patient flow. Canadian Journal of Health Technologies. 2024 May 1;4(5). doi:10.51731/cjht.2024.877
4. Real-time dashboards [Internet]. 2024 [cited 2024 Nov 5]. Available from: https://it.uclahealth.org/about/ohia/products/real-time-dashboards
5. Tuominen J, Pulkkinen E, Peltonen J, Kanniainen J, Oksala N, Palomäki A, Roine A. Forecasting emergency department occupancy with advanced machine learning models and multivariable input [Internet]. 2024;40(4):1410-1420. Available from: https://doi.org/10.1016/j.ijforecast.2023.12.002
6. UCLA Health. About UCLA Health [Internet]. [cited 2024 Dec 2]. Available from: https://www.uclahealth.org/discover/about
7. Núñez Reiz A, Armengol de la Hoz MA, Sánchez García M. Big Data Analysis and Machine Learning in Intensive Care Units [Internet]. 2019 Oct [cited 2024 Dec 2];43(7):416-426. Available from: https://www.medintensiva.org/en-big-data-analysis-machine-learning-articulo-S2173572719301420
8. Abuhay TM, Robinson S, Mamuye A, Kovalchuk SV. Machine Learning Integrated Patient Flow Simulation: Why and how? Journal of Simulation. 2023 May 29;17(5):580–93. doi:10.1080/17477778.2023.2217334
9. Stone K, Zwiggelaar R, Jones P, Mac Parthaláin N. A systematic review of the prediction of hospital length of stay: Towards a unified framework. PLOS Digital Health. 2022 Apr 14;1(4). doi:10.1371/journal.pdig.0000017
10. Mahmoudian Y, Nemati A, Safaei AS. A forecasting approach for hospital bed capacity planning using machine learning and Deep Learning with application to public hospitals. Healthcare Analytics. 2023 Dec;4:100245. doi:10.1016/j.health.2023.100245
11. Md Saleh NI, Ab Ghani H, Jilani Z. Defining factors in hospital admissions during COVID-19 using LSTM-FCA explainable model. Artificial Intelligence in Medicine. 2022 Oct;132:102394. doi:10.1016/j.artmed.2022.102394 


## Contact Information

For any questions, please contact:
- Name: Christopher Fu
- Email: christopherfuwas@gmail.com
