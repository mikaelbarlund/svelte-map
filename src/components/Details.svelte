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
				selected={viewPortShips.length === 1 && viewPortShips[0].mmsi === selectedShips[selectedShip].mmsi}
			/>
		{/each}
	</div>
</div>

<style>
	.details {
		flex: 1;
		padding: 0.1rem;
	}
</style>
