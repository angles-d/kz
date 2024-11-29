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

	let filter = 'all';

	function mainIntoView(id) {
		showIntro = false;

		const el = document.querySelector(`#${id}`);
		if (!el) return;
		el.scrollIntoView({
			behavior: 'smooth'
		});
	}
	$: console.log(filter);

	let resFilters = ['all', ...restaurants.reduce((acc, r) => acc.add(r.cuisine), new Set())];
</script>

<main class="bg-teal-50 {showIntro ? 'overflow-hidden' : ''} ">
	<div class="h-screen overflow-hidden">
		<Typewriter
			mode={'cascade'}
			interval={30}
			delay={100}
			on:done={() => {
				showButton = true;
			}}
		>
			<div class="absolute top-[15vh] mx-8">
				<h1 class=" capitalize py-4 text-3xl font-semibold">Hello Kexin Zhang!</h1>
				<!-- <p class="whitespace-break-spaces">{' '}</p>
				<p>
					I was originally going to get you a gift card for food delivery since you need tasty
					convenient brain fuel for your art doctor dreams, but all the delivery services are
					actually so bad for restaurants...
				</p>
				<p class="whitespace-break-spaces">{' '}</p>
				<p>So I made this bc I didn't want support ubereats lol</p>
				<p class="whitespace-break-spaces">{' '}</p>
				<p>
					Turns out your local restaurants do not have their own delivery, so this is more like a
					general list of places near you but I tried.
				</p>
				<p class="whitespace-break-spaces">{' '}</p>
				<p>Hope you like the restaurants!</p> -->
			</div>
		</Typewriter>
		<button
			class="duration-1000 transition-transform {showButton
				? '-translate-y-32'
				: 'translate-y-0'}  absolute left-1/2 -translate-x-1/2 top-full"
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
			class="font-['Sniglet'] py-2 text-md font-bold rounded-s-lg border border-black {showMap
				? ' bg-white w-[30vw]'
				: ' bg-yellow-100 w-[43vw]'} duration-300"
		>
			List
		</button>

		<button
			on:click={() => {
				showMap = true;
				mainIntoView('resies');
			}}
			type="button"
			class=" font-['Sniglet'] font-bold border-b border-t border-r border-black px-16 py-2 text-sm rounded-e-lg {!showMap
				? ' bg-white w-[43vw]'
				: ' bg-yellow-100 w-[30vw]'} duration-300"
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
			<div class="mx-[7vw] pt-20 pb-8">
				<div class="my-2">
					<DropdownMenu.Root>
						<DropdownMenu.Trigger asChild let:builder>
							<Button
								variant="outline"
								builders={[builder]}
								class="bg-purple-100 border font-['Sniglet']  border-black w-[33vw] capitalize"
								>{filter}</Button
							>
						</DropdownMenu.Trigger>
						<DropdownMenu.Content class="w-[33vw]">
							{#each resFilters as res}
								<DropdownMenu.Item
									on:click={() => {
										filter = res;
									}}
									class="capitalize font-['Sniglet'] ">{res}</DropdownMenu.Item
								>
							{/each}
						</DropdownMenu.Content>
					</DropdownMenu.Root>
				</div>
				{#each restaurants.filter((r) => filter == 'all' || r.cuisine == filter) as restaurant}
					<Restaurant {restaurant}></Restaurant>
				{/each}
			</div>
		{/if}
	</div>
</main>
