<script>
	import Ship from './Ship.svelte';
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	export let selectedShips = {};
	let selected;
	const onShip = (e) => {
		if (selected === e.detail.ship.mmsi) selected = undefined;
		else selected = e.detail.ship.mmsi;
		dispatch('ship', selected ? e.detail : undefined);
	};
</script>

<div class="details">
	<div>
		{#each Object.keys(selectedShips).sort((a, b) => a.localeCompare(b)) as selectedShip (selectedShip)}
			<Ship ship={selectedShips[selectedShip]} on:ship={onShip} on:newLocation on:remove selected={selected === selectedShips[selectedShip].mmsi} />
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
