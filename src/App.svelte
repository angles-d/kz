<script lang="ts">
	import './app.css';
	import restaurants from './restaurants.json';
	import Restaurant from '$lib/components/Restaurant.svelte';
	import Map from '$lib/components/Map.svelte';
	import Switch from '$lib/components/ui/switch/switch.svelte';

	let mapView = true;
</script>

<main class="">
	<label
		class="inline-flex items-center cursor-pointer absolute left-1/2 transform -translate-x-1/2 z-10 my-4"
	>
		<input type="checkbox" bind:checked={mapView} class="sr-only peer" />
		<div
			class="relative w-1/8 h-10 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-9 after:w-9 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"
		>
			hgghghgh
		</div>
	</label>

	{#if mapView}
		<Map></Map>
	{:else}
		<div class="py-4">
			{#each restaurants as restaurant}
				<Restaurant {restaurant}></Restaurant>
			{/each}
		</div>
	{/if}
</main>
