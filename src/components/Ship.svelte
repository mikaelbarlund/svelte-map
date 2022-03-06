<script lang="ts">
	export let ship;
</script>

<div>
	<h2>Callname {ship.callSign}</h2>
	<h3>{ship.name}</h3>
	<ul>
		<li>destination: {ship.destination}</li>
		<li>draught: {ship.draught}</li>
	</ul>
</div>
