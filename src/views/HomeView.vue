<script setup>
import axios from 'axios';
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import CityList from '@/components/CityList.vue';

  const router = useRouter();

  const mapboxPublicToken = import.meta.env.VITE_MAPBPOX_PUBLIC_TOKEN;
  const searchQuery = ref("");
  const queryTimeout = ref(null);
  const mapboxSearchResults = ref(null);
  const searchError = ref(false);

  const getSearchResults = () => {
    clearTimeout(queryTimeout.value);
    queryTimeout.value = setTimeout(async () => {
      if (searchQuery.value !== "") {
        try {
          const endpoint = `https://api.mapbox.com/search/geocode/v6/forward?q=${searchQuery.value}&access_token=${mapboxPublicToken}&types=place&limit=10`;
          const result = await axios.get(endpoint);
          mapboxSearchResults.value = result.data.features;
        } catch {
          searchError.value = true;
        }
        return;
      }
      mapboxSearchResults.value = null;
    }, 300);
  };

  const previewCity = (searchResult) => {
    const countryName = searchResult.properties.context.country.name;
    const cityName = searchResult.properties.context.place.name;

    router.push({
      name: "city",
      params: { city: cityName, country: countryName },
      query: {
        lat: searchResult.geometry.coordinates[1],
        lng: searchResult.geometry.coordinates[0],
        preview: true
      }
    });
  }
</script>

<template>
  <section class="container text-white">
    <div class="pt-6 mb-8 relative">
      <input 
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city..."
        class="p-2 w-full bg-transparent border-b focus:border-weather-secondary placeholder:text-gray-300 focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      >
      <ul
        v-if="mapboxSearchResults"
        class="absolute bg-weather-secondary text-white w-full shadow-md rounded-md p-2 top-[72px]"
      >
        <p v-if="searchError">
          Sorry, something went wrong. Please try again.
        </p>
        <p v-if="!searchError && mapboxSearchResults.length === 0">
          No results found. Please search for a different city or state.
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="p-2 rounded-lg cursor-pointer hover:bg-weather-primary"
            @click="previewCity(searchResult)"
          >
            {{ searchResult.properties.full_address }}
          </li>
        </template>
      </ul>
    </div>

    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback>
          <p class="py-12 font-semibold text-2xl text-center text-white">
            Loading...
          </p>
        </template>
      </Suspense>
    </div>
  </section>
</template>
