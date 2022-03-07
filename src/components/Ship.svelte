<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import { onMount } from 'svelte';
	const dispatch = createEventDispatcher();

	export let ship;
	export let selected;

	let locations = [];

	onMount(() => {
		const fetchData = async () => {
			const response = await fetch(`https://meri.digitraffic.fi/api/v1/locations/latest/${ship.mmsi}`);
			const data = await response.json();
			const newLocation = data.features.map((a) => ({
				timestamp: a.properties.timestampExternal,
				geometry: a.geometry
			}))[0];
			if (locations[0]?.timestamp !== newLocation.timestamp) {
				locations = [...locations, newLocation].sort((a, b) => (a.timestamp > b.timestamp ? -1 : 1));
				console.info(`found new location for ${ship.mmsi} ${ship.name}`);
			}
			if (selected) {
				dispatch('newLocation', locations);
			}
		};
		const interval = setInterval(fetchData, 60000);
		fetchData();
		return () => clearInterval(interval);
	});
</script>

<div class="ship" class:selected on:click={() => dispatch('ship', { ship, locations })}>
	<h2>Callname {ship.callSign}</h2>
	<h3>{ship.name}</h3>
	<ul>
		<li>destination: {ship.destination}</li>
		<li>draught: {ship.draught}</li>
	</ul>
</div>

<style>
	.ship {
		background-color: white;
		padding: 0.5rem;
		margin: 0.5rem;
		border-radius: 0.5rem;
	}

	.selected {
		background-color: rgb(182, 226, 182);
	}
</style>
