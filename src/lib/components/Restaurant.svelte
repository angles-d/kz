<script lang="ts">
	export let restaurant;
	import Button from './ui/button/button.svelte';
</script>

<div class="mb-6 mx-4">
	<div class="flex items-center py-0">
		<a href={restaurant.website} class="hover:underline text-sm px-4 font-bold capitalize pr-0"
			>{restaurant.name}</a
		>
		<p class="capitalize text-sm">, {restaurant.cuisine}</p>
	</div>
	<Button
		href="http://maps.apple.com/?address={restaurant.address}"
		variant="link"
		class="capitalize py-0">{restaurant.address}</Button
	>
	<img
		src={restaurant.image_url}
		alt="photo of {restaurant.cuisine} food from {restaurant.cuisine}"
		class="px-4 object-cover h-36 w-full"
	/>

	<div>
		<Button href={restaurant.website} variant="link">Website</Button>
		<Button class="py-0" href={restaurant.online_ordering} variant="link">Order Online</Button>
		<Button class="py-0" href={restaurant.menu} variant="link">Menu</Button>
	</div>
	<a href="tel:{restaurant.phone}" class="hover:underline text-xs px-4 capitalize pr-0"
		>{restaurant.phone}</a
	>
</div>
