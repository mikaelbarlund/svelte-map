<script lang="ts">
	import ShipModal from './ShipModal.svelte';
	import Ship from './Ship.svelte';
	import Map from './Map.svelte';
	import { googleApi } from '../stores.js';
	import Details from './Details.svelte';
	let mmsi;
	let selectedShips = {};
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
			on:mmsi={(a) => {
				mmsi = a.detail;
			}}
		/>
	{/if}
	<Details {selectedShips} />
</div>

<style>
	.container {
		border-radius: 0.5rem;
		display: flex;
		background-color: rgb(170, 219, 255);
	}
</style>
