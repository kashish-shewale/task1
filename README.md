# task1
import requests
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Function to fetch data from OpenWeatherMap API
def get_weather_data(api_key, city, days=7):
    # OpenWeatherMap API endpoint for historical weather data
    url = f'http://api.openweathermap.org/data/2.5/onecall?lat={city[0]}&lon={city[1]}&exclude=current,minutely,hourly&units=metric&cnt={days}&appid={api_key}'
    
    response = requests.get(url)
    
    if response.status_code == 200:
        data = response.json()
        daily_data = data['daily']  # List of daily data
        
        # Extract relevant information (temperature, date)
        dates = []
        temperatures = []
        
        for day in daily_data:
            dates.append(pd.to_datetime(day['dt'], unit='s'))
            temperatures.append(day['temp']['day'])  # Day temperature
        
        # Create a DataFrame
        weather_df = pd.DataFrame({
            'Date': dates,
            'Temperature (°C)': temperatures
        })
        
        return weather_df
    else:
        print(f"Error: {response.status_code}")
        return None

# API Key and city coordinates (e.g., London)
api_key = 'YOUR_API_KEY'  # Replace with your OpenWeatherMap API Key
city = (51.5074, -0.1278)  # Coordinates for London (Latitude, Longitude)

# Get weather data
weather_data = get_weather_data(api_key, city)

# Check if data was fetched successfully
if weather_data is not None:
    # Visualization
    sns.set(style='darkgrid')
    
    # Plotting temperature trend over the past 7 days
    plt.figure(figsize=(10, 6))
    sns.lineplot(x='Date', y='Temperature (°C)', data=weather_data, marker='o', color='b')

    # Title and labels
    plt.title(f"Temperature Trend for {city}", fontsize=14)
    plt.xlabel('Date', fontsize=12)
    plt.ylabel('Temperature (°C)', fontsize=12)
    
    # Rotate x-axis labels for better readability
    plt.xticks(rotation=45)
    
    # Show the plot
    plt.tight_layout()
    plt.show()
else:
    print("Failed to retrieve weather data.")
output:
Date	Temperature (°C)
2025-03-08	12
2025-03-09	14
2025-03-10	15
2025-03-11	13
2025-03-12	16
2025-03-13	17
2025-03-14	18
 |                           *                       
 |                        *       
 |                     *            
 |                 *         
 |              *        
 |        *            
 |    *             
 |-------------------------------  
         2025-03-08    2025-03-14
               Date        
