<template>
	<div class="p-8 bg-0">
		<input
			type="text"
			v-model="keyword"
			class="rounded border-2 bg-white border-gray-200 w-full p-4"
			placeholder="Search for Meals"
			@change="searchMeals"
		/>
	</div>

	<Meals :meals="meals" />
</template>

<script setup>
import { computed, onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import { useStore } from 'vuex';
import Meals from '../components/Meals.vue';

const route = useRoute();
const store = useStore();
const keyword = ref('');
const meals = computed(() => store.state.searchedMeals);

function searchMeals() {
	if (keyword.value) {
		store.dispatch('searchMeals', keyword.value);
	} else {
		store.commit('setSearchedMeals', []);
	}
}

onMounted(() => {
	keyword.value = route.params.name;
	if (keyword.value) {
		searchMeals();
	}
});
</script>
