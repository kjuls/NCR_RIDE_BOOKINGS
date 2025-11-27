# NCR_RIDE_BOOKINGS
This project presents a professional data analysis of an Uber ride dataset. The analysis aims to uncover insights into booking patterns, cancellations, operational efficiency, and user satisfaction. Python and data visualization libraries were used to clean, process, and interpret the data.

#### Table of content 
- Project Overview 
- Project Scope 
- Project Objectives 
- Dataset Description
- Document Purpose  
- [Data Source](https://www.kaggle.com/datasets/yashdevladdha/uber-ride-analytics-dashboard)
- Data Cleaning and Processing 
- Data Analysis and Insights
- Recommendation 
- Conclusion

### Project Overview 
This comprehensive dataset contains detailed ride-sharing data from Uber operations for the year 2024, providing rich insights into booking patterns, vehicle performance, revenue streams, cancellation behaviors, and customer satisfaction metrics.

### Project Scope 
The scope of this project covers the exploration, analysis, and interpretation of uber ride data to identify operational patterns, customer behavior, and business performance metrics. It includes:
- Data Cleaning & Preparation
- Data Analysis
- Behavioral Insights
- Operational Insights
- Revenue & Demand Patterns
- Recommendations

### Project Objectives 
The analysis seeks to:
- Understand ride demand patterns: Identify peak booking hours, popular locations, and trends by day of the week.
- Investigate cancellation behavior: Determine the frequency and causes of cancellations by drivers and customers.
- Assess operational performance: Evaluate Vehicle Turnaround Time (VTAT) and Customer Turnaround Time (CTAT) across different vehicle types. 
- Analyze revenue and ride metrics: Explore relationships between booking value, ride distance, and vehicle type.
- Examine user satisfaction: Compare driver and customer ratings to assess service quality.
- Study payment preferences: Identify which payment methods are most commonly used. 

### Dataset Description 
The dataset consists of various columns including:
- Date, Time, Booking ID, Customer ID
- Vehicle Type, Booking Status, Pickup and Drop Locations
- Average VTAT (Vehicle Turnaround Time), Average CTAT (Customer Turnaround Time)
- Cancellation details (Customer and Driver)
- Booking Value, Ride Distance, Ratings, and Payment Method

These attributes provide a broad overview of Uber ride activities, operational metrics, and customer behavior.

### Document Purpose 
This document serves as a comprehensive reference for stakeholders, providing:
- A clear understanding of the project objectives and scope.
- An overview of the dataset and its structure.
- A detailed account of data cleaning and processing techniques.
- Insights derived from the analysis and their business implications.

### Data source
The dataset for this project is sourced from [Kaggle website](https://www.kaggle.com/datasets/yashdevladdha/uber-ride-analytics-dashboard) and includes information on 150,000 total bookings across multiple vehicle types and provides a complete view of ride-sharing operations including successful rides, cancellations, customer behaviors, and financial metrics.

### Data Cleaning and Preparation
The preprocessing phase focused on ensuring data consistency and reliability.
Key steps included:
- Addressing Duplicate Records: There were no duplicates in the Dataset.
- Count the number of null values in each column.
- Converting date and time columns into datetime format.
- Extracting additional temporal features such as 'hour' and 'day_of_week'.
- Handling missing or invalid values appropriately.
- Ensuring numerical data (e.g., booking value, distance) were in the correct format.

**1.	Importing Required libraries** 

import pandas as pd

import matplotlib.pyplot as plt

import seaborn as sns

**2.	Loading the Dataset**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Loading%20The%20Dataset.png)

**3.	Are there any Null Values? How will it be handled?**

Null values refer to missing or undefined entries in a dataset. They can occur due to incomplete data collection, system errors, or manual omissions. Identifying null values is crucial as they can lead to inaccuracies in analysis or misinterpretation of results. Outliers are extreme data points that deviate significantly from the majority of observations. They can arise from errors, rare events, or natural variability within the data. These values have the potential to skew statistical measures and distort the overall analysis if not properly addressed.

Both null values and outliers are critical considerations during the data cleaning process. Their presence affects the dataset's quality and reliability, making their detection and evaluation a vital step in preparing data for meaningful analysis.

In order to carry out the above, this was done;

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/DF.Info.png)

df.info() displays the structure of the Data Frame, including column names, data types, and counts of non-null values. 

- To count the number of missing values of each column in the dataset.

The code: df.isnull().sum() was applied 

**Result:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Null%20Columns.png)

From the above 

The missing values are logical, not errors. They just represent non-applicable scenarios e.g.; a complete ride won’t have a cancellation reason.

- **Filling Missing Values for a Clean Reporting** 

**i.	For numeric columns:** VTAT, CTAT, Booking Value, Distance, Ratings, should be replaced with mean, median, or 0 if it means “Not Applicable”.

**CODE:**

df['Avg VTAT'].fillna(df['Avg VTAT'].median(), inplace=True)

df['Avg CTAT'].fillna(df['Avg CTAT'].median(), inplace=True)

df['Booking Value'].fillna(0, inplace=True)

df['Ride Distance'].fillna(0, inplace=True)

df['Driver Ratings'].fillna(df['Driver Ratings'].mean(), inplace=True)

df['Customer Rating'].fillna(df['Customer Rating'].mean(), inplace=True)

**ii.	For categorical columns:** Replace missing values with placeholder like “Not Applicable” and “No Payment”.

**CODE:** 

df['Reason for cancelling by Customer'].fillna('Not Applicable', inplace=True)

df['Driver Cancellation Reason'].fillna('Not Applicable', inplace=True)

df['Incomplete Rides Reason'].fillna('Not Applicable', inplace=True)

df['Payment Method'].fillna('No Payment (Cancelled)', inplace=True)

df['Cancelled Rides by Customer'].fillna('Not Applicable', inplace=True)

df['Cancelled Rides by Driver'].fillna('Not Applicable', inplace=True)

df['Incomplete Rides'].fillna('Not Applicable', inplace=True)

**RESULT:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Updated%20Null%20Columns.png)

From the table above the dataset contain columns with no missing values.

**4.	Convert the Date column**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Convert%20the%20date%20column.png)

- Changes the ‘date’ column from a string (text) into a proper datetime object.
- This makes it easier to sort by date, filter by date ranges, or extract parts like month, day, or year.

**5.	Combine Date and Time into Datetime column**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Combine%20date%20and%20time.png)

- Merges the two columns (date and time) into one full timestamp column called ‘datetime’.
- This is crucial for time-based analysis (hourly, daily, weekly patterns).
- astype(str) converts them to strings temporarily for safe concatenation, then pd.to_datetime() converts them back to timestamps.

**6.	Extract Hour and Day of the week**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Extract%20hour.png)

- dt.hour creates a new column ‘hour’ that stores just the hour of the day (0-23).
- It is super helpful for analyzing peak booking hours.
- dt.day_name creates extract and create another new column that contains the name of the weekday (e.g., Monday, Tuesday,).
- It is useful for analyzing daily trends (weekend vs weekday demand).

### Data Analysis and Insights 

**1.	Demand analysis**

Bookings by the day

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Demand%20Analysis.png)

**Result**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Demand%20Analysis%20Plot.png)

The “Bookings by Hour of Day” analysis shows a distinct bi-modal trend in ride demand. Bookings peak during morning (8–10 AM) and evening (5–8 PM) hours, aligning with typical commuter behavior. Demand remains low during late-night and midday periods. Uber can improve service efficiency by allocating more drivers during peak hours and introducing off-peak promotions to encourage balanced utilization throughout the day.

**2.	Cancellation Analysis** 

This report analyzes booking outcomes from the dataset, focusing on understanding who cancels more (drivers or customers), and identifying operational issues that contribute to unsuccessful bookings. The analysis uses value counts of booking status and a bar chart illustrating the distribution of booking outcomes.

- I.	What is the overall cancellation rate?
