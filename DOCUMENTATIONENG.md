# Final Project Documentation

## Overview
This code consists of a Python application that utilizes the [restcountries](https://restcountries.com/) API to obtain information about countries, such as region, name, capital, currency, population, area, latitude, and longitude.

## Libraries Used
- **pandas**: Used for data manipulation and analysis, especially for reading JSON data obtained from the API and writing them to CSV files.
- **requests**: Used to make HTTP requests to the restcountries API.
- **csv**: Python's standard library for CSV file manipulation.
- **datetime**: Used for date and time manipulation.
- **plyer**: Used for sending notifications.
- **os**: Python's standard library for operating system-related functionalities.
- **sqlite3**: Database for storing information.
- ***ast***: Module in the library that helps Python applications process abstract syntax tree grammars.

## Code Steps

### 1. Importing Libraries
```python
import pandas as pd
import requests
import csv
from datetime import datetime
from plyer import notification
import os
import sqlite3
import ast
```

### 2. Requesting and Processing API Data
The code makes requests to the restcountries API to obtain information about countries in different aspects like region, name, capital, currency, population, area, latitude, and longitude.

### 3. Data Manipulation and Writing to CSV Files
After obtaining data from the API, the code converts the JSON results into pandas DataFrames and writes them to CSV files.

### 4. Alert Notification Function
The code includes an `alert()` function that checks whether the CSV files were successfully saved in the destination folder. Depending on the result, an alert notification is sent.

### 5. Calling the Notification Function
The notification function is called at the end of the script to send the alert notification.

## Execution
To execute this code, simply run each code cell sequentially in a Python environment with the necessary libraries installed. Ensure to replace the destination folder path in the `alert()` function with the correct path for your system.

## Notes
- This code depends on the availability and stability of the restcountries API.
- Make sure the Python environment you are running the code in has internet access to make requests to the API.
- Notifications will only be sent if the plyer library is installed and correctly configured in the Python environment.

### 6. Importing Libraries and Reading CSV Files
The code imports the `sqlite3` library to work with SQLite and the `pandas` library for data manipulation. Then, it reads the CSV files generated in the previous step into pandas DataFrames.

```python
import sqlite3
import pandas as pd

# Reading CSV files
paises_regiao = pd.read_csv('paises_regiao.csv')
paises_moeda = pd.read_csv('paises_moeda.csv')
paises_area = pd.read_csv('paises_area.csv')
```

### 7. Handling and Renaming Columns in DataFrames
The DataFrames are processed to ensure the data is formatted correctly. Then, the columns are renamed for better understanding of the data.

```python
# Renaming columns in DataFrames
paises_regiao.rename(columns={'name': 'Nome', 'ccn3': 'Código', 'capital': 'Capital', 'region': 'Região'}, inplace=True)
paises_moeda.rename(columns={'ccn3': 'Código', 'currencies': 'Moeda'}, inplace=True)
paises_area.rename(columns={'ccn3': 'Código', 'latlng': 'Coordenadas', 'area': 'Área', 'population': 'População'}, inplace=True)
```

### 8. Variable Types Analysis
The `info()` function of pandas is used to analyze the variable types in each DataFrame, as well as the count of non-null values in each column.

```python
# Variable types analysis in each DataFrame
print("Variable types in paises_regiao:")
print(paises_regiao.info())

print("\nVariable types in paises_moeda:")
print(paises_moeda.info())

print("\nVariable types in paises_area:")
print(paises_area.info())
```

### 9. Missing Values Analysis
The `isna().sum()` function of pandas is used to analyze the amount of missing values in each column of each DataFrame.

```python
# Missing values analysis in each DataFrame
print("Missing values in paises_regiao:")
print(paises_regiao.isna().sum())

print("\nMissing values in paises_moeda:")
print(paises_moeda.isna().sum())

print("\nMissing values in paises_area:")
print(paises_area.isna().sum())
```

### 10. Creation of sqlite3 Database and Data Insertion
A sqlite3 database is created using the `sqlite3` library. Then, a table called "paises" is created to store countries with their respective information. The data is inserted into the table using a loop over the DataFrame rows.

```python
# Creation of sqlite3 database
conn = sqlite3.connect('paises.db')
cursor = conn.cursor()

# Creation of "paises" table and data insertion
cursor.execute('''
    CREATE TABLE paises (
        id INTEGER PRIMARY KEY,
        nome TEXT,
        codigo INTEGER,
        capital TEXT,
        regiao TEXT
    )
''')

for index, row in paises_regiao.iterrows():
    cursor.execute('''
        INSERT INTO paises (nome, codigo, capital, regiao) VALUES (?, ?, ?, ?)
    ''', (row['Nome'], row['Código'], row['Capital'], row['Região']))

conn.commit()
cursor.close()
conn.close()
```

### 11. Importing Libraries and Reading CSV Files
Once again, libraries are imported, and CSV files are read so that they can be processed in the final part of the project.

### 12. Handling of datasets
The datasets are processed by extracting information, creating new columns, and then deleting specific columns. Some rows are also removed, those with null values. The beginning of the dataset handling can be seen below.

```python
# REGION BASE
paises_regiao_without_nulls1 = pd.read_csv('paises_regiao.csv')

paises_regiao_without_nulls = paises_regiao_without_nulls1.dropna(how='any')

# function to extract code and currency name
def extract_country_info(row):
    try:
        code = row['ccn3']
        info_name = ast.literal_eval(row['name'])  # Convert the string to a Python dictionary
        country_name = info_name['common']
        capital = ast.literal_eval(row['capital'])[0]
        region = row['region']
        return code, country_name, capital, region
    except (KeyError, ValueError, SyntaxError, IndexError):
        return None, None, None, None

# Apply the function to extract country name, capital, and region and create new columns
paises_regiao_without_nulls['Code'], paises_regiao_without_nulls['Country Name'], paises_regiao_without_nulls['Capital'], paises_regiao_without_nulls['Region'] = zip(*paises_regiao_without_nulls.apply(extract_country_info, axis=1))

# Drop "Currency" and "ccn3" columns
paises_regiao_without_nulls.drop(columns=['name'], inplace=True)
paises_regiao_without_nulls.drop(columns=['ccn 3'], inplace=True)
paises_regiao_without_nulls.drop(columns=['region'], inplace=True)
paises_regiao_without_nulls.drop(columns=['capital'], inplace=True)

print(paises_regiao_without_nulls)
```

### 13. Saving processed datasets to CSV and analyzing variable types.
The next step is to save all the processed datasets and new csv files, additionally, it prints the variable types in each dataframe. Afterwards, the check will be done without a base with null data.

### 14. Joining tables
This step aims to join the 3 tables into one, creating a single document with all relevant columns and rows.

```python
# first join (region and currency) using the "code" key
final_df = pd.merge(paises_regiao_without_nulls, paises_moeda_without_nulls, on='Code', how='inner')

# second join (merged_df with area) using the "code" key
full_base = pd.merge(final_df, paises_area_without_nulls, on='Code', how='inner')

# Display the resulting DataFrame
print(full_base)
```

### 15. Creating the table in the database
Here is where we start creating the table within the database.

First, we establish a connection with the database and inform that, if necessary, an existing table will be replaced with the new one, with the processed information.

### 16. Selection and visualization of tables
Finally, as the last step, we select the created table and print it entirely to verify if all the information was correct, and then, we can visualize the assembled table with all columns and rows organized.