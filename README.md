# api_and_requests.py
Assignment 4 API and Requests


""""
NASA APOD API Data Retrieval Script

Description:
    This script retrieves Astonomy Picure of the Day (APOD) data from NASA's public API and saves it into a CSV file.
"""

import requests
import pandas as pd

API_KEY = "DEMO_KEY"        #public demo key (limited rate usage)
BASE_URL = "https://api.nasa.gov/planetary/apod"

params = {                  #Setting parameters for the API request
    "api_key": API_KEY,
    "count": 5}             #Get 5 random APOD entries       

try:
    response = requests.get(BASE_URL, params=params)
    response.raise_for_status()  #Raises error for bad status codes
    
    print("Request Successful!")

except requests.exceptions.RequestException as e:
    print("Error making request:")
    exit()

data = response.json()

df = pd.DataFrame(data)         #Converts JSON list into DataFrame

df = df[[
    "date",
    "title",
    "explanation",
    "media__type",
    "url"]]

output_file = "nasa_apod_data.csv"          #Saves data to CSV
df.to_csv(output_file, index=False)

print(f"Data saved to {output_file}")
print(df.head())
