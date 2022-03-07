<script lang="ts">
	export let ship;
	export let selected;
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	function onClick(ship) {
		dispatch('ship', ship);
	}
</script>

<div class="ship" class:selected on:click={() => onClick(ship)}>
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
