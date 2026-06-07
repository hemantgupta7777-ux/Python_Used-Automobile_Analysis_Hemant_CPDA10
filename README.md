Used Automobile Analysis
Objective
Analyze used car listings to identify pricing patterns, market trends, vehicle condition effects, and regional sales performance.

Dataset
car_prices.csv

Tools Used
Python
Pandas
NumPy
Matplotlib
Seaborn

[ ]
# Import Data
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

[ ]
from google.colab import files

uploaded = files.upload()


[ ]
# Load dataset

df_car_prices = pd.read_csv('/content/car_prices.csv')

[ ]
# Display first 5 rows

df_car_prices.head()


[ ]
df_car_prices = pd.read_csv('/content/car_prices.csv')

[ ]
# Dataset Information

df_car_prices.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 558837 entries, 0 to 558836
Data columns (total 16 columns):
 #   Column        Non-Null Count   Dtype  
---  ------        --------------   -----  
 0   year          558837 non-null  int64  
 1   make          548536 non-null  object 
 2   model         548438 non-null  object 
 3   trim          548186 non-null  object 
 4   body          545642 non-null  object 
 5   transmission  493485 non-null  object 
 6   vin           558833 non-null  object 
 7   state         558837 non-null  object 
 8   condition     547017 non-null  float64
 9   odometer      558743 non-null  float64
 10  color         558088 non-null  object 
 11  interior      558088 non-null  object 
 12  seller        558837 non-null  object 
 13  mmr           558799 non-null  float64
 14  sellingprice  558825 non-null  float64
 15  saledate      558825 non-null  object 
dtypes: float64(4), int64(1), object(11)
memory usage: 68.2+ MB

[ ]
# Total number of records

len(df_car_prices)
558837

[ ]
# Shape of dataset

df_car_prices.shape
(558837, 16)

[ ]
# Column names

df_car_prices.columns
Index(['year', 'make', 'model', 'trim', 'body', 'transmission', 'vin', 'state',
       'condition', 'odometer', 'color', 'interior', 'seller', 'mmr',
       'sellingprice', 'saledate'],
      dtype='object')

[ ]
# Data types of columns

df_car_prices.dtypes


[ ]
# Count missing values

df_car_prices.isnull().sum()


[ ]
import matplotlib.pyplot as plt

# Bar chart for missing values

plt.figure(figsize=(12,6))

df_car_prices.isnull().sum().plot(kind='bar', color='pink')

plt.title('Missing Values Per Column')

plt.xlabel('Columns')
plt.ylabel('Count of Missing Values')

plt.xticks(rotation=90)

plt.show()


[ ]
# Heatmap for missing values

plt.figure(figsize=(12,6))

sns.heatmap(df_car_prices.isnull(), cbar=False, cmap='viridis')

plt.title('Missing Values Heatmap')

plt.show()


[ ]
# Fill numerical null values with median

num_cols = ['condition', 'odometer', 'mmr', 'sellingprice']

for col in num_cols:
    df_car_prices[col].fillna(df_car_prices[col].median(), inplace=True)
/tmp/ipykernel_1829/4188619101.py:6: FutureWarning: A value is trying to be set on a copy of a DataFrame or Series through chained assignment using an inplace method.
The behavior will change in pandas 3.0. This inplace method will never work because the intermediate object on which we are setting values always behaves as a copy.

For example, when doing 'df[col].method(value, inplace=True)', try using 'df.method({col: value}, inplace=True)' or df[col] = df[col].method(value) instead, to perform the operation inplace on the original object.


  df_car_prices[col].fillna(df_car_prices[col].median(), inplace=True)

[ ]
# Fill categorical null values with mode

cat_cols = ['make', 'model', 'trim', 'body',
            'transmission', 'color',
            'interior', 'seller', 'state']

for col in cat_cols:
    df_car_prices[col].fillna(df_car_prices[col].mode()[0], inplace=True)
/tmp/ipykernel_1829/1603163615.py:8: FutureWarning: A value is trying to be set on a copy of a DataFrame or Series through chained assignment using an inplace method.
The behavior will change in pandas 3.0. This inplace method will never work because the intermediate object on which we are setting values always behaves as a copy.

For example, when doing 'df[col].method(value, inplace=True)', try using 'df.method({col: value}, inplace=True)' or df[col] = df[col].method(value) instead, to perform the operation inplace on the original object.


  df_car_prices[col].fillna(df_car_prices[col].mode()[0], inplace=True)

[ ]
# Check missing values after treatment

df_car_prices.isnull().sum()

Missing Value Treatment Strategy
Numerical columns were imputed using Median because it is robust against outliers.
Categorical columns were imputed using Mode because it represents the most frequent category.
This approach preserves data distribution while minimizing information loss.

[ ]
# Check duplicate records
duplicate_count = df_car_prices.duplicated().sum()

print("Duplicate Records:", duplicate_count)

# Remove duplicates
df_car_prices = df_car_prices.drop_duplicates()

print("Shape after removing duplicates:", df_car_prices.shape)
Duplicate Records: 0
Shape after removing duplicates: (558837, 17)

[ ]
# Final cleaned dataset info

df_car_prices.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 558837 entries, 0 to 558836
Data columns (total 16 columns):
 #   Column        Non-Null Count   Dtype  
---  ------        --------------   -----  
 0   year          558837 non-null  int64  
 1   make          558837 non-null  object 
 2   model         558837 non-null  object 
 3   trim          558837 non-null  object 
 4   body          558837 non-null  object 
 5   transmission  558837 non-null  object 
 6   vin           558833 non-null  object 
 7   state         558837 non-null  object 
 8   condition     558837 non-null  float64
 9   odometer      558837 non-null  float64
 10  color         558837 non-null  object 
 11  interior      558837 non-null  object 
 12  seller        558837 non-null  object 
 13  mmr           558837 non-null  float64
 14  sellingprice  558837 non-null  float64
 15  saledate      558825 non-null  object 
dtypes: float64(4), int64(1), object(11)
memory usage: 68.2+ MB

[ ]
# Save cleaned dataset

df_car_prices.to_csv('cleaned_car_prices.csv', index=False)
Data Frame Queries

[ ]
# Average, Minimum and Maximum Selling Price

print("Average Car Price:", df_car_prices['sellingprice'].mean())

print("Minimum Car Price:", df_car_prices['sellingprice'].min())

print("Maximum Car Price:", df_car_prices['sellingprice'].max())

Average Car Price: 13611.326356343621
Minimum Car Price: 1.0
Maximum Car Price: 230000.0

[ ]
# Unique colors of cars and their counts

color_counts = df_car_prices['color'].value_counts().reset_index()
color_counts.columns = ['Color', 'Count']
display(color_counts)

Next steps:

[ ]
# Bar plot of color counts
plt.figure(figsize=(15, 7))
sns.barplot(x='Color', y='Count', data=color_counts, color='lightpink')
plt.title('Distribution of Car Colors')
plt.xlabel('Car Color')
plt.ylabel('Number of Cars')
plt.xticks(rotation=90)
plt.tight_layout()
plt.show()


[ ]
# Number of unique car brands

print("Unique Car Brands:", df_car_prices['make'].nunique())

# Number of unique car models

print("Unique Car Models:", df_car_prices['model'].nunique())
Unique Car Brands: 96
Unique Car Models: 973

[ ]
# Cars having selling price greater than 165000

high_price_cars = df_car_prices[df_car_prices['sellingprice'] > 165000]

high_price_cars


[ ]
# Top 5 most frequently sold car models

df_car_prices['model'].value_counts().head(5)


[ ]
# Average selling price by make

avg_price_by_make = df_car_prices.groupby('make')['sellingprice'].mean()

avg_price_by_make.sort_values(ascending=False)


[ ]
# Minimum selling price for each interior

min_price_interior = df_car_prices.groupby('interior')['sellingprice'].min()

min_price_interior


[ ]
# Highest odometer reading per year

highest_odometer = df_car_prices.groupby('year')['odometer'].max()

highest_odometer.sort_values(ascending=False)


[ ]
# Create car_age column

df_car_prices['car_age'] = 2025 - df_car_prices['year']

# Display year and car age

df_car_prices[['year', 'car_age']].head()


[ ]
# Filter cars based on condition and odometer

filtered_cars = df_car_prices[
    (df_car_prices['condition'] >= 48) &
    (df_car_prices['odometer'] > 90000)
]

# Count filtered cars

print("Number of Cars:", filtered_cars.shape[0])
Number of Cars: 746

[ ]
# Filter newer cars

newer_cars = df_car_prices[df_car_prices['year'] > 2013]

# Average selling price by state

state_prices = newer_cars.groupby('state')['sellingprice'].mean()

# Sort from highest to lowest

state_prices.sort_values(ascending=False)


[ ]
# Find top 20% condition threshold

condition_threshold = df_car_prices['condition'].quantile(0.80)

# Filter excellent condition cars

excellent_cars = df_car_prices[
    df_car_prices['condition'] >= condition_threshold
]

# Average price by make

value_for_money = excellent_cars.groupby('make')['sellingprice'].mean()

# Lowest average price first

value_for_money.sort_values().head(10)


[ ]
import matplotlib.pyplot as plt
import seaborn as sns
# Select numerical columns

numerical_columns = df_car_prices.select_dtypes(include=['int64', 'float64'])

# Correlation matrix

correlation_matrix = numerical_columns.corr()

# Plot heatmap

plt.figure(figsize=(10,8))

sns.heatmap(correlation_matrix,
            annot=True,
            cmap='RdPu',
            fmt='.2f')

plt.title('Correlation Matrix of Numerical Features')

plt.show()


[ ]
correlation_matrix['sellingprice']\
.sort_values(ascending=False)

Positive correlation with year Negative correlation with odometer Relationship with condition


[ ]
import matplotlib.pyplot as plt

# Average selling price by year

avg_price_year = df_car_prices.groupby('year')['sellingprice'].mean()

# Plot

plt.figure(figsize=(14,6))

avg_price_year.plot(kind='bar', color='Pink')

plt.title('Average Selling Price by Year')

plt.xlabel('Year')

plt.ylabel('Average Selling Price')

plt.show()


[ ]
# Scatter plot of odometer vs selling price

import matplotlib.pyplot as plt

plt.figure(figsize=(10,6))

plt.scatter(df_car_prices['odometer'],
            df_car_prices['sellingprice'],
            alpha=0.5,
            color='lightpink')

plt.title('Selling Price vs Odometer')

plt.xlabel('Odometer')

plt.ylabel('Selling Price')

plt.show()


[ ]
import matplotlib.pyplot as plt

# Cars sold by state

cars_by_state = df_car_prices['state'].value_counts()

# Plot

plt.figure(figsize=(15,6))

cars_by_state.plot(kind='bar', color='lightpink')

plt.title('Number of Cars Sold by State')

plt.xlabel('State')

plt.ylabel('Number of Cars Sold')

plt.show()


[ ]
# Top 3 car selling states

cars_by_state.head(3)


[ ]
import pandas as pd

# Create condition bins

df_car_prices['condition_range'] = pd.cut(
    df_car_prices['condition'],
    bins=range(0, 105, 5)
)

[ ]
# Average price by condition range

avg_price_condition = df_car_prices.groupby(
    'condition_range'
)['sellingprice'].mean()
/tmp/ipykernel_1829/2868116431.py:3: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
  avg_price_condition = df_car_prices.groupby(

[ ]
# Plot graph

plt.figure(figsize=(14,6))

avg_price_condition.plot(kind='bar', color='lightpink')

plt.title('Average Selling Price by Condition Range')

plt.xlabel('Condition Range')

plt.ylabel('Average Selling Price')

plt.xticks(rotation=45)

plt.show()


[ ]
# Plot graph for average selling price by make

plt.figure(figsize=(16, 8))

avg_price_by_make.sort_values(ascending=False).head(15).plot(kind='bar', color='lightpink')

plt.title('Top 15 Car Makes by Average Selling Price')

plt.xlabel('Car Make')

plt.ylabel('Average Selling Price')

plt.xticks(rotation=45, ha='right')

plt.tight_layout()

plt.show()

Most cars are concentrated in medium to high condition ranges. Very low-condition cars are less frequently sold.


[ ]
# Box plot of selling price by color
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(15,6))

sns.boxplot(
    x='color',
    y='sellingprice',
    data=df_car_prices,
    color='lightpink'
)

plt.title('Selling Price Distribution by Color')

plt.xticks(rotation=90)

plt.show()

Some car colors have higher price variations. Luxury cars in certain colors create extreme outliers.


[ ]
# Calculate Q1 and Q3

Q1 = df_car_prices['sellingprice'].quantile(0.25)

Q3 = df_car_prices['sellingprice'].quantile(0.75)

# Calculate IQR

IQR = Q3 - Q1

# Lower and Upper Limits

lower_limit = Q1 - 1.5 * IQR

upper_limit = Q3 + 1.5 * IQR

# Remove outliers

df_no_outliers = df_car_prices[
    (df_car_prices['sellingprice'] >= lower_limit) &
    (df_car_prices['sellingprice'] <= upper_limit)
]

[ ]
plt.figure(figsize=(15,6))

sns.boxplot(
    x='color',
    y='sellingprice',
    data=df_no_outliers,
    color='lightpink'
)

plt.title('Selling Price Distribution by Color (Without Outliers)')

plt.xticks(rotation=90)

plt.show()

Descriptive Statistics of Selling Price by Color (Without Outliers)
To provide a more analytical view, here are the key statistics (mean, standard deviation, min, max, quartiles) of selling prices grouped by car color, after removing outliers. This helps in understanding the central tendency and spread of prices for each color numerically.


[ ]
color_price_stats = df_no_outliers.groupby('color')['sellingprice'].describe()
display(color_price_stats.sort_values(by='mean', ascending=False))

After removing outliers, the distribution becomes more balanced and easier to compare across colors.

Dashboard Insights
Newer cars generally have higher selling prices. High mileage reduces resale value. Certain states dominate used car sales. Car condition strongly impacts pricing. Some brands appear more frequently due to higher market demand.


[ ]
fig, axes = plt.subplots(2, 2, figsize=(18, 12))

# ---------------------------------------------
# 1. Average Selling Price by Year
# ---------------------------------------------

avg_price_year = df_car_prices.groupby('year')['sellingprice'].mean()

avg_price_year.plot(
    kind='bar',
    ax=axes[0,0],
    color='lightpink' # Applied lightpink color
)

axes[0,0].set_title('Average Selling Price by Year')
axes[0,0].set_xlabel('Year')
axes[0,0].set_ylabel('Avg Selling Price')

# ---------------------------------------------
# 2. Cars Sold by State
# ---------------------------------------------

state_sales = df_car_prices['state'].value_counts().head(10)

state_sales.plot(
    kind='bar',
    ax=axes[0,1],
    color='lightpink' # Applied lightpink color
)
axes[0,1].set_title('Top 10 States by Car Sales')
axes[0,1].set_xlabel('State')
axes[0,1].set_ylabel('Cars Sold')

# ---------------------------------------------
# 3. Selling Price vs Odometer
# ---------------------------------------------

axes[1,0].scatter(
    df_car_prices['odometer'],
    df_car_prices['sellingprice'],
    alpha=0.3,
    color='lightpink' # Applied lightpink color
)

axes[1,0].set_title('Selling Price vs Odometer')
axes[1,0].set_xlabel('Odometer')
axes[1,0].set_ylabel('Selling Price')
# ---------------------------------------------
# 4. Top 10 Car Brands
# ---------------------------------------------

top_brands = df_car_prices['make'].value_counts().head(10)

top_brands.plot(
    kind='bar',
    ax=axes[1,1],
    color='lightpink' # Applied lightpink color
)

axes[1,1].set_title('Top 10 Car Brands')
axes[1,1].set_xlabel('Brand')
axes[1,1].set_ylabel('Count')

# ---------------------------------------------
# Dashboard Layout
# ---------------------------------------------

plt.tight_layout()

plt.show()

Dashboard Insights
Here are some analytical insights derived from the dashboard:

Average Selling Price by Year: This plot clearly illustrates the trend of average car selling prices over different years. Generally, newer cars (higher year values) tend to have higher average selling prices, indicating depreciation over time. There might be some fluctuations, which could be due to economic factors, specific model releases, or changes in market demand.

Top 10 States by Car Sales: This bar chart highlights the states with the highest volume of car sales. This information is valuable for identifying key markets or regions where car demand is particularly high. States like Florida (fl), California (ca), and Pennsylvania (pa) appear to be dominant in terms of the number of cars sold.

Selling Price vs Odometer: This scatter plot shows the relationship between a car's odometer reading (mileage) and its selling price. A general trend of decreasing selling price with increasing odometer readings is evident, reflecting that cars with higher mileage typically command lower prices due to wear and tear. The density of points can also reveal common mileage ranges for certain price points.

Top 10 Car Brands: This bar chart displays the ten most frequently sold car brands. This indicates popular brands in the used car market, which can be useful for inventory management, marketing strategies, or understanding brand influence on sales volume. Brands like Nissan (Altima), Ford (F-150, Fusion, Escape), and Toyota (Camry) are among the most frequently sold.

Final Project Conclusion
This project embarked on a comprehensive analysis of used car sales data, employing a robust methodology that encompassed data cleaning, exploratory data analysis (EDA), advanced visualization, and dashboard creation. Utilizing Python libraries such as Pandas for data manipulation, Matplotlib and Seaborn for visualization, we transformed raw data into actionable insights.

Key Stages and Outcomes:

Data Cleaning and Preprocessing: We meticulously addressed missing values by imputing numerical columns with medians and categorical columns with modes, ensuring data integrity. Duplicate entries were handled, and relevant features like car_age were engineered to enrich the dataset.

Exploratory Data Analysis (EDA): Through statistical summaries and initial plots, we gained a foundational understanding of the data's distribution, central tendencies, and potential anomalies. This phase was crucial for identifying trends before deep-diving into specific questions.

Visualization and Insight Generation: A variety of visualizations were created to illuminate different facets of the data:

Selling Price Trends: We observed clear relationships between sellingprice and year, odometer readings, and condition (e.g., newer cars with lower mileage and better condition generally commanded higher prices).
Geographic and Brand Insights: Visualizations highlighted the top states for car sales and the most frequently sold car brands, indicating regional market strengths and brand popularity.
Color Distribution and Impact: An analysis of car colors revealed the most common choices and how color, even after outlier removal, could influence selling price distributions, with certain colors showing more price variance.
Interactive Dashboard Creation: A consolidated dashboard was developed to present key findings cohesively. This allowed for a multi-dimensional view of factors influencing car sales and pricing, such as average selling price by year, cars sold by state, selling price versus odometer, and top car brands.

Overall Impact:

This project successfully demonstrated a practical application of data science techniques in understanding a complex dataset. The insights derived provide valuable intelligence for stakeholders in the used car market, aiding in strategic decision-making related to pricing, inventory management, marketing, and identifying high-value market segments. The robust data preprocessing and analytical visualizations lay a strong foundation for potential future work, such as predictive modeling to forecast car prices or identify optimal selling strategies.

Business Recommendations
Newer vehicles command higher resale values.
Vehicle condition significantly influences price.
High mileage reduces market value.
Top-selling states should receive greater inventory allocation.
Brands offering lower average prices with high condition scores represent value-for-money opportunities.
