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
   - [Model](#model)
      - [Model Methodology](#model-methodology)
      - [Model Development](#model-development)  
      - [Model Evaluation](#model-evaluation)  
7. [Conclusion](#conclusion)  
   - [Insights](#insights)  
   - [Other Notes](#other-notes)
   - [Miscellaneous](#miscellaneous)
8. [Dependencies](#dependencies)  
9. [Project Structure](#project-structure)
10. [References](#references)
11. [Contact Information](#contact-information)  

## Project Overview

This project focuses on analyzing and forecasting bed capacity in the delivery ward of the OB/GYN department at Ronald Reagan Medical Center. By examining data from early 2022 through September 2024, we aim to provide actionable insights to enhance patient flow and optimize operational efficiency at UCLA Health, specifically to our stakeholder Scott Jahnke, Director of Operations.

Key Objectives:
1. Perform exploratory data analysis (EDA) to uncover trends and patterns in bed usage within the delivery ward.
2. Develop a robust time series forecasting model to predict bed capacity needs for the next few months.
3. Utilize these predictions to support decision-making and improve resource allocation, thereby streamlining patient flow in the delivery ward.
  
Scope:
The analysis includes data on admission time, bed logistics, and other relevant factors impacting bed availability. An LSTM model is deployed to forecast delivery volumes, with the training data spanning the years 2022 and 2023; the model is then used to predict delivery outcomes from the beginning of 2024 through September 2024, allowing for a comparison with actual 2024 data.

Impact:
This project serves as a strategic tool for the OB/GYN department and UCLA Health's operations team to:

- Mitigate overcrowding risks, especially in the case of deliveries where the health of the baby and mother are crucial.
- Ensure smoother patient flow, reducing costs and increasing successful deliveries.
- Enhance overall service delivery for expectant mothers.

Results:
The results can be summarized by the following descriptions:

- About 7 deliveries happened per day, with length of stays averaging around 70 days; the delivery ward would have around 24 patients concurrently; labor duration would last on average 20 hours
- Based on EDA, some departments such as 5FDU IOF and Perioperative Area were at maximum bed capacity as opposed to others such as 5EOB and 5DR; resources would need to be allocated more to those departments at maximum capacity
- Bed Usage in 2024 exhibited extreme downward trends as opposed to 2022 and 2023; although these results could be due to data incompleteness, the result could imply other issues occurring at Ronald Reagan Medical Center for this atypical trajectory.
- Deliveries seem to occur more often towards the afternoon and evening hours (i.e. 2pm-8pm) as opposed to midnight and morning hours. More staff would need to be present to account for this spike in deliveries.
- The number of deliveries by day of the week seemed negligible; in other words, the deliveries by one day would be comparable to that of the next or previous day.
- The LSTM model, although capturing the general behavior of actual 2024 data, failed to match the peaks and troughs as reflected by the evaluation metrics of a RMSE of 17.69 and a MAE of 14.91; more hyperparameter tuning would be required to fix the accuracy of the model prior to deployment for predicting 2025 trends.

Recommendations:
Finetuning the LSTM model further is required in order for a proper deployment and application in operational planning and resource allocation within the hospital. Next steps would be the following:

1. Incorporating Patients with Predicted Due Dates
2. Incorporating Scheduled C-Sections
3. Larger-Scale Hyperparameter Optimization
4. Stacking or Ensembling with Regression-Based Models

### Team Acknowledgments

I would like to extend my gratitude to everyone who contributed to this project:

- Courtney Rauchman
- Adi Pillai
- Troy Sisson
- Waleed Khan
- Ronan MacRunnels

## Motivation

Obstetrics (OB) and maternity wards are vital for the health and safety of mothers and their babies. Despite significant advancements in childbirth over the past century, challenges such as complications, errors, and opportunities for improvement persist,<sup>[1]</sup> particularly in protecting patients within the OB department. This is especially critical for populations with historically higher complication rates and strained relationships with the healthcare system. Each statistic represents a mother, a baby, and a family navigating one of the most vulnerable periods of their lives, underscoring the urgency of addressing challenges in OB care. As technology evolves rapidly in this sector, establishing robust infrastructure and conducting rigorous experimentation is essential to ensure these tools effectively benefit OB patients.

Compounding these challenges, California’s labor and delivery departments are closing at a rate exceeding the national average, with 56 closures since 2012 and eight more planned for 2024 alone.<sup>[2]</sup> This trend disproportionately impacts residents in rural areas, patients of color, and those from lower socioeconomic backgrounds. With fewer OB care settings and reduced staffing, it is essential for hospital systems to optimize patient flow within the OB ward, ensuring that the maximum number of patients can receive adequate prenatal and perinatal care, regardless of their identity, background, or geographic location.

As advanced computational techniques and technologies proliferate, healthcare researchers are increasingly leveraging Electronic Health Record (EHR) data to study and optimize patient flow.<sup>[3]</sup> At UCLA Health, clinical research teams are using Artificial Intelligence (AI) to enhance patient care across various settings, with the Office of Health Information & Analytics (OHIA) developing a real-time monitoring dashboard for patient flow.<sup>[4]</sup>

Predicting patient flow is a complex challenge that encompasses numerous subcomponents, such as location and department capacity, Cost of Treatment (CoT), patient inflow, and waiting times.<sup>[5]</sup> Unit capacity, which estimates the proportion of beds occupied on a given day, is particularly relevant to UCLA Health. With over 280 clinics alongside its main medical centers and hospitals—including Ronald Reagan Medical Center, Mattel Children’s Hospital, the Stewart and Lynda Resnick Neuropsychiatric Hospital, Santa Monica Medical Center, and West Valley Medical Center—the expansive healthcare ecosystem presents both remarkable opportunities for patient care and challenges related to scale and capacity.<sup>[6]</sup> Different patients are best suited to different locations and services, but imbalances and extreme overcapacity at certain sites raise significant concerns for hospital management and operations. Concentrated patient volumes can burden both patients and clinicians, potentially leading to poorer clinical outcomes. While many capacity modeling efforts have focused on the Emergency Department and critical care settings to manage immediate patient needs and wait times,<sup>[7]</sup> there is substantial potential for similar approaches in the OB setting. Capacity analysis could enable UCLA to allocate hospital resources more effectively, enhance the quality of maternal care during the ante-, intra-, and postpartum periods, and reduce costs for patients.

This research study analyzed key hospital capacity data from the UCLA Deidentified Data Repository (DDR), focusing on the labor and delivery site(s) at UCLA’s Ronald Reagan Medical Center. More than two years of delivery data were extracted for retrospective analysis and predictive modeling. A subset of relevant capacity predictors (e.g. seasonality, staffing availability, and significant events) was evaluated for clinical relevance and predictive power. The study employs commonly used techniques to determine feature importance and provides an analytical overview of factors contributing to capacity.<sup>[8]</sup> It offers insights into which clinical variables merit further investigation within the UCLA OB context and presents a preliminary model for predicting delivery capacity. Ultimately, this research could enhance future operational practices, such as strategically scheduling cesarean sections to avoid peak times and optimizing staff allocation around holidays. By initiating research in this area, we aim to contribute valuable insights that will inform future efforts to predict unit capacity more accurately and guide resource optimization in OB across the UCLA Health system.

## Data Description

### Data Acknowledgment

The data used in this project was provided by UCLA Health DDR (De-identified Data Repository) under strict privacy and security regulations. Due to HIPAA and privacy policies, the raw data and scripts cannot be shared.

The dataset contains anonymized records related to weekly hospital admissions and bed capacity trends for the delivery ward in the OB/GYN department of Ronald Reagan Medical Center. All analyses and findings were conducted in compliance with data protection guidelines to ensure confidentiality.

### Data Structure

Due to privacy constraints, there will not be any photos shown of the data as it appears in Azure Data Studio. However, to describe what it may appear to look like in the software, the DDR comprises of many different sets of tables, usually with the prefix CDM (Clinical Data Management) or DIM (Dimensional Table). The majority of the columns would comprise of an identifier such as the Patient ID or Encounter/Event ID, temporal data including the time a patient was admitted and discharged from the hospital, and unique information pertaining to the subject of that table (e.g. OB/GYN table would contain information about bed logistics such as bed name and group).

If you are curious to see what the SQL query looks like when querying the dataset for analysis, see the image of the [SQL code here](code/SQL%20Query%20for%20OB%20Delivery.png).

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

![Fig.1](images/Dataset.png)

**Fig 1: Data Representation in Jupyter Notebook**

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

There were 7,386 deliveries in our study cohort across five departments in the Ronald Reagan Medical Center (‘RR 5DR’, ‘RR PERIOPERATIVE AREA’, ‘RR 5FDU IOF’, ‘RR 5EOB’, ‘RR 7ICU’). Table 2 illustrates descriptive statistics for four key capacity metrics: delivery volume, length of stay, occupancy, and labor duration. 

![Table 1](images/Descriptive%20Statistics.png)

**Table 1: Descriptive Statistics**

![Fig.2](images/Avg%20Bed%20Usage%20by%20Dept.png)

**Fig 2: Average Bed Usage by Department within Ronald Reagan Obstetrics.** Figure was created by running summary statistics, specifically the mean of ‘BedInCensus’ grouped by department. Departments RR 5FDU and RR PERIOPERATIVE AREA were at maximum average bed usage, whereas departments RR 5EOB and RR 5DR were between 80% and 100% average bed usage. Knowing which departments are at full capacity can provide insight for operations to redirect resources (i.e. staff, beds) to accommodate high usage in those areas.

![Fig.3](images/Weekly%20Bed%20Usage%20Line%20Plot.png)

**Fig 3: Weekly Trends in Bed Usage in Ronald Reagan Obstetrics Department.** Total bed usage across all years observed hundreds of beds being used each week. A smaller data frame was created by taking the sum of ‘BedInCensus’, grouped by each year and week. Year 2024 observed a downward trend compared to other years, with bed usage peaking around weeks 8-10; Years 2022 and 2024 had fluctuating trends, peaking around weeks 45-50. The atypical trend in 2024 could be attributed to recent data lag, unless this trend accurately reflects activity for that specific year, which would require further investigation into other factors contributing to unusual patterns in bed usage.


![Fig.4](images/Deliveries%20Hour%20of%20Day.png)

**Fig 4: Hourly Number of Deliveries in Ronald Reagan Obstetrics Department.** The total number of deliveries during a given hour across 2022-2024 data. Hours 15-20 have a higher than average total delivery count. The hourly delivery distribution appears left skewed, with most deliveries occurring in the evening compared to midnight and morning hours.

![Fig.5](images/Deliveries%20Day%20of%20Week.png)

**Fig 5: Average Deliveries per Day in Ronald Reagan Delivery Units.** The average number of deliveries per day of the week across the period of study. The majority of deliveries (93%) occurred in ‘RR 5DR.’ Average deliveries peaked for Tuesday, Wednesday, and Saturday, as well as dipped the lowest on Friday, although average deliveries across the week seem relatively comparable.

![Fig.6](images/Weekly%20OB%20Capacity.png)

**Fig.6: Weekly capacity readout of Ronald Reagan Obstetrics Department.** Time period for analysis is defined from January 3, 2022 to September 1, 2024. Capacity of the obstetrics unit is defined as the median number of patients occupying beds per day for a given week. Weekly capacity ranges from 1-90 patients per day.

The dataset used for modeling spanned a period of approximately 2 years, with a test period of 9 months. During EDA, trends and seasonality in patient admissions were observed, highlighting the suitability of an LSTM model for capturing temporal dependencies and patterns. The capacity data were normalized to a range of [0, 1] to enhance model performance and ensure stable training.

For additional EDA findings, refer to the jupyter notebooks in the github repository:
- [Delivery](code/Delivery.pdf)
- [LSTM](code/LSTM.pdf)
- [OB/GYN](code/OBGYN.pdf)

### Model 

#### Model Methodology

For the purpose of this study, weekly capacity was operationalized as the median number of patients
admitted per day within a week. This metric was chosen due to its resilience against the influence of
extreme values, which are common in healthcare datasets.

The forecasting model used in this study was a Long Short-Term Memory (LSTM) network, a type of
recurrent neural network (RNN) designed to capture sequential dependencies and temporal patterns in
time-series data. LSTMs have demonstrated exceptional performance in applications requiring the
modeling of long-term dependencies, such as hospital bed capacity forecasting and patient admission
trends, as shown in recent works.<sup>[10,11]</sup>

LSTMs use a gating mechanism to regulate the flow of information through the network, addressing the
vanishing gradient problem typically encountered in traditional RNNs. This architecture allows the
network to effectively learn from long sequences, making it particularly well-suited for healthcare
forecasting tasks where historical data significantly influences future outcomes.

In this study, the LSTM model was configured to use weekly patient admissions as input, with a lookback
window of two weeks. The lookback window size was determined through experimentation, as detailed in
the Results section. The input sequences were fed into the LSTM layers, which extracted temporal
features and passed them to fully connected layers for capacity prediction. Hyperparameters, including the
number of epochs, batch size, and learning rate, were optimized using GridSearchCV to minimize
validation error.

The model was trained on two years of data and tested on a nine-month holdout set. The model utilized
data only up to September 2024 due to concerns about data completeness and reporting lag, ensuring the
reliability of performance metrics. Model performance was evaluated using Root Mean Squared Error
(RMSE) and Mean Absolute Error (MAE). These metrics provided insight into the magnitude and
consistency of prediction errors, reflecting the model’s suitability for deployment.

#### Model Development

A key hyperparameter in the model was the lookback window, representing the number of prior weeks
used as input for predicting future capacity. A range of 1 to 10 weeks was evaluated, with a lookback
window of 2 weeks yielding optimal performance. This was determined by comparing validation errors
during initial experiments, confirming that a shorter lookback window effectively captured relevant
temporal dependencies without overfitting.

The LSTM network was designed to process sequential input data with two layers of LSTM units
followed by fully connected layers. The model was trained using the Adam optimizer, with the mean
absolute error (MAE) as the loss function. Hyperparameter optimization was conducted for the number of
epochs and batch size using a grid search approach:

- **Epochs:** Evaluated within the range of 10 to 100.
- **Batch Size:** Evaluated within the range of 8 to 64.
- **Learning rate:** Evaluated within the range of 0.001 to 0.01.

GridSearchCV was implemented to identify the combination of these parameters that minimized
validation loss. The optimal configuration was determined to be a learning rate of 0.005, 50 epochs, and a
batch size of 8.

#### Model Evaluation

The final LSTM model was assessed using Root Mean Squared Error (RMSE) and Mean Absolute Error
(MAE) as metrics:

- **RMSE: 17.69**
- **MAE: 14.91**

![Fig.7](images/LSTM.png)
**Fig 7: Actual vs. predicted weekly capacity of Ronald Reagan Obstetrics Department using
Long-Short Term Memory model.** The training set is defined by the period from January 3, 2022 to
December 31, 2023, and the testing set is defined by the period between January 1 to August 31 of 2024.

The Root Mean Squared Error (RMSE) for the model performance on the test set is 17.69, and the Mean
Absolute Error is 14.71. These results indicate that the model captures the variability in weekly capacity.
The RMSE value reflects the magnitude of prediction errors, while the MAE provides a straightforward
measure of average error. Both metrics underscore the model’s effectiveness in forecasting capacity trends
within the OBGYN ward.

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

### Miscellaneous 

If you are curious to view my team's report in a more visual and succint manner, check out the [presentation](Patient%20Flow%20Plan%20Presentation.pdf) we delivered together at the In-Person MSDSB/Biodesign Event at UCLA!

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
├── code/                                # Folder containing all code related files
|   ├── Delivery.pdf                     # PDF of Python code focused on EDA and model building in the delivery ward
|   ├── LSTM.pdf                         # PDF of Python code focused on LSTM model for weekly bed capacity
|   ├── OBGYN.pdf                        # PDF of Python code focused on EDA and model building for Ronald Reagan's OB/GYN department
|   └── SQL Query for OB Delivery.png    # Picture of query for assembling dataset in Azure Data Studio
|
├── images/                              # Folder containing all relevant images of EDA and model results
|   ├── Avg Bed Usage by Dept.png        # Bar Plot of Average Bed Usage by Department of Ronald Reagan Medical Center for OB/GYN Delivieres
|   ├── Dataset.png                      # Jupyter Notebook Python Pandas representation of the dataset
|   ├── Deliveries Day of Week.png       # Deliveries separated by the day of the week
|   ├── Deliveries Hour of Day.png       # Deliveries sorted by hour of the day
|   ├── Descriptive Statistics.png       # Descriptive Statistics of the variables of interest for OB/GYN Deliveries
|   ├── LSTM.png                         # Line plots of Weekly Capacity, the orange line being the LSTM predicted trend and the blue being actual trends
|   ├── Weekly Bed Usage Line Plot.png   # Line plot of the weekly bed usage separated by year (2022-2024)
|   └── Weekly OB Capacity.png           # Line plot of the weekly bed capacity, with tick marks denoting the week of the year
|
├── LICENSE                              # License information
├── Patient Flow Plan Presentation.pdf   # PDF of presentation shown In-Person MSDSB/Biodesign Event at UCLA
└── README.md                            # Project documentation
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
