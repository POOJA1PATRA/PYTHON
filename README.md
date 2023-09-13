# PYTHON
Assignment 2:REST API -PYTHON FRAMEWORK
import requests

# API URL
api_url = "https://samples.openweathermap.org/data/2.5/forecast/hourly"

# API key (you can replace it with your own key if needed)
api_key = "b6907d289e10d714a6e88b30761fae22"

def get_weather_data(city):
    # Prepare the request URL
    url = f"{api_url}?q={city}&appid={api_key}"

    # Send a GET request to the API
    response = requests.get(url)

    # Check if the request was successful
    if response.status_code == 200:
        return response.json()
    else:
        print("Failed to fetch weather data.")
        return None

def get_temperature(data, date_time):
    for forecast in data['list']:
        if forecast['dt_txt'] == date_time:
            return forecast['main']['temp']
    return None

def get_wind_speed(data, date_time):
    for forecast in data['list']:
        if forecast['dt_txt'] == date_time:
            return forecast['wind']['speed']
    return None

def get_pressure(data, date_time):
    for forecast in data['list']:
        if forecast['dt_txt'] == date_time:
            return forecast['main']['pressure']
    return None

if __name__ == "__main__":
    city = "London,us"  # Change to the desired location
    weather_data = get_weather_data(city)

    if weather_data is not None:
        while True:
            print("\nOptions:")
            print("1. Get Temperature")
            print("2. Get Wind Speed")
            print("3. Get Pressure")
            print("0. Exit")

            option = input("Enter your choice: ")

            if option == "1":
                date_time = input("Enter date and time (YYYY-MM-DD HH:MM:SS): ")
                temperature = get_temperature(weather_data, date_time)
                if temperature is not None:
                    print(f"Temperature at {date_time}: {temperature} K")
                else:
                    print("Data not found for the specified date and time.")

            elif option == "2":
                date_time = input("Enter date and time (YYYY-MM-DD HH:MM:SS): ")
                wind_speed = get_wind_speed(weather_data, date_time)
                if wind_speed is not None:
                    print(f"Wind Speed at {date_time}: {wind_speed} m/s")
                else:
                    print("Data not found for the specified date and time.")

            elif option == "3":
                date_time = input("Enter date and time (YYYY-MM-DD HH:MM:SS): ")
                pressure = get_pressure(weather_data, date_time)
                if pressure is not None:
                    print(f"Pressure at {date_time}: {pressure} hPa")
                else:
                    print("Data not found for the specified date and time.")

            elif option == "0":
                break

            else:
                print("Invalid option. Please try again.")
