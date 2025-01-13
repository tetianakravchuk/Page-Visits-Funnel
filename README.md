README: Customer Journey Analysis with Pandas

Description

This project demonstrates how to analyze the customer journey for an e-commerce platform using Pandas. By merging multiple datasets, we gain insights into user behavior across different stages: visiting, adding to cart, checking out, and making a purchase. The analysis identifies bottlenecks in the conversion funnel and calculates metrics such as the percentage of users who dropped out at each stage and the average time to purchase.

Features

	1.	Data Loading and Exploration
	•	Load CSV files for each stage of the customer journey (visits, cart, checkout, purchase).
	•	Parse timestamps into datetime objects for time-based analysis.
	2.	Data Merging
	•	Merge datasets step-by-step to create a complete view of the customer journey.
	3.	Funnel Analysis
	•	Calculate the percentage of users who visit but do not add items to their cart.
	•	Determine the percentage of users who proceed to checkout but do not complete a purchase.
	4.	Time-to-Purchase Analysis
	•	Calculate the time taken for users to complete a purchase from their initial visit.
	•	Compute the average time-to-purchase.

Dataset

The following datasets (CSV files) are used:
	1.	visits.csv: User visits to the website with timestamps.
	2.	cart.csv: Users adding items to their cart with timestamps.
	3.	checkout.csv: Users proceeding to checkout with timestamps.
	4.	purchase.csv: Users completing their purchase with timestamps.

Code Examples

Load Data

import pandas as pd

visits = pd.read_csv('visits.csv', parse_dates=[1])
cart = pd.read_csv('cart.csv', parse_dates=[1])
checkout = pd.read_csv('checkout.csv', parse_dates=[1])
purchase = pd.read_csv('purchase.csv', parse_dates=[1])

print(visits.head())

Merge Datasets

# Merge visits and cart
visits_cart = pd.merge(visits, cart, how='left', on='user_id')

# Merge the result with checkout
visits_cart_checkout = pd.merge(visits_cart, checkout, how='left', on='user_id')

# Merge the result with purchase
all_data = pd.merge(visits_cart_checkout, purchase, how='left', on='user_id')

print(all_data.head())

Funnel Analysis

# Users who visited but did not add to cart
percent_no_cart = (len(all_data[all_data['cart_time'].isnull()]) / float(len(visits))) * 100
print(f"Percentage of users who did not add to cart: {percent_no_cart:.2f}%")

# Users who proceeded to checkout but did not purchase
no_purchase = all_data[(all_data['checkout_time'].notnull()) & (all_data['purchase_time'].isnull())]
percent_no_purchase = (len(no_purchase) / float(len(all_data[all_data['checkout_time'].notnull()]))) * 100
print(f"Percentage of users who did not purchase after checkout: {percent_no_purchase:.2f}%")

Time-to-Purchase Analysis

all_data['time_to_purchase'] = all_data['purchase_time'] - all_data['visit_time']
average_time_to_purchase = all_data['time_to_purchase'].mean()
print(f"Average time to purchase: {average_time_to_purchase}")

Key Results

	•	Percentage of users who visited but did not add to cart.
	•	Percentage of users who proceeded to checkout but did not purchase.
	•	Average time taken to complete a purchase.

Requirements

	•	Python 3.x
	•	Pandas library

To install Pandas, run:

pip install pandas

How to Use

	1.	Clone the repository:

git clone https://github.com/yourusername/customer-journey-analysis.git


	2.	Navigate to the project folder:

cd customer-journey-analysis


	3.	Place the required CSV files (visits.csv, cart.csv, checkout.csv, purchase.csv) in the project directory.
	4.	Run the script:

python analysis.py



Insights

This analysis provides valuable insights into the customer journey, helping e-commerce businesses identify and resolve bottlenecks in their sales funnel.
