# Hotel sales analysis with Python and Tableau

## 1. Resume
This project integrates two key tools: Python for data cleaning, normalization, and transformation, and Tableau for data visualization and presentation. The dataset, sourced from [Kaggle](https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand) under the profile of "Jesse Mostipak" is first imported into Python for the preprocessing, cleaning and normalization. Then, the clean database is exported for further use in Tableau.

## 2. Data visualization

<a href="https://public.tableau.com/views/Revenuereport_17273939714540/Revenuereport?:language=es-ES&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link"><img src='./images/Revenue report.png'></a>
**Note:** Markdown doesn't support Tableau frame, so you can click the image

## 3. Code fragments

### Data cleaning
The dataset originally had the date split into separate columns (day, month, and year), with the month in string format. I replaced the month with its corresponding numerical value and combined the three columns into one to create a time series in Tableau.

|   date_year    |	date_month   |   date_day_of_month   |   new_column    |
| :---      | :---     | :---     | :---      |
|2015 | September | 17 |17/09/2015 |

Also droping irrelevant columns:

```python
table.drop(columns=["total_of_special_requests", "required_car_parking_spaces","market_segment","babies","previous_cancellations","meal","previous_bookings_not_canceled","reservation_status", "arrival_date_week_number", "days_in_waiting_list", "arrival_date_num_month"],inplace=True)
table.dropna(axis=0,subset="country",inplace=True)
table = table[table["country"] != "TMP"]
```

### Data normalizing

Since the company and agent numbers are coded, I removed the decimal places from the float values and converted them to strings to facilitate analysis of which company or agent generated the most sales. Additionally, the term "Hotel" was removed from the hotel column for relevance, and the children column was converted to integers.

```python
table["company"] = pd.to_numeric(table["company"],errors="coerce").replace(np.NaN,0).astype(int).astype(str)
table["children"] = pd.to_numeric(table["children"],errors="coerce").replace(np.NaN,0).astype(int)
table["agent"] = pd.to_numeric(table["agent"],errors="coerce").replace(np.NaN,0).astype(int).astype(str)
table["hotel"] = table["hotel"].str.replace(" Hotel","")
```