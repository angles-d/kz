<script lang="ts">
	import Slider from './lib/components/Slider.svelte';
	import Map from '$lib/components/Map.svelte';
	import Restaurant from '$lib/components/Restaurant.svelte';
	import Typewriter from 'svelte-typewriter';
	import './app.css';
	import restaurants from './restaurants.json';
	import Button from '$lib/components/ui/button/button.svelte';
	import { ChevronDown } from 'lucide-svelte';

	let showIntro = true;
	let showMap = false;
	let showButton = false;

	function mainIntoView(id) {
		showIntro = false;

		const el = document.querySelector(`#${id}`);
		if (!el) return;
		el.scrollIntoView({
			behavior: 'smooth'
		});
	}
</script>

<!-- https://scrollrevealjs.org/ -->
<!-- https://github.com/mattboldt/typed.js -->
<!-- https://mojs.github.io/tutorials/burst/#burst-2 -->
<main class="bg-teal-50 {showIntro ? 'overflow-hidden' : ''} ">
	<div class=" min-h-dvh">
		<Typewriter
			mode={'cascade'}
			interval={50}
			delay={100}
			on:done={() => {
				showButton = true;
			}}
		>
			<div class="absolute top-1/3 mx-8">
				<h1 class=" capitalize py-1 text-3xl font-semibold">Hello Kexin Zhang!</h1>
				<p>I made this bc I didn't want support ubereats lol</p>
			</div>
		</Typewriter>
		<Button
			variant="ghost"
			class="duration-1000 transition-transform {showButton
				? '-translate-y-32'
				: 'translate-y-0'} size-24 absolute left-1/2 -translate-x-1/2 -bottom-24"
			on:click={() => {
				mainIntoView('resies');
			}}
		>
			<div class="flex flex-col items-center">
				<p>To the Restaurants</p>
				<ChevronDown size={64} strokeWidth={1} color="#111"></ChevronDown>
			</div>
		</Button>
	</div>

	<div
		class="inline-flex absolute rounded-md shadow-sm right-1/2 translate-x-1/2 z-10 my-6"
		role="group"
	>
		<button
			on:click={() => {
				showMap = false;
			}}
			type="button"
			class="font-['Sniglet'] px-6 py-2 text-md font-bold text-gray-900 rounded-s-2xl border border-black {showMap
				? ' bg-white  text-gray-900'
				: ' bg-yellow-100 text-gray-900'}"
		>
			List
		</button>

		<button
			on:click={() => {
				showMap = true;
				mainIntoView('resies');
			}}
			type="button"
			class=" font-['Sniglet'] font-bold border-b border-t border-r border-black px-6 py-2 text-sm rounded-e-2xl {!showMap
				? ' bg-white  text-gray-900'
				: ' bg-yellow-100 text-gray-900'}"
		>
			Map
		</button>
	</div>

	<div id="resies">
		{#if showMap}
			<div id="map">
				<Map></Map>
			</div>
		{:else}
			<div class="pt-20 pb-8">
				{#each restaurants as restaurant}
					<Restaurant {restaurant}></Restaurant>
				{/each}
			</div>
		{/if}
	</div>
</main>
