<template>
    <div>
        <div v-for="city in saveCities" :key="city.id" class="mb-3">
            <CityCard :city="city" @click="goToCityView(city)"/>
        </div>
    </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import CityCard from './CityCard.vue'
import {useRouter} from 'vue-router'

const router =useRouter();
const saveCities = ref([]);
const getCities = async () => {
	if (localStorage.getItem("saveCities")) {
		saveCities.value = JSON.parse(localStorage.getItem("saveCities"));
	}

	const requests = [];
	saveCities.value.forEach((city) => {
		requests.push(
			axios.get(
				`https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=imperial`
			)
		);
	});
    const weatherData = await  Promise.all(requests)

    weatherData.forEach((value,index) => {
        saveCities.value[index].weather = value.data
    })
};

await getCities();

   const goToCityView = (city) => {

        router.push({
            name:"cityView",
            params:{state:city.state,city:city.city},
            query:{id:city.id, lat:city.coords.lat,lng:city.coords.lng}
        })
    }
</script>

<style scoped></style>
