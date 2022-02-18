<script>
	import { onMount } from 'svelte';
	import Map from '../components/Map.svelte';
	import Spin from '../components/Spin.svelte';

	let ready;
	onMount(() => {
		window.initMap = () => {
			ready = true;
		};
	});
</script>

<svelte:head>
	<script
		defer
		async
		src="https://maps.googleapis.com/maps/api/js?key=GOOGLE_API_KEY&callback=initMap">
	</script>
</svelte:head>

<section>
	{#if ready}
		<Map />
	{:else}
		<Spin />
	{/if}
</section>
