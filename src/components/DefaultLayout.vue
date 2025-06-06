<template>
	<div class="bg-gray-100 h-full">
		<navbar />
		<main class="bg-gray-100">
			<div class="max-w-[1800px] mx-auto w-full">
				<router-view />
			</div>
		</main>
	</div>
</template>

<script setup>
import Navbar from '../components/Navbar.vue';
</script>
