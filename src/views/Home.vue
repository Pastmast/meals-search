<template>
	<div class="flex flex-col p-8">
		<h1 class="text-3xl font-bold mb-6 text-orange-500">Random Meals</h1>
		<div v-if="loading" class="text-center">Loading...</div>
		<Meals v-else :meals="meals" />
	</div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axiosClient from '../axiosClient.js';
import Meals from '../components/Meals.vue';

const meals = ref([]);
const loading = ref(true);

async function fetchRandomMeals(count = 9) {
	loading.value = true;
	const promises = [];
	for (let i = 0; i < count; i++) {
		promises.push(axiosClient.get('random.php').then((res) => res.data.meals[0]));
	}

	const results = await Promise.all(promises);
	const uniqueMeals = [];
	const ids = new Set();

	for (const meal of results) {
		if (!ids.has(meal.idMeal)) {
			uniqueMeals.push(meal);
			ids.add(meal.idMeal);
		}
		if (uniqueMeals.length === count) break;
	}
	meals.value = uniqueMeals;
	loading.value = false;
}

onMounted(() => {
	fetchRandomMeals();
});
</script>
