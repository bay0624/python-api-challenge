# Weather API
A Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. To accomplish this, I utilized a simple Python library, the OpenWeatherMap API, and a little common sense to create a representative model of weather across world cities.

I used the following scatter plots to visualize the data:
 - Temperature (F) vs. Latitude
 - Humidity (%) vs. Latitude
 - Cloudiness (%) vs. Latitude
 - Wind Speed (mph) vs. Latitude
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

# Vacation API
I used the data i gathered above, as well as jupyter-gmaps and the Google Places API to plan likely vacation spots. I was able to narrow down the data frame to only show cities with perfect weather conditions: A max temperature lower than 80 degrees but higher than 70; Wind speed less than 10 mph; and Zero cloudiness.
<img width="977" alt="hotel_map_info" src="https://github.com/bay0624/python-api-challenge/blob/main/VacationPy/hotel_map_info.png">


<img width="977" alt="hotel_map_info" src="https://user-images.githubusercontent.com/53978733/117921905-8ed8c280-b2bf-11eb-8c47-46f0a4831ef3.png">
