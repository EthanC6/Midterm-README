# Midterm_Cheng_Jiayi-README
#Student Name: Jiayi Cheng Student Number: 1011110049
#Course INF1340 Lecture 0101
#instructor: Dr. Maher Elshakankiri

# House Price Analysis

This project analyzes a dataset of house prices, including various attributes such as area, number of rooms, parking availability, and more. The analysis aims to extract insights related to house pricing.

## Table of Contents
- [Introduction](#introduction)
- [Data](#data)
- [Code](#code)
- [Analysis Goals](#analysis-goals)
- [Results](#results)

## Introduction
This analysis is designed to explore house prices across different areas and provide valuable insights into pricing trends based on various features.

## Data
The dataset used in this analysis contains the following columns:

| NO. | Area | Room | Parking | Warehouse | Elevator | Address | Price(USD) |
|-----|------|------|---------|-----------|----------|---------|-------------|
| 1   | 63   | 1    | TRUE    | TRUE      | 1        | Shahran | 61666.67    |
| 2   | 60   | 1    | TRUE    | TRUE      | 1        | Shahran | 61666.67    |
| ... | ...  | ...  | ...     | ...       | ...      | ...     | ...         |
| 200 | 124  | 3    | TRUE    | TRUE      | 1        | Pirouzi | 132266.67   |

(The above table shows a sample of the data. The full dataset contains 200 rows.)

## Results
*The highest house price:** 898,333.33 USD
*The lowest house price:** 120 USD

## Analysis Goals

1. Find the most expensive house price in USD.
2. Find the cheapest house price in USD.
3. Find the average price for houses with different numbers of rooms.
4. Find the top 3 addresses with the most houses.
5. Find the average price for houses in Punak.
6. Find house price ranges for houses in Punak.
7. Find the elevator percentage for houses in Punak, West Ferdows Boulevard, and Saadat Abad.
8. Find the top 10 addresses with the highest warehouse percentage.


## Results
*The highest house price:** 898,333.33 USD
*The lowest house price:** 120 USD
### Average prices for houses with different number of rooms:

| Room | Average Price (USD) |
|------|---------------------|
| 1    | 62,624.88           |
| 2    | 94,440.74           |
| 3    | 269,019.15          |
| 0    | 6,883.34            |
| 4    | 733,333.34          |

### Top 3 addresses with the most houses:

| Address                   | Count |
|---------------------------|-------|
| Punak                     | 15    |
| West Ferdows Boulevard    | 14    |
| Saadat Abad               | 11    |

- **The average price for houses in Punak:** 99,537.78 USD
- **The price range for houses in Punak:** from 58,333.33 to 254,400.00 USD

### Elevator percentage for houses:

| Address                   | Elevator Percentage |
|---------------------------|---------------------|
| Punak                     | 66.67%              |
| West Ferdows Boulevard    | 85.71%              |
| Saadat Abad               | 100.00%             |

### Top 10 addresses with the highest percentage of properties with a warehouse:

| Address                            | Percentage |
|------------------------------------|------------|
| Abbasabad                          | 100.0%     |
| Marzdaran                          | 100.0%     |
| Narmak                             | 100.0%     |
| Amirieh                            | 100.0%     |
| Northern Janatabad                 | 100.0%     |
| Northren Jamalzadeh               | 100.0%     |
| Pakdasht KhatunAbad              | 100.0%     |
| Pardis                             | 100.0%     |
| Pasdaran                           | 100.0%     |
| Persian Gulf Martyrs Lake         | 100.0%     |


## Code
```python
# Input data
from google.colab import drive
drive.mount('/drive', force_remount=True)
%cd '/drive/MyDrive/data/CSV/'

import pandas as pd

# Prompt the user to enter the input file name
input_file = input("Enter the input file name: ")

# Read the data from the input file, and let you know it loaded successfully
df = pd.read_csv('middata.csv')
print("Data loaded successfully!")

# Show reader some basic information about this dataset
print(df.head())
print(df.info())

#1 find the highest price

def find_highest_price_usd(df):
    highest_price = float(df["Price(USD)"].iloc[0])
    for price_usd in df["Price(USD)"]:
        if float(price_usd) > highest_price:
            highest_price = float(price_usd)
    return highest_price

#2. find the cheapest house's price in USD
def find_lowest_price_usd(df):
    lowest_price = float(df["Price(USD)"].iloc[0])
    for price_usd in df["Price(USD)"]:
        if float(price_usd) < lowest_price:
            lowest_price = float(price_usd)
    return lowest_price


#3 Find average prices for houses with different number of rooms.
#find how many room a house may have
unique_rooms = df['Room'].unique()
print("Unique numbers of rooms:", unique_rooms)
#find the average price for each kind of house with different number of rooms
average_prices = {}
for room in unique_rooms:
    room_prices = df[df['Room'] == room]['Price(USD)']
    average_price = room_prices.sum() / len(room_prices)
    average_prices[room] = average_price
# Sort the average prices by room number
sorted_average_prices = dict(sorted(average_prices.items()))

#4 find the top 3 area with most house
# Find all unique addresses
unique_addresses = df['Address'].unique()
print("Unique addresses:"+str(unique_addresses))
# Count the number of houses for each address
address_counts = df['Address'].value_counts()
# Find the top 3 addresses with the most houses
top_3_addresses = address_counts.head(3)
print("\nTop 3 addresses with the most houses:",top_3_addresses)

#5. find the average price for houses in Punak
# Filter houses in Punak and calculate the average price
punak_houses = df[df['Address'] == 'Punak']
average_price_punak = punak_houses['Price(USD)'].mean()

#6 find the price range for houses in Punak
# Filter houses in Punak
punak_houses = df[df['Address'] == 'Punak']
# Find the minimum and maximum prices
min_price_punak = punak_houses['Price(USD)'].min()
max_price_punak = punak_houses['Price(USD)'].max()

#7. find the elevator percentage for houses in Punak,West Ferdows Boulevard, and Saadat Abad
# define addresses to check
addresses_to_check = ['Punak', 'West Ferdows Boulevard', 'Saadat Abad']
# Calculate the elevator percentage for each address in the list and store the results
elevator_percentages = []
for address in addresses_to_check:
    address_houses = df[df['Address'] == address]
    if len(address_houses) == 0:
        elevator_percentages.append((address, 0))
        print(f"No houses found in {address}")
    else:
        elevator_percentage = (address_houses['Elevator'].sum() / len(address_houses)) * 100
        elevator_percentages.append((address, elevator_percentage))
        print(f"Elevator percentage for houses in {address}: {elevator_percentage:.2f}%")
# Convert the results to a DataFrame
Q7 = pd.DataFrame(elevator_percentages, columns=['Address', 'Elevator Percentage'])

#8. find the top 10 area with most warehouse percentage
# Filter rows where Warehouse is True
warehouse_true = df[df['Warehouse'] == True]
# Group by Address and calculate the percentage of properties with a warehouse
address_warehouse_percentage = warehouse_true.groupby('Address').size() / df.groupby('Address').size() * 100
# Sort by percentage in descending order and get the top 3 addresses
top_10_addresses = address_warehouse_percentage.sort_values(ascending=False).head(10)

# Combine the outputs
output = "\n\nThe highest house price:\n" + str(highest_price_usd) + \
  "\n\nThe lowest house price:\n" + str(lowest_usdprice) + \
  "\n\nAverage prices for houses with different number of rooms:\n" + str(Q3) + \
  "\n\nTop 3 addresses with the most houses:\n" + str(top_3_addresses) + \
  "\n\nThe average price for houses in Punak:\n" + str(average_price_punak) + \
  "\n\nThe price range for houses in Punak:\n from " + str(min_price_punak) + " to " + str(max_price_punak) + \
  "\n\nElevator percentage for houses in Punak, West Ferdows Boulevard, and Saadat Abad:\n" + str(Q7) + \
  "\n\nTop 10 addresses with the highest percentage of properties with a warehouse:\n" + str(top_10_addresses)

# Prompt the user to provide the output file name
output_file = input("Enter the output file name (with extension): ")

# Save the output to the specified file
with open(output_file, 'w') as file:
    file.write(output)
print("Output saved successfully!")
