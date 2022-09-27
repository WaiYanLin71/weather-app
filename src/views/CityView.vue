<template>
    <Suspense>
       <AsyncCityView/>
        <template #fallback>
            <p>loading...</p>
        </template>
    </Suspense>
</template>

<script setup>
import AsyncCityView from '../components/AsyncCityView.vue'
</script>