<template>
	<header class="sticky top-0 bg-weather-primary shadow-lg">
		<nav
			class="container flex flex-cold sm:flex-row items-center gap-4 text-white py-6"
		>
			<RouterLink :to="{ name: 'home' }">
				<div class="flex items-center gap-3 flex-1">
					<em class="fa-regular fa-sun text-2xl"></em>
					<p class="text-2xl">The Local Weather</p>
				</div>
			</RouterLink>

			<div class="flex gap-3 flex-1 justify-end">
				<em
					class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
					@click="toggleModal"
				></em>
				<em	
					v-if="route.query.preview"
					class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"
					@click="addCity"
				></em>
			</div>
			<BaseModal :modalActive="modalActive" @close-modal="toggleModal">
				<div class="text-black">
					<h1 class="text-2xl mb-1">About:</h1>
					<p class="mb-4">
						The Local Weather allows you to track the current and
						future weather of cities of your choosing.
					</p>
					<h2 class="text-2xl">How it works:</h2>
					<ol class="list-decimal list-inside mb-4">
						<li>
							Search for your city by entering the name into the
							search bar.
						</li>
						<li>
							Select a city within the results, this will take you
							to the current weather for your selection.
						</li>
						<li>
							Track the city by clicking on the "+" icon in the
							top right. This will save the city to view at a
							later time on the home page.
						</li>
					</ol>

					<h2 class="text-2xl">Removing a city</h2>
					<p>
						If you no longer wish to track a city, simply select the
						city within the home page. At the bottom of the page,
						there will be am option to delete the city.
					</p>
				</div>
			</BaseModal>
		</nav>
	</header>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import { RouterLink, useRoute,useRouter } from "vue-router";
import BaseModal from "./BaseModal.vue";
import { uid } from "uid";

const route = useRoute();
const router = useRouter();
const modalActive = ref(null);
const saveCities = ref([]);

const addCity = () => {
	if (localStorage.getItem("saveCities")) {
		saveCities.value = JSON.parse(localStorage.getItem("saveCities"));
	}

	const locationObj = {
		id: uid(),
		state: route.params.state,
		city: route.params.city,
		coords: {
			lat: route.query.lat,
			lng: route.query.lng,
		},
	};

	saveCities.value.push(locationObj);
	localStorage.setItem("saveCities",JSON.stringify(saveCities.value))

	let query = Object.assign({},route.query)
	delete query.preview
	query.id = locationObj.id
	router.replace({query})
};

const toggleModal = () => {
	modalActive.value = !modalActive.value;
};
</script>
