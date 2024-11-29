<script lang="ts">
	export let restaurant;
</script>

<div class="mb-4 rounded-xl shadow-lg py-4 px-4 bg-white border border-black flex flex-col">
	<div class="flex items-center">
		<a href={restaurant.website} class="text-sm font-bold capitalize pr-2 font-['Sniglet']"
			>{restaurant.name},
		</a>
		<p class="capitalize text-sm font-['Sniglet']">{restaurant.cuisine}</p>
	</div>
	<a
		href="http://maps.apple.com/?address={restaurant.address}"
		class="font-['Sniglet'] capitalize py-1 text-sm">{restaurant.address}</a
	>
	<img
		src={restaurant.image_url}
		alt="photo of {restaurant.cuisine} food from {restaurant.cuisine}"
		class="object-cover h-36 w-full"
	/>

	<div class="pt-2 pb-1 flex gap-6 text-sm items-bottom">
		<a href={restaurant.menu} class="font-['Sniglet']">Menu</a>
		<a href={restaurant.website} class="font-['Sniglet']">Website</a>
		{#if restaurant.online_ordering}
			<a href={restaurant.online_ordering} class="text-sm font-['Sniglet']">Order Online</a>
		{/if}
	</div>
	<a href="tel:{restaurant.phone}" class="text-bold text-xs capitalize font-['Sniglet']"
		>{restaurant.phone}</a
	>
</div>
