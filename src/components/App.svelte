<script lang="ts">
	import ShipModal from './ShipModal.svelte';
	import Ship from './Ship.svelte';
	import Map from './Map.svelte';
	import { googleApi } from '../stores.js';
	import Details from './Details.svelte';
	import { onMount } from 'svelte';
	let mmsi;
	let selectedShips = {};
	let allShips = [];
	let viewPortShips = [];
	const getShips = async () => {
		const response = await fetch('https://meri.digitraffic.fi/api/v1/locations/latest');
		const ships = await response.json();
		allShips = ships.features.filter((a) => a.properties.timestampExternal > Date.now() - 360000);
		viewPortShips = allShips;
	};
	onMount(async () => {
		getShips();
	});
</script>

{#if mmsi}
	<ShipModal
		{mmsi}
		on:click={() => (mmsi = undefined)}
		on:ship={(e) => {
			selectedShips[e.detail.name] = e.detail;
			selectedShips = { ...selectedShips };
		}}
	/>
{/if}
<div class="container">
	{#if $googleApi}
		<Map
			bind:features={viewPortShips}
			on:mmsi={(a) => {
				mmsi = a.detail;
			}}
		/>
	{/if}
	<Details
		{selectedShips}
		{viewPortShips}
		on:ship={(ship) => {
			if (viewPortShips.length == 1 && viewPortShips[0].mmsi === ship.detail.mmsi) viewPortShips = allShips;
			else viewPortShips = allShips.filter((a) => a.mmsi === ship.detail.mmsi);
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
