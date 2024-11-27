<script lang="ts">
	import './app.css';
	import restaurants from './restaurants.json';
	import Restaurant from '$lib/components/Restaurant.svelte';
	import Map from '$lib/components/Map.svelte';

	let mapView = false;
</script>

<main class="bg-teal-50">
	<div
		class="inline-flex absolute rounded-md shadow-sm right-1/2 translate-x-1/2 z-10 my-6"
		role="group"
	>
		<button
			on:click={() => {
				mapView = false;
			}}
			type="button"
			class="font-['Sniglet'] px-6 py-2 text-md font-bold text-gray-900 rounded-s-2xl border border-black {mapView
				? ' bg-white  text-gray-900'
				: ' bg-yellow-100 text-gray-900'}"
		>
			List
		</button>

		<button
			on:click={() => {
				mapView = true;
			}}
			type="button"
			class=" font-['Sniglet'] font-bold border-b border-t border-r border-black px-6 py-2 text-sm rounded-e-2xl {!mapView
				? ' bg-white  text-gray-900'
				: ' bg-yellow-100 text-gray-900'}"
		>
			Map
		</button>
	</div>

	{#if mapView}
		<Map></Map>
	{:else}
		<div class="pt-20 pb-8">
			{#each restaurants as restaurant}
				<Restaurant {restaurant}></Restaurant>
			{/each}
		</div>
	{/if}
</main>
