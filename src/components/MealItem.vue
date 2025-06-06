<template>
	<div class="bg-white shadow rounded-xl">
		<router-link v-if="meal && meal.idMeal" :to="{ name: 'mealDetails', params: { id: meal.idMeal } }">
			<img
				:src="meal.strMealThumb"
				:alt="meal.strMeal"
				class="rounded-t-xl w-full h-70 object-cover hover:opacity-90 transition-opacity"
			/>
		</router-link>
		<div class="p-5">
			<h3 class="font-bold">{{ meal.strMeal }}</h3>
			<p class="mb-4 line-clamp-3">
				{{ meal.strInstructions }}
			</p>
			<div class="flex items-center justify-between">
				<YouTubeButton :href="meal.strYoutube">YouTube</YouTubeButton>
			</div>
		</div>
	</div>
</template>

<script setup>
import YouTubeButton from '../components/YouTubeButton.vue';

const { meal } = defineProps({
	meal: {
		required: true,
		type: Object,
	},
});
</script>
