import requests

def get_weather(city, api_key):
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}&units=metric"
    response = requests.get(url)
    return response.json()

def get_clothing_suggestion(temp, weather):
    weather = weather.lower()

    if temp < 15:
        base = "Wear a warm jacket, full sleeves, and possibly gloves."
    elif 15 <= temp < 25:
        base = "Wear a light jacket or hoodie with comfortable jeans."
    elif 25 <= temp < 35:
        base = "Wear cotton shirts, light pants, or casual outfits."
    else:
        base = "Wear very light cotton clothes, shorts, and stay hydrated."

    if "rain" in weather:
        acc = "Carry an umbrella or raincoat."
    elif "clear" in weather or "sun" in weather:
        acc = "Use sunscreen and sunglasses."
    elif "cloud" in weather:
        acc = "Weather may change, keep a light jacket."
    elif "snow" in weather:
        acc = "Wear thermal clothes, gloves, and a wool cap."
    else:
        acc = "No special accessories needed."

    return base + "\n" + acc

print("🌤 Real-Time Weather-Based Clothing Suggestion System 🌤")
city = input("Enter your city name: ")

api_key = "5872300c316b11517537df48d4a5907d"

data = get_weather(city, api_key)

if data.get('cod') != 200:
    print("Error: Could not fetch weather data. Check city name or API key.")
else:
    temp = data['main']['temp']
    weather = data['weather'][0]['description']

    print("\n--- Current Weather ---")
    print(f"City: {city}")
    print(f"Temperature: {temp}°C")
    print(f"Condition: {weather}")

    print("\n--- Clothing Suggestions ---")
    print(get_clothing_suggestion(temp, weather))
# weather-clothing-suggestion
