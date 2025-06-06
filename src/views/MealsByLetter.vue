<template>
	<div class="flex justify-center gap-2 mt-2">
		<router-link :to="{ name: 'byLetter', params: { letter } }" v-for="letter of letters" :key="letter">
			{{ letter }}
		</router-link>
	</div>
	<Meals v-if="Array.isArray(meals)" :meals="meals" />
	<div v-else class="p-20 flex justify-center text-gray-600">There are no meals</div>
</template>

<script setup>
import { computed, onMounted, watch } from 'vue';
import { useRoute } from 'vue-router';
import { useStore } from 'vuex';
import Meals from '../components/Meals.vue';

const store = useStore();
const route = useRoute();

const letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('');
const meals = computed(() => store.state.mealsByLetter);

watch(
	() => route.params.letter,
	(newLetter) => {
		store.dispatch('searchMealsByLetter', newLetter);
	}
);

onMounted(() => {
	store.dispatch('searchMealsByLetter', route.params.letter);
});
</script>
