<template>
    <div class="flex flex-col flex-1 items-center">
        <div 
            class="text-white p-4 w-full text-center"
            :class="route.query.preview ? 'bg-weather-secondary' : 'bg-green-800'"
        >
            <p v-if="route.query.preview">
                You are currently previewing this city, click the "+" icon to start tracking this city.
            </p>
            <p v-else>
                You are currently tracking this city, click the "trash" icon to stop tracking.
            </p>
        </div>

        <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-3xl mb-2">
                {{ route.params.city }},
                {{ route.params.country }}
            </h1>
            <p class="text-lg mb-6">
                {{ weatherData.currentDateTime }}
            </p>

            <p class="text-8xl mb-4">
                {{ Math.round(weatherData.currentTemperature) }}&deg;
            </p>
            
            <p class="capitalize text-md mb-2">
                {{ weatherData.weather[0].description }}
            </p>

            <img
                class="w-[124px] mb-2"
                :src="weatherIconURL"
            />
        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const weatherIconURL = ref(null);

const getWeatherData = async () => {
    try {
        const lat = route.query.lat;
        const lng = route.query.lng;
        const openWeatherMapAPIKey = import.meta.env.VITE_OPENWEATHERMAP_API_KEY;
        const endpoint = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lng}&exclude={part}&appid=${openWeatherMapAPIKey}&units=imperial`;
        const weatherData = await axios.get(endpoint);

        const data = weatherData.data;

        const dateOptions = {
            weekday: 'short',
            month: 'short',
            day: '2-digit',
            hour: '2-digit',
            minute: '2-digit',
        };
        const date = new Intl.DateTimeFormat('us-US', dateOptions).format(new Date(data.dt * 1000));

        data.currentDateTime = date;
        data.currentTemperature = convertTemperatureFahrenheitToCelcius(data.main.temp);
        weatherIconURL.value = `https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;

        return data;
    } catch (err) {
        console.error(err);
    }
};

const convertTemperatureFahrenheitToCelcius = (fahrenheit) => {
    return ((fahrenheit - 32) * 5 / 9).toFixed(1);
};

const weatherData = await getWeatherData();
</script>