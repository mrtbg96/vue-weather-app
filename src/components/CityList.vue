<template>
    <h1 class="text-xl">Tracked Cities ({{ trackedCities.length }})</h1>

    <div 
        v-for="city in trackedCities"
        :key="city.id"
    >
        <CityCard
            :city="city"
            @click="viewCity(city)"
        />
    </div>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';
import CityCard from './CityCard.vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const trackedCities = ref([]);
const openWeatherMapAPIKey = import.meta.env.VITE_OPENWEATHERMAP_API_KEY;

const getTrackedCities = async () => {
    if (localStorage.getItem('trackedCities')) {
        trackedCities.value = JSON.parse(localStorage.getItem('trackedCities'));
    }

    const requests = [];
    trackedCities.value.forEach((city) => {
        const lat = city.coords.lat;
        const lng = city.coords.lng;

        const endpoint = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lng}&exclude={part}&appid=${openWeatherMapAPIKey}&units=imperial`
        requests.push(axios.get(endpoint));
    });

    const weatherData = await Promise.all(requests);

    weatherData.forEach((value, index) => {
        trackedCities.value[index].weather = value.data;
    });
};

await getTrackedCities();

const viewCity = (city) => {
    router.push({
        name: 'city',
        params: {
            city: city.city,
            country: city.country
        },
        query: {
            id: city.id,
            lat: city.coords.lat,
            lng: city.coords.lng,
        }
    });
};
</script>