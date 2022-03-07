<script lang="ts">
	import Map from './Map.svelte';
	import { googleApi } from '../stores.js';
	import Details from './Details.svelte';
	import { onMount } from 'svelte';

	let center = { lat: 60.192059, lng: 24.945831 };
	let selectedShips = {};
	let allShips = [];
	let viewPortShips = [];
	let track = [];
	const getShips = async () => {
		const response = await fetch('https://meri.digitraffic.fi/api/v1/locations/latest');
		const ships = await response.json();
		allShips = ships.features.filter((a) => a.properties.timestampExternal > Date.now() - 360000);
		viewPortShips = allShips;
	};
	onMount(async () => {
		getShips();
	});
	const onMmsi = async (e) => {
		const mmsi = e.detail;
		const response = await fetch(`https://meri.digitraffic.fi/api/v1/metadata/vessels/${mmsi}`);
		const data = await response.json();
		selectedShips[data.name] = data;
		selectedShips = { ...selectedShips };
	};
</script>

<div class="container">
	{#if $googleApi}
		<Map bind:center bind:viewPortShips bind:track on:mmsi={onMmsi} />
	{/if}
	<Details
		{selectedShips}
		{viewPortShips}
		on:ship={(e) => {
			if (viewPortShips.length === 1 && viewPortShips[0].mmsi === e.detail.ship.mmsi) {
				viewPortShips = allShips;
				track = [];
			} else {
				viewPortShips = [];
				track = e.detail.locations;
				const [lng, lat] = track[0].geometry.coordinates;
				center = { lat, lng };
			}
		}}
		on:newLocation={(a) => {
			track = a.detail;
		}}
		on:remove={(a) => {
			delete selectedShips[a.detail.name];
			selectedShips = selectedShips;
			viewPortShips = allShips;
			track = [];
		}}
	/>
</div>

<style>
	.container {
		border-radius: 0.5rem;
		display: flex;
		background-color: rgb(170, 219, 255);
	}
</style>
