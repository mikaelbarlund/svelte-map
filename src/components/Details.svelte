<script>
	import Ship from './Ship.svelte';
	export let selectedShips = {};
	export let viewPortShips = [];
</script>

<div class="details">
	<div>
		{#each Object.keys(selectedShips).sort((a, b) => a.localeCompare(b)) as selectedShip (selectedShip)}
			<Ship
				ship={selectedShips[selectedShip]}
				on:ship
				on:newLocation
				selected={viewPortShips.length === 1 && viewPortShips[0].mmsi === selectedShips[selectedShip].mmsi}
			/>
		{/each}
	</div>
</div>

<style>
	.details {
		flex: 1;
		padding: 0.1rem;
		height: 90vh;
		overflow: auto;
	}
</style>
