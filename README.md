# Find-weather-by-city-name
##you can find over 2 lakhs city's weather details 
import streamlit as st
import requests
API_KEY = "insert your API KEY here"
def convert_to_celcius(tempreature_in_kelvin):
    return tempreature_in_kelvin - 273.15



def find_current_weather(city):
    base_url = f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={API_KEY}"
    weather_data = requests.get(base_url).json()
    
    try:
        general = weather_data['weather'][0]['main']
        icon_id = weather_data['weather'][0]['icon']
        temperature = round(convert_to_celcius(weather_data['main']['temp']))
        icon = f"http://openweathermap.org/img/wn/{icon_id}@2x.png"




    except KeyError:
        st.error("city not found")
        st.stop()
    return general,temperature,icon


def main():
    st.header("FIND THE WEATHER")
    city = st.text_input('enter the city name').lower()
    if st.button("Find"):
        general,tempreature,icon = find_current_weather(city)
        col_1,col_2 = st.columns(2)
        with col_1 :
            st.metric(label="Temperature", value= f"{tempreature} °c")
        with col_2:
            st.write(general)
            st.image(icon)
    

if __name__ == '__main__':
    main()
