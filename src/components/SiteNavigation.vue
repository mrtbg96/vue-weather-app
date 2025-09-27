<template>
    <header class="sticky top-0 bg-weather-primary shadow">
        <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-4">
            <RouterLink :to="{ name: 'home' }">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-sun text-xl"></i>
                    <p class="text-xl">Vue Weather App</p>
                </div>
            </RouterLink>

            <div class="flex justify-end flex-1 gap-3">
                <i 
                    class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer text-white"
                    @click="toggleModal"
                ></i>
                <i class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer text-white"></i>
            </div>

            <BaseModal 
                :modalActive="modalActive"
                @close-modal="toggleModal"
            >
                <div class="text-black">
                    <h1 class="font-semibold text-xl text-weather-primary mb-1">About</h1>
                    <p class="text-md mb-4">
                        The Local Weather allows you to track the current and
                        future weather of cities of your choosing.
                    </p>

                    <h1 class="font-semibold text-xl text-weather-primary mb-1">How it works</h1>
                    <ol class="list-decimal list-inside text-md mb-4">
                        <li>
                            Search for your city by entering the name into thesearch bar.
                        </li>
                        <li>
                            Select a city within the results, this will take you to the current weather for your selection.
                        </li>
                        <li>
                            Track the city by clicking on the "+" icon in the top right. 
                            This will save the city to view at a later time on the home page.
                        </li>
                    </ol>

                    <h2 class="font-semibold text-xl text-weather-primary mb-1">Removing a city</h2>
                    <p class="text-md">
                        If you no longer wish to track a city, simply select the city within the home page. 
                        At the bottom of the page, there will be am option to delete the city.
                    </p>
                </div>
            </BaseModal>
        </nav>
    </header>
</template>

<script setup>
import { RouterLink } from 'vue-router';
import BaseModal from './BaseModal.vue';
import { ref } from 'vue';

const modalActive = ref(null);
const toggleModal = () => {
    modalActive.value = !modalActive.value;
};
</script>