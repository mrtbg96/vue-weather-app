<script setup>
import axios from 'axios';
import { ref } from 'vue';

  const mapboxPublicToken = import.meta.env.VITE_MAPBPOX_PUBLIC_TOKEN;
  const searchQuery = ref("");
  const queryTimeout = ref(null);
  const mapboxSearchResults = ref(null);

  const getSearchResults = () => {
    clearTimeout(queryTimeout.value);
    queryTimeout.value = setTimeout(async () => {
      if (searchQuery.value !== "") {
        const endpoint = `https://api.mapbox.com/search/geocode/v6/forward?q=${searchQuery.value}&access_token=${mapboxPublicToken}&types=place`;
        const result = await axios.get(endpoint);
        mapboxSearchResults.value = result.data.features;
        return;
      }
      mapboxSearchResults.value = null;
    }, 300);
  };
</script>

<template>
  <section class="container text-white">
    <div class="pt-6 mb-8 relative">
      <input 
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city or state..."
        class="p-2 w-full bg-transparent border-b focus:border-weather-secondary placeholder:text-gray-300 focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      >
      <ul
        v-if="mapboxSearchResults"
        class="absolute bg-weather-secondary text-white w-full shadow-md p-2 top-[66px]"
      >
        <li 
          v-for="searchResult in mapboxSearchResults"
          :key="searchResult.id"
          class="py-2 cursor-pointer"
        >
          {{ searchResult.properties.full_address }}
        </li>
      </ul>
    </div>
  </section>
</template>
