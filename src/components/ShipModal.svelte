<script lang="ts">
	import Spin from './Spin.svelte';
	import { fade, fly } from 'svelte/transition';
	import Ship from './Ship.svelte';
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let mmsi;
	const getShip = async () => {
		const response = await fetch(`https://meri.digitraffic.fi/api/v1/metadata/vessels/${mmsi}`);
		return response.json();
	};
	let promisedShip = getShip();
	function onClick(ship) {
		dispatch('ship', ship);
	}
</script>

<div class="modal" on:click in:fade out:fade>
	<div class="modal-content" in:fly={{ x: -200, duration: 1500 }}>
		{#await promisedShip}
			<Spin />
		{:then ship}
			<div on:click={() => onClick(ship)}>
				<Ship {ship} />
			</div>
		{:catch error}
			<div>{error}</div>
		{/await}
	</div>
</div>

<style>
	.modal {
		display: block; /* Hidden by default */
		position: fixed; /* Stay in place */
		z-index: 1; /* Sit on top */
		left: 0;
		top: 0;
		width: 100vw; /* Full width */
		height: 100vh; /* Full height */
		overflow: auto; /* Enable scroll if needed */
		background-color: rgb(0, 0, 0); /* Fallback color */
		background-color: rgba(0, 0, 0, 0.2); /* Black w/ opacity */
	}
	.modal-content {
		background-color: #fefefe;
		margin: 15% auto; /* 15% from the top and centered */
		padding: 20px;
		border: 1px solid #888;
		width: 60vh; /* Could be more or less, depending on screen size */
		border-radius: 0.5rem;
	}
</style>
