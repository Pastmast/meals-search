<template>
	<div class="grid grid-cols-1 md:grid-cols-3 gap-5 p-8" v-if="meals.length">
		<MealItem v-for="meal of meals" :key="meal.idMeal" :meal="meal" />
	</div>
	<div v-else class="flex justify-center text-gray-600">There are no meals</div>
</template>

<script setup>
import MealItem from './MealItem.vue';

const { meals } = defineProps({
	meals: {
		required: true,
		type: Array,
	},
});
</script>
