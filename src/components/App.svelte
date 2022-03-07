<script lang="ts">
	import ShipModal from './ShipModal.svelte';
	import Ship from './Ship.svelte';
	import Map from './Map.svelte';
	import { googleApi } from '../stores.js';
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
	<div class="details element">
		<div>
			{#each Object.keys(selectedShips).sort((a, b) => a.localeCompare(b)) as selectedShip (selectedShip)}
				<div class="ship">
					<Ship ship={selectedShips[selectedShip]} />
				</div>
			{/each}
		</div>
	</div>
</div>

<style>
	.ship {
		background-color: white;
		padding: 0.5rem;
		margin: 0.5rem;
		border-radius: 0.5rem;
	}
	.container {
		border-radius: 0.5rem;
		display: flex;
		background-color: rgb(170, 219, 255);
	}
	.element {
		padding: 0.1rem;
	}
	.details {
		flex: 1;
	}
</style>
