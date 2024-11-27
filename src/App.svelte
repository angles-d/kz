<script lang="ts">
	import Map from '$lib/components/Map.svelte';
	import Restaurant from '$lib/components/Restaurant.svelte';
	import Button from '$lib/components/ui/button/button.svelte';
	import * as DropdownMenu from '$lib/components/ui/dropdown-menu/index.js';
	import { ChevronDown } from 'lucide-svelte';
	import Typewriter from 'svelte-typewriter';
	import './app.css';
	import restaurants from './restaurants.json';

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

	let resFilters = [...restaurants.reduce((acc, r) => acc.add(r.cuisine), new Set())];
</script>

<main class="bg-teal-50 {showIntro ? 'overflow-hidden' : ''} ">
	<div class="h-screen">
		<Typewriter
			mode={'cascade'}
			interval={50}
			delay={100}
			on:done={() => {
				showButton = true;
			}}
		>
			<div class="absolute top-1/3 mx-8">
				<h1 class=" capitalize py-4 text-3xl font-semibold">Hello Kexin Zhang!</h1>
				<p>I made this bc I didn't want support ubereats lol</p>
				<p class="whitespace-break-spaces">{' '}</p>
				<p>Hope you like the restaurants!</p>
			</div>
		</Typewriter>
		<button
			class="duration-1000 transition-transform {showButton
				? '-translate-y-32'
				: 'translate-y-0'}  absolute left-1/2 -translate-x-1/2 -bottom-24"
			on:click={() => {
				mainIntoView('resies');
			}}
		>
			<div class="flex flex-col items-center">
				<p>To the Restaurants</p>
				<ChevronDown size={64} strokeWidth={1} color="#111"></ChevronDown>
			</div>
		</button>
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
			class="font-['Sniglet'] px-16 py-2 text-md font-bold rounded-s-xl border border-black {showMap
				? ' bg-white  '
				: ' bg-yellow-100'}"
		>
			List
		</button>

		<button
			on:click={() => {
				showMap = true;
				mainIntoView('resies');
			}}
			type="button"
			class=" font-['Sniglet'] font-bold border-b border-t border-r border-black px-16 py-2 text-sm rounded-e-xl {!showMap
				? ' bg-white '
				: ' bg-yellow-100 '}"
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
			<div class="mx-6 pt-20 pb-8">
				<div class="my-2">
					<DropdownMenu.Root>
						<DropdownMenu.Trigger asChild let:builder>
							<Button
								variant="outline"
								builders={[builder]}
								class="bg-purple-100 border border-black w-32">Filter</Button
							>
						</DropdownMenu.Trigger>
						<DropdownMenu.Content class="w-32">
							{#each resFilters as res}
								<DropdownMenu.Item class="capitalize">{res}</DropdownMenu.Item>
							{/each}
						</DropdownMenu.Content>
					</DropdownMenu.Root>
				</div>
				{#each restaurants as restaurant}
					<Restaurant {restaurant}></Restaurant>
				{/each}
			</div>
		{/if}
	</div>
</main>
