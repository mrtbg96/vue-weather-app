<template>
    <div>
        <Suspense>
            <AsyncCityView />
            <template #fallback>
                <p class="py-12 font-semibold text-2xl text-center text-white">
                    Loading...
                </p>
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import AsyncCityView from '@/components/AsyncCityView.vue';
</script>