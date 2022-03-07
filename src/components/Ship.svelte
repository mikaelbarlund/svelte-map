<script lang="ts">
	export let ship;
</script>

<div class="ship">
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
</style>
