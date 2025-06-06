<template>
	<Meals :meals="meals" />
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
import { useStore } from 'vuex';
import { useRoute } from 'vue-router';
import Meals from '../components/Meals.vue';
import axiosClient from '../axiosClient';

const store = useStore();
const route = useRoute();

const meals = ref([]);
const loading = ref(false);

async function fetchFullMeals() {
	loading.value = true;
	const baseMeals = store.state.mealsByIngredient || [];
	if (!baseMeals.length) {
		meals.value = [];
		loading.value = false;
		return;
	}

	const promises = baseMeals.map((meal) =>
		axiosClient.get(`lookup.php?i=${meal.idMeal}`).then((res) => res.data.meals[0])
	);
	meals.value = await Promise.all(promises);
	loading.value = false;
}

onMounted(async () => {
	await store.dispatch('searchMealsByIngredient', route.params.ingredient);
	await fetchFullMeals();
});

watch(
	() => route.params.ingredient,
	async (newIngredient) => {
		await store.dispatch('searchMealsByIngredient', newIngredient);
		await fetchFullMeals();
	}
);
</script>
