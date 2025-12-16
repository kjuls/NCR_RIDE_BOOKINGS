# NCR_RIDE_BOOKINGS
This project presents a professional data analysis of an Uber ride dataset. The analysis aims to uncover insights into booking patterns, cancellations, operational efficiency, and user satisfaction. Python and data visualization libraries were used to clean, process, and interpret the data.

#### Table of content 
- Project Overview 
- Project Scope 
- Project Objectives 
- Dataset Description
- Document Purpose  
- Data Source
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

`import pandas as pd`

`import matplotlib.pyplot as plt`

`import seaborn as sns`

**2.	Loading the Dataset**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Loading%20The%20Dataset.png)

```df = pd.read_excel('/content/drive/MyDrive/My Assessment/ncr_ride_booking.csv.xlsx')```

After saving the data file in the variable as `df`, the dataset is called out using only the variable.

`df`

**3.	Are there any Null Values? How will it be handled?**

Null values refer to missing or undefined entries in a dataset. They can occur due to incomplete data collection, system errors, or manual omissions. Identifying null values is crucial as they can lead to inaccuracies in analysis or misinterpretation of results. Outliers are extreme data points that deviate significantly from the majority of observations. They can arise from errors, rare events, or natural variability within the data. These values have the potential to skew statistical measures and distort the overall analysis if not properly addressed.

Both null values and outliers are critical considerations during the data cleaning process. Their presence affects the dataset's quality and reliability, making their detection and evaluation a vital step in preparing data for meaningful analysis.

In order to carry out the above, this was done;

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/DF.Info.png)

`df.info()` displays the structure of the Data Frame, including column names, data types, and counts of non-null values. 

- To count the number of missing values of each column in the dataset.

The code: df.isnull().sum() was applied 

**Result:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Null%20Columns.png)

From the above 

The missing values are logical, not errors. They just represent non-applicable scenarios e.g.; a complete ride won’t have a cancellation reason.

- **Filling Missing Values for a Clean Reporting** 

**i.	For numeric columns:** VTAT, CTAT, Booking Value, Distance, Ratings, should be replaced with mean, median, or 0 if it means “Not Applicable”.

**CODE:**

`df['Avg VTAT'].fillna(df['Avg VTAT'].median(), inplace=True)`

`df['Avg CTAT'].fillna(df['Avg CTAT'].median(), inplace=True)`

`df['Booking Value'].fillna(0, inplace=True)`

`df['Ride Distance'].fillna(0, inplace=True)`

`df['Driver Ratings'].fillna(df['Driver Ratings'].mean(), inplace=True)`

`df['Customer Rating'].fillna(df['Customer Rating'].mean(), inplace=True)`

**ii.	For categorical columns:** Replace missing values with placeholder like “Not Applicable” and “No Payment”.

**CODE:** 

`df['Reason for cancelling by Customer'].fillna('Not Applicable', inplace=True)`

`df['Driver Cancellation Reason'].fillna('Not Applicable', inplace=True)`

`df['Incomplete Rides Reason'].fillna('Not Applicable', inplace=True)`

`df['Payment Method'].fillna('No Payment (Cancelled)', inplace=True)`

`df['Cancelled Rides by Customer'].fillna('Not Applicable', inplace=True)`

`df['Cancelled Rides by Driver'].fillna('Not Applicable', inplace=True)`

`df['Incomplete Rides'].fillna('Not Applicable', inplace=True)`

**RESULT:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Updated%20Null%20Columns.png)

From the table above the dataset contain columns with no missing values.

**4.	Convert the Date column**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Convert%20the%20date%20column.png)

`df['Date'] = pd.to_datetime(df['Date'])`

- Changes the ‘date’ column from a string (text) into a proper datetime object.
- This makes it easier to sort by date, filter by date ranges, or extract parts like month, day, or year.

**5.	Combine Date and Time into Datetime column**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Combine%20date%20and%20time.png)

`df['Datetime'] = pd.to_datetime(df['Date'].astype(str) + " " + df['Time'].astype(str))`

- Merges the two columns (date and time) into one full timestamp column called ‘datetime’.
- This is crucial for time-based analysis (hourly, daily, weekly patterns).
- astype(str) converts them to strings temporarily for safe concatenation, then pd.to_datetime() converts them back to timestamps.

**6.	Extract Hour and Day of the week**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Extract%20hour.png)

`df['Hour'] = df['Datetime'].dt.hour`

`df['Day_of_week'] = df['Datetime'].dt.day_name()`

`df`

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

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Overall%20cancellation%20rate.png)

The overall cancellation rate is 25%. 

- II.	Who cancels more? Driver vs Customer

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Who%20cancels%20more.png)

The data shows that while a high number of bookings are successfully completed, there is a significant volume of cancellations driven predominantly by supply-side (driver-related) issues.

- III.	Cancellations overview

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Cancellations%20Overview.png)

**Result:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Cancellation%20Plot.png)

With 93,000 completed trips, the platform demonstrates strong operational efficiency for the majority of requests. This indicates healthy engagement between customers and drivers and a functioning matching ecosystem.

However, the remaining unsuccessful booking outcomes need further attention.

**3.	Average VTAT & CTAT by Vehicle Type**

VTAT (Vehicle Time to Arrive at Trip) - How long it takes a driver to reach the customer after accepting a trip.

CTAT (Customer Time After Trip) - Total time the customer remains engaged during the trip (likely including travel time + waiting).

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Operational%20Metrics.png)

**Result:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Operational%20Metrics%20Plot.png)

- **VTAT is stable and low:** Strong pickup efficiency across categories.
- **CTAT is high but uniform:** Trip duration is more influenced by traffic/trip length than vehicle type.
- **No vehicle type underperforms:** Fleet management and matching are consistent.

**4.	Revenue & Distance Analysis**

- **Revenue (Total & Average)**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Revenue%20%26%20Distance.png)

The above indicates a high-volume, mid-value ride ecosystem, meaning the platform earns well through a large number of moderately priced rides rather than high-value rides.

- **Revenue by Vehicle Type**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Revenue%20by%20vehicle%20type.png)

**Result:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Revenue%20by%20vehicle%20type%20plot.png)

Insights 
1.	Auto Generates the Highest Revenue

2.	Go Mini and Go Sedan Perform Strongly

3.	Uber XL Generates the Least Revenue
- XL rides are expensive but rarely booked.
- Low demand → low total revenue.
- Indicates a niche use case (group travel, airport trips).

4.	Bike and eBike in Mid-Low Revenue Zone

Bikes generate decent revenue because they are:
- Cheap.
- Fast in traffic.
- Ideal for short trips.

eBikes generate even less revenue due to:
- Limited fleet.
- Limited distance capability.
- Lower customer adoption.

- **Distance vs value**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Distance%20vs%20value.png)

**Result:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Distance%20vs%20value%20plot.png)

The plot above shows that a positive relationship exist. As distance increases booking value generally increases. The plot shows that there’s a high variability in Booking Value.

The spread remains noisy, meaning pricing is not purely distance-driven but uses multiple dynamic factors. 

**5.	Ratings Overview**

- **Average ratings**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Average%20ratings.png)

The above shows that:
- Customers are happier with drivers than drivers are with customers.
- Drivers are stricter and more critical in rating.
- Customer behavior may be more problematic or inconsistent.
- Driver performance is stable and acceptable but not exceptional.

- **Ratings distribution**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Rating%20distribution.png)

**Result:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Rating%20distribution%20plot.png)

Driver Ratings cluster around 4.2 – 4.3 show that drivers are performing at a good but not perfect level, indicating: reliable service quality, and some dissatisfaction factors are recurring (delays, behavior, route choices, vehicle condition, etc.).  

Customer Rating cluster higher around 4.4 – 4.5 shows that customer tend to rate drivers more generously than drivers rate customers.

The histogram shows a small but noticeable clusters of 3.0 – 4.0 ratings for both groups. These represent dissatisfaction cases which implies red flags for operational issues affecting experience.

**6.	Payment Method Analysis**

- **Payment method counts**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Payment%20method%20counts.png)

- **Payment method preference plot**

`plt.figure(figsize=(7,7))`

`plt.pie(payment_counts.values, labels=payment_counts.index, autopct='%1.1f%%', startangle=90, colors=plt.cm.Paired.colors, wedgeprops={'edgecolor':'white', 'linewidth': 1})`

Add a title

`plt.title('Distribution of Payment Methods', fontsize=14, fontweight='bold')`

Show the pie chart

`plt.show()`

**Result:**

![](https://github.com/kjuls/NCR_RIDE_BOOKINGS/blob/main/Payment%20prefrence%20visual.png)

The analysis above shows:
- One-third of rides result in no completed payment, indicating extremely high cancellation behavior. 
- UPI is becoming the platform’s strongest channel, enabling fast and frictionless completion.
- Cash dependency remains moderate but contributes to payment disputes, higher driver cancellations, and operational delays.
- Wallet + Card payment are underutilized (combine < 20%), missing an opportunity to lock in customer loyalty.

### Recommendation 
Based on the provided analysis, here are several recommendations for Uber Data Analysis:
- Optimize driver allocation during peak hours to reduce cancellations.
- Improve matching algorithm.
- Implement driver incentive programs tied to performance ratings.
- Conduct system log investigation into: API failures, driver app version fragmentation, payment errors, GPS dropout due to old devices.
- Investigate vehicle types with longer VTAT/CTAT to improve efficiency.
- Improve CTAT by addressing traffic & routing.
- Analyze CTAT by time of day and area.
- Consider premium pricing for faster pickup (VTAT).
- Expand bike & ebike fleet in congested zones.
- Address low-rating root causes.
- Educate customers on Etiquette.
- Improve pickup and communication experience.
- Introduce real-time rating alerts.
- Encourage digital payment adoption to improve transaction efficiency.
- Monitor cancellation reasons regularly to improve both driver and customer experience.
- Boost UPI adoption even further (Allow autopay for UPI)
- Prevent cancellation at payment stage.

### Conclusion 
This Uber dataset analysis provides actionable insights into the operational and behavioral patterns of the ride-hailing ecosystem.

By understanding booking trends, cancellations, and performance metrics, NCR Ride can implement data-driven strategies to improve efficiency, customer satisfaction, and revenue growth.

Thank you for reading. I’m interested in Analytics role in an organization where I can showcase my skills, take more responsibilities, continue to learn, an organization that I can grow with, where my work will be highly beneficial to the organization.

You can reach me on juliusokolawole@gmail.com
