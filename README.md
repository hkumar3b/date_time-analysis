# Date and Time Analysis with Pandas

This project demonstrates fundamental date and time manipulation techniques using the `pandas` and `numpy` libraries in Python. It focuses on extracting various time-based features from datetime columns and performing simple calculations.

---

## Project Structure

The analysis is performed on two datasets: `orders.csv` and `messages.csv`.

- **`orders.csv`**: Contains information about orders, including a `date` column, `product_id`, `city_id`, and `orders` quantity.
- **`messages.csv`**: Contains messages with a `date` (timestamp) and `msg` content.

---

## Key Steps and Analysis

### 1. Data Loading and Initial Inspection

The project begins by loading the `orders.csv` and `messages.csv` datasets into pandas DataFrames. The `head()` method is used to preview the first few rows of each DataFrame, and `info()` provides a summary of the DataFrame's structure, including data types and non-null counts.

```python
import pandas as pd
import numpy as np

# Load the datasets
date = pd.read_csv('orders.csv')
time = pd.read_csv('messages.csv')

# Display first 5 rows
print("Orders DataFrame:")
print(date.head(5))
print("\nMessages DataFrame:")
print(time.head(5))

# Display DataFrame information
print("\nOrders DataFrame Info:")
date.info()
print("\nMessages DataFrame Info:")
time.info()
```

### 2. Converting to Datetime Objects

The `date` columns in both DataFrames are initially of `object` (string) dtype. To enable date and time operations, these columns are converted to `datetime64[ns]` using `pd.to_datetime()`.

```python
# Convert 'date' columns to datetime objects
date['date'] = pd.to_datetime(date['date'])
time['date'] = pd.to_datetime(time['date'])

# Verify the conversion
print("\nOrders DataFrame Info after datetime conversion:")
date.info()
print("\nMessages DataFrame Info after datetime conversion:")
time.info()
```

### 3. Extracting Date Components

New columns are created in the `date` DataFrame to extract various components from the `date` column:
- **`date_yr`**: Year
- **`date_mth`**: Month as an integer
- **`date_mth_name`**: Full month name
- **`date_day`**: Day of the month

```python
date['date_yr'] = date['date'].dt.year
date['date_mth'] = date['date'].dt.month
date['date_mth_name'] = date['date'].dt.month_name()
date['date_day'] = date['date'].dt.day

print("\nOrders DataFrame with extracted date components:")
print(date.head())
```

### 4. Extracting Day of Week and Checking for Weekends

Further date components are extracted to analyze the day of the week:
- **`da_of week`**: Day of the week as an integer (Monday=0, Sunday=6)
- **`daY_name`**: Full day name

A new binary column, `y_or_n`, is created to indicate whether a given date falls on a weekend (Saturday or Sunday), with `1` for weekend and `0` for weekday. This is achieved using `numpy.where()` in conjunction with `isin()`.

```python
date['da_of week'] = date['date'].dt.dayofweek
date['daY_name'] = date['date'].dt.day_name()

# Check if it's a weekend
date['y_or_n'] = np.where(date['daY_name'].isin(['Saturday', 'Sunday']), 1, 0)

print("\nOrders DataFrame with day of week and weekend indicator:")
print(date.head())
```

### 5. Calculating Time Differences

The current date and time are obtained using `datetime.datetime.now()`. The difference between the current date and each `date` in the `orders` DataFrame is calculated. The total number of days in these time differences is then extracted.

```python
import datetime

today = datetime.datetime.now()
print(f"\nToday's date and time: {today}")

# Calculate time difference
time_difference = today - date['date']
print("\nTime difference (timedelta objects):")
print(time_difference.head())

# Extract days from time difference
days_difference = (today - date['date']).dt.days
print("\nDays difference:")
print(days_difference.head())

# Alternative way to calculate days difference using timedelta64
days_difference_np = np.round((today - date['date']) / np.timedelta64(1, 'D'), 0)
print("\nDays difference (using numpy timedelta64):")
print(days_difference_np.head())
```

### 6. Extracting Hour from Messages

Finally, the hour component is extracted from the `date` column in the `messages` DataFrame, providing insight into the time of day messages were sent.

```python
time['hour'] = time['date'].dt.hour
print("\nMessages DataFrame with extracted hour:")
print(time.head())
```

---

## Conclusion

This project serves as a basic illustration of how to effectively manage and extract meaningful insights from date and time data in Python using the powerful `pandas` library. These techniques are crucial for time-series analysis, feature engineering for machine learning models, and generating reports based on temporal patterns.

---

Feel free to explore further by:
- Visualizing the extracted date and time features (e.g., orders per month, messages per hour).
- Analyzing the distribution of orders/messages across weekdays and weekends.
- Investigating trends over time for both datasets.
```
