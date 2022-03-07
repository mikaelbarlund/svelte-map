<script>
	import Ship from './Ship.svelte';
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	export let selectedShips = {};
	export let viewPortShips = [];
	let selected;
	const onShip = (e) => {
		selected = e.detail.ship.mmsi;
		console.log(e.detail);
		dispatch('ship', e.detail);
	};
</script>

<div class="details">
	<div>
		{#each Object.keys(selectedShips).sort((a, b) => a.localeCompare(b)) as selectedShip (selectedShip)}
			<Ship
				ship={selectedShips[selectedShip]}
				on:ship={onShip}
				on:newLocation
				on:remove
				selected={viewPortShips.length === 0 && selected === selectedShips[selectedShip].mmsi}
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
