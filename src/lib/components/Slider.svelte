<script lang="ts">
	import '../../app.css';

	export let mapView = false;
	export let mainIntoView;
</script>

<div
	class="inline-flex absolute rounded-md shadow-sm right-1/2 translate-x-1/2 z-10 my-6"
	role="group"
>
	<button
		on:click={() => {
			mapView = false;
			mainIntoView('map');
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
