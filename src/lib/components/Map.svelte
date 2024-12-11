<script>
	import { MapLibre, Marker, Popup } from 'svelte-maplibre';

	import { CroissantIcon } from 'lucide-svelte';
	import { SoupIcon } from 'lucide-svelte';
	import { HouseIcon } from 'lucide-svelte';
	import { ShellIcon } from 'lucide-svelte';
	import 'material-icons/iconfont/outlined.css';

	let center = [-71.11623808310333, 42.34947224559537];
	let home = [-71.13600574636236, 42.36108290934698];
	import restaurants from '../../restaurants.json';
	let selectedRes = restaurants[0];
	let show = false;

	const sweet = new Set(['sweet', 'bakery', 'cafe']);
</script>

<div class="overflow-clip h-dvh">
	<MapLibre
		style="https://basemaps.cartocdn.com/gl/positron-gl-style/style.json"
		class="pt-2 w-full h-full"
		standardControls
		zoom={12}
		{center}
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
				class="grid size-9 place-items-center rounded-full shadow-2xl border-[1px] border-stone-600
			{sweet.has(res.type) ? 'bg-pink-100' : 'bg-teal-100'}"
			>
				{#if res.type == 'bakery'}
					<span class="yummy-icon yi-141 fill-black text-[20px]"></span>
				{:else if res.type == 'sweet'}
					<ShellIcon strokeWidth={1} color="#000"></ShellIcon>
				{:else if res.type == 'noodle'}
					<span class="yummy-icon yi-181 fill-black text-[24px]"></span>
				{:else if res.type == 'korean'}
					<span class="yummy-icon yi-047 fill-black text-[22px]"></span>
				{:else if res.type == 'dumpling'}
					<span class="yummy-icon yi-238 fill-black rotate-180 text-[20px]"></span>
				{:else if res.type == 'cafe'}
					<span class="yummy-icon yi-006 fill-black text-[22px]"></span>
				{:else}
					<span class="yummy-icon yi-055 fill-black text-[22px]"></span>
				{/if}
			</Marker>
		{/each}

		<Marker
			lngLat={[home[0], home[1]]}
			onclick={() => {
				center = home;
			}}
			class="grid size-9 place-items-center rounded-full shadow-2xl border-[1px] border-stone-600 bg-orange-100 "
			><HouseIcon strokeWidth={1} color="#000"></HouseIcon></Marker
		>
	</MapLibre>
	<div
		class="w-full absolute text-sm ease-in-out duration-300 border-t border-l border-r border-gray-800 {show
			? '-translate-y-full'
			: 'translate-y-0'} {sweet.has(selectedRes.type)
			? 'bg-pink-50'
			: 'bg-teal-50'} rounded-t-xl z-10 py-4 overflow-clip px-4 shadow-md transition-transform"
	>
		{#if selectedRes}
			<div class="my-auto">
				<div class="flex items-center">
					<a href={selectedRes.website} class=" text-sm font-['Sniglet'] font-bold capitalize pr-0"
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
				<div class="py-1 flex gap-7">
					<a href={selectedRes.website} class=" text-sm font-[Sniglet]">Website</a>
					<a href={selectedRes.menu} class=" text-sm font-[Sniglet]">Menu</a>
					{#if selectedRes.online_ordering}
						<a href={selectedRes.online_ordering} class="text-sm font-['Sniglet']">Order Online</a>
					{/if}
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
