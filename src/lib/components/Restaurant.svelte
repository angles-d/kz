<script lang="ts">
	export let res;
	import { CarIcon } from 'lucide-svelte';

	let showDelivery = false;
</script>

<div class="mb-4 rounded-xl shadow-lg p-4 bg-white border border-black flex flex-col static">
	<div class="flex items-end">
		<a href={res.website} class="text-sm font-bold capitalize pr-2 font-['Sniglet']"
			>{res.name},
		</a>
		<p class="capitalize text-sm font-['Sniglet']">{res.cuisine}</p>
		{#if res.delivery !== 'no'}
			<div class="flex flex-col items-center ml-4">
				{#if showDelivery}
					<span
						class="z-5 text-xs absolute -translate-y-3 {res.delivery == 'yes'
							? 'text-black'
							: 'text-[#a3a3a3]'}">{res.delivery == 'yes' ? 'Delivers!' : '3rd Party'}</span
					>
				{/if}
				<button
					on:click={() => {
						showDelivery = !showDelivery;
					}}
				>
					<CarIcon strokeWidth={1.25} color={res.delivery == 'yes' ? '#000' : '#a3a3a3'}
					></CarIcon></button
				>
			</div>
		{/if}
	</div>
	<a
		href="http://maps.apple.com/?address={res.address}"
		class="font-['Sniglet'] capitalize py-1 text-sm">{res.address}</a
	>
	<img
		src={res.image_url}
		alt="photo of {res.cuisine} food from {res.cuisine}"
		class="object-cover h-36 w-full"
	/>

	<div class="pt-2 pb-1 flex gap-6 text-sm items-center">
		<a href={res.menu} class="font-['Sniglet']">Menu</a>
		<a href={res.website} class="font-['Sniglet']">Website</a>
		{#if res.online_ordering}
			<a href={res.online_ordering} class="text-sm font-['Sniglet']">Order Online</a>
		{/if}
	</div>
	<a href="tel:{res.phone}" class="text-bold text-xs capitalize font-['Sniglet']">{res.phone}</a>
</div>
