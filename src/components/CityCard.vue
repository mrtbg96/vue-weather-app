<template>
    <div class="flex p-4 bg-weather-secondary rounded-md shadow-md cursor-pointer">
        <div class="flex flex-col flex-1 space-y-2">
            <h1 class="text-2xl">
                {{ city.city }}
            </h1>
            <p class="text-sm">
                {{ city.country }}
            </p>
        </div>

        <div class="flex flex-col gap-2">
            <p class="text-3xl self-end">
                {{ Math.round(convertTemperatureFahrenheitToCelcius(city.weather.main.temp)) }}&deg;
            </p>
            <div class="flex gap-2">
                <span class="text-xs">
                    Max:
                    {{ Math.round(convertTemperatureFahrenheitToCelcius(city.weather.main.temp_max)) }}&deg;
                </span>
                <span class="text-xs">
                    Min:
                    {{ Math.round(convertTemperatureFahrenheitToCelcius(city.weather.main.temp_min)) }}&deg;
                </span>
            </div>
        </div>
    </div>
</template>

<script setup>
defineProps({
    city: {
        type: Object,
        default: () => {}
    }
});

const convertTemperatureFahrenheitToCelcius = (fahrenheit) => {
    return ((fahrenheit - 32) * 5 / 9).toFixed(1);
};
</script>