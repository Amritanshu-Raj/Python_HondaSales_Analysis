# Honda Vehicle Sales Analysis and Customer Segmentation
## Overview
This project analyzes Honda vehicle sales data to perform customer segmentation and extract key business insights for strategic decision-making. 
The analysis includes data cleaning, exploratory data analysis, statistical calculations, and visualization of sales patterns across different dimensions.
![Sales Analysis Dashboard](images/sales_dashboard.png) *<!-- Note: This image is just a placeholder; in actual project, you would have a dashboard image -->*
## Key Insights
### Top Performances
- **Top Selling Location**: Kankarbagh (89 customers)
- **Best Staff Performer**: Neha (188 vehicles sold)
- **Most Popular Vehicle**: Honda Activa
- **Highest Average Sales**: Vikash (₹107,213 per vehicle)
### Financial Metrics
| Metric | Value |
|--------|-------|
| Total Sales | ₹105,031,079 |
| Average Vehicle Cost | ₹105,031 |
| Cost Standard Deviation | ₹26,009 |
### Customer Analysis
**Top 5 Customers by Spending:**
1. Rohit Singh - ₹2,084,245
2. Suman Choudhary - ₹1,946,917
3. Pooja Kumar - ₹1,864,919
4. Ravi Kumar - ₹1,801,954
5. Suresh Sharma - ₹1,750,672
## Code Implementation
### Data Loading and Cleaning
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
# Load and clean data
df = pd.read_csv('data/Data.csv')
df.rename(columns={
    "Sn.o": "S_No.",
    "Vachile Name": "Vehicle_Name",
    "Vachile Model": "Vehicle_Model"
}, inplace=True)
# Convert to datetime
df["Date"] = pd.to_datetime(df["Date"], errors="coerce")
df["Purchase Date"] = pd.to_datetime(df["Purchase Date"], errors="coerce")
# Validate cost data
df = df[df["Cost"] > 0]
```
### Key Metrics Calculation
```python
# Financial metrics
total_sales = df["Cost"].sum()
avg_cost = df["Cost"].mean()
cost_std = np.std(df["Cost"].values)
# Top performers
top_location = df.groupby("Location")["Customer Name"].nunique().sort_values(ascending=False).head(1)
top_staff = df.groupby('Staff Name')['Vehicle_Name'].count().sort_values(ascending=False).head(1)
```
### Data Visualization
```python
# Vehicle sales by location heatmap
plt.figure(figsize=(12, 8))
vehicle_location = pd.crosstab(df["Vehicle_Name"], df["Location"])
sns.heatmap(vehicle_location, annot=True, fmt="d", cmap="Blues")
plt.title("Vehicle Sales by Location")
plt.savefig('images/vehicle_location_heatmap.png')
```
### Sales Trend Analysis
```python
# Monthly sales trends for top 3 vehicles
top_vehicles = df['Vehicle_Name'].value_counts().head(3).index
top_data = df[df['Vehicle_Name'].isin(top_vehicles)].copy()
top_data['Month'] = top_data['Purchase Date'].dt.strftime('%Y-%m')
monthly_sales = top_data.groupby(['Month', 'Vehicle_Name']).size().unstack()
monthly_sales.plot(kind='bar', figsize=(12,6))
plt.title('Monthly Sales for Top 3 Vehicles')
plt.savefig('images/monthly_sales_trend.png')
```
## Visualizations
It is inside the Jupyter_Notebook file. 
https://github.com/Amritanshu-Raj/Python_HondaSales_Analysis/blob/main/Honda_Sales_Analysis.ipynb
