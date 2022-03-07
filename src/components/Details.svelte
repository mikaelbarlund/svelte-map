<script>
	import Ship from './Ship.svelte';
	export let selectedShips = {};
</script>

<div class="details">
	<div>
		{#each Object.keys(selectedShips).sort((a, b) => a.localeCompare(b)) as selectedShip (selectedShip)}
			<Ship ship={selectedShips[selectedShip]} />
		{/each}
	</div>
</div>

<style>
	.details {
		flex: 1;
		padding: 0.1rem;
	}
</style>
