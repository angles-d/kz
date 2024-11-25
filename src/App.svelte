<script lang="ts">
	import './app.css';
	import restaurants from './restaurants.json';
	import Restaurant from '$lib/components/Restaurant.svelte';
</script>

<main class="my-4">
	{#each restaurants as restaurant}
		<Restaurant {restaurant}></Restaurant>
	{/each}
</main>
