<script>
	import { MapLibre, Marker, Popup } from 'svelte-maplibre';

	import { CroissantIcon } from 'lucide-svelte';
	import { SoupIcon } from 'lucide-svelte';
	import { HouseIcon } from 'lucide-svelte';

	let center = [-71.10781873212616, 42.346225159981266];
	let home = [-71.1510406295893, 42.35181833496808];
	import restaurants from '../../restaurants.json';
	let selectedRes = restaurants[0];
	let show = false;
</script>

<div class="overflow-clip h-dvh">
	<MapLibre
		style="https://basemaps.cartocdn.com/gl/positron-gl-style/style.json"
		class="pt-2 w-full h-full"
		standardControls
		zoom={11.4}
		{center}
	>
		<Marker
			lngLat={[home[0], home[1]]}
			onclick={() => {
				center = home;
			}}
			class="grid h-8 w-8 place-items-center rounded-full shadow-2xl border-[1px] border-stone-600 bg-orange-100 "
			><HouseIcon strokeWidth={1} color="#000"></HouseIcon></Marker
		>
		{#each restaurants as res (res.name)}
			{@const [lat, long] = res.latlong.split(', ').map((x) => parseFloat(x))}
			<Marker
				lngLat={[long, lat]}
				onclick={() => {
					show = true;
					center = [long, lat];
					selectedRes = res;
				}}
				class="grid h-8 w-8 place-items-center rounded-full shadow-2xl border-[1px] border-stone-600
			{res.cuisine == 'bakery' ? 'bg-pink-100' : 'bg-teal-100'}"
			>
				{#if res.cuisine == 'bakery'}
					<span>
						<CroissantIcon strokeWidth={1} color="#000"></CroissantIcon>
					</span>
				{:else}
					<span>
						<SoupIcon strokeWidth={1} color="#000"></SoupIcon>
					</span>
				{/if}
			</Marker>
		{/each}
	</MapLibre>
	<div
		class="w-full absolute text-sm ease-in-out duration-300 border-t border-l border-r border-gray-800 {show
			? '-translate-y-full'
			: 'translate-y-0'} {selectedRes.cuisine == 'bakery'
			? 'bg-pink-50'
			: 'bg-teal-50'} rounded-t-xl z-10 py-4 overflow-clip px-4 shadow-md transition-transform"
	>
		{#if selectedRes}
			<div class="my-auto">
				<div class="flex items-center">
					<a href={selectedRes.website} class=" text-sm font-bold capitalize pr-0"
						>{selectedRes.name}</a
					>
					<p class="capitalize text-sm font-['Sniglet']">, {selectedRes.cuisine}</p>
				</div>
				<a
					href="http://maps.apple.com/?address={selectedRes.address}"
					class="capitalize text-xs font-[Sniglet] py-2">{selectedRes.address}</a
				>
				<a href={selectedRes.website}>
					<img
						src={selectedRes.image_url}
						alt="photo of {selectedRes.cuisine} food from {selectedRes.cuisine}"
						class="object-cover h-[16vh] rounded-lg my-1 w-full"
					/>
				</a>
				<div class="py-1">
					<a href={selectedRes.website} class=" text-sm font-[Sniglet]">Website</a>
					<a href={selectedRes.online_ordering} class=" text-sm px-4 font-[Sniglet]">Order Online</a
					>
					<a href={selectedRes.menu} class=" text-sm font-[Sniglet]">Menu</a>
				</div>
				<a href="tel:{selectedRes.phone}" class=" text-xs capitalize font-[Sniglet]"
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
