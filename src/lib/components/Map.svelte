<script>
	import { MapLibre, Marker, Popup } from 'svelte-maplibre';

	import { CroissantIcon } from 'lucide-svelte';
	import { SoupIcon } from 'lucide-svelte';

	let center = [-71.1256, 42.3564];
	import restaurants from '../../restaurants.json';
	let selectedRes = restaurants[0];
	let show = false;
</script>

<div class="overflow-clip h-screen">
	<MapLibre
		style="https://basemaps.cartocdn.com/gl/positron-gl-style/style.json"
		class="pt-2 w-full h-full"
		standardControls
		zoom={13}
		{center}
	>
		{#each restaurants as res (res.name)}
			{console.log(res.latlong)}
			{@const [lat, long] = res.latlong.split(', ').map((x) => parseFloat(x))}
			<Marker
				lngLat={[long, lat]}
				onclick={() => {
					show = true;
					center = [long, lat];
					selectedRes = res;
				}}
				class="grid h-8 w-8 place-items-center rounded-full shadow-2xl
			bg-teal-700"
			>
				<span>
					<SoupIcon strokeWidth={1.5} color="#fff"></SoupIcon>
				</span>
			</Marker>
		{/each}
	</MapLibre>
	<div
		class="w-full absolute h-1/3 text-sm ease-in-out duration-300 border border-gray-800 {show
			? '-translate-y-full'
			: 'translate-y-0'}  bg-teal-50 rounded-xl z-10 py-4 overflow-clip px-4 shadow-md"
	>
		{#if selectedRes}
			<div class="my-auto">
				<div class="flex items-center">
					<a href={selectedRes.website} class="hover:underline text-sm font-bold capitalize pr-0"
						>{selectedRes.name}</a
					>
					<p class="capitalize text-sm">, {selectedRes.cuisine}</p>
				</div>
				<a
					href="http://maps.apple.com/?address={selectedRes.address}"
					class="capitalize text-xs py-2">{selectedRes.address}</a
				>
				<img
					src={selectedRes.image_url}
					alt="photo of {selectedRes.cuisine} food from {selectedRes.cuisine}"
					class="object-cover h-[16vh] rounded-lg my-1 w-full"
				/>

				<div class="py-1">
					<a href={selectedRes.website} class="hover:underline text-sm">Website</a>
					<a href={selectedRes.online_ordering} class="hover:underline text-sm px-4">Order Online</a
					>
					<a href={selectedRes.menu} class="hover:underline text-sm">Menu</a>
				</div>
				<a href="tel:{selectedRes.phone}" class="hover:underline text-xs capitalize pr-0"
					>{selectedRes.phone}</a
				>
			</div>
		{/if}
	</div>
</div>

<style>
	:global(.map) {
		height: 500px;
	}
</style>
