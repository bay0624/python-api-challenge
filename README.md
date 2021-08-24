# Weather API
A Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. To accomplish this, I utilized a simple Python library, the OpenWeatherMap API, and a little common sense to create a representative model of weather across world cities.

### Dependencies
```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time
import json
import scipy.stats as st
from scipy.stats import linregress
from api_keys import weather_api_key

from citipy import citipy
```
### Generating City Lists
```python
lat_lngs = []
cities = []

# A set of random lat and lng combinations
lats = np.random.uniform(lat_range[0], lat_range[1], size=1500)
lngs = np.random.uniform(lng_range[0], lng_range[1], size=1500)
lat_lngs = zip(lats, lngs)

# Identifying nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
len(cities)
```
### Performing API Calls and inserting cities into a DataFrame
```python 
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "imperial"

print("Beginning Data Retrieval")
print("------------------------")

record_num = 1
num_of_set = 1

for index, row in cities_weather_df.iterrows():
    
    city = row["City"]
        
    target_url = f"{url}appid={weather_api_key}&units={units}&q={city}"
    response = requests.get(target_url)
    data = response.json()
    
    try:
        cities_weather_df.loc[index, "Cloudiness"] = data["clouds"]["all"]
        cities_weather_df.loc[index, "Country"] = data["sys"]["country"]
        cities_weather_df.loc[index, "Date"] = data["dt"]
        cities_weather_df.loc[index, "Humidity"] = data["main"]["humidity"]
        cities_weather_df.loc[index, "Lat"] = data["coord"]["lat"]
        cities_weather_df.loc[index, "Lng"] = data["coord"]["lon"]
        cities_weather_df.loc[index, "Max Temp"] = data["main"]["temp_max"]
        cities_weather_df.loc[index, "Wind Speed"] = data["wind"]["speed"]
            
        print(f"Processing Record {record_num} of Set {num_of_set}| {city}")
        record_num += 1
        
    except:
        print("City not found. Skipping...")
        
    if record_num % 51 == 0 and record_num >= 51:
            num_of_set += 1
            record_num = 1
            time.sleep(60)

print("-----------------------")
print("Data Retrieval Complete")
print("-----------------------")
```

### I used the following scatter plots to visualize the data:
 - Temperature (F) vs. Latitude
 - Humidity (%) vs. Latitude
 - Cloudiness (%) vs. Latitude
 - Wind Speed (mph) vs. Latitude

### Scatter plot code snippet for Temperature (F) vs. Latitude
```python 
latitude_values = cities_df["Lat"]
temp_values = cities_df["Max Temp"]
plt.figure(figsize=(10,5))
plt.scatter(latitude_values, temp_values, marker="o", facecolors="blue", alpha=0.6, edgecolor="black", s=50)
plt.title("City Latitude vs. Max Temperature (5/13/2021)")
plt.xlabel("Latitude")
plt.ylabel("Max Temperature (F)")
plt.grid()
plt.show()
plt.tight_layout()
```

### Scatter plot images for Temperature (F), Humidity (%), Cloudiness (%) and  Wind Speed (mph) vs. Latitude
![Plots_img](https://user-images.githubusercontent.com/53978733/117922206-1de5da80-b2c0-11eb-8a4a-4e7d50d3638f.jpg)

I also ran linear regression on each relationship above by creating 8 additional plots. I separated the plots into Northern Hemisphere (greater than or equal to 0 degrees latitude) and Southern Hemisphere (less than 0 degrees latitude). The following are the plots are created (and also calculated the r-value of each):
 - Northern Hemisphere - Temperature (F) vs. Latitude
 - Southern Hemisphere - Temperature (F) vs. Latitude
 - Northern Hemisphere - Humidity (%) vs. Latitude
 - Southern Hemisphere - Humidity (%) vs. Latitude
 - Northern Hemisphere - Cloudiness (%) vs. Latitude
 - Southern Hemisphere - Cloudiness (%) vs. Latitude
 - Northern Hemisphere - Wind Speed (mph) vs. Latitude
 - Southern Hemisphere - Wind Speed (mph) vs. Latitude

### Code snippet for linear regression
```python 
northern_hemisphere = cities_df["Lat"] > 0
northern_hemisphere_table = cities_df[northern_hemisphere]

x_values = northern_hemisphere_table["Lat"]
y_values = northern_hemisphere_table["Max Temp"]
(slope, intercept, rvalue, pvalue, stderr) = linregress(x_values, y_values)
regress_values = x_values * slope + intercept
line_eq = "y = " + str(round(slope,2)) + "x + " + str(round(intercept,2))

# print(f"The r-value is {st.pearsonr(x_values,y_values)[0]}")
print(f"The r-value is: {rvalue}")

plt.figure(figsize=(10,5))
plt.scatter(x_values,y_values, marker="o", facecolors="blue", alpha=0.6, edgecolor="black", s=50)
plt.plot(x_values,regress_values,"r-", alpha=0.6)
plt.annotate(line_eq,(0,20),fontsize=12,color="red")
plt.title("Northern Hemisphere - Max Temperature vs. Latitude")
plt.xlabel("Latitude")
plt.ylabel("Max Temperature (F)")
plt.show()
```

# Vacation API
I used the data i gathered above, as well as jupyter-gmaps and the Google Places API to plan likely vacation spots. I was able to narrow down the data frame to only show cities with perfect weather conditions: A max temperature lower than 80 degrees but higher than 70; Wind speed less than 10 mph; and Zero cloudiness.
<img width="977" alt="hotel_map_info" src="https://github.com/bay0624/python-api-challenge/blob/main/VacationPy/hotel_map_info.png">
