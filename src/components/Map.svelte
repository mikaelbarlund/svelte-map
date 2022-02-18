<script>
	import Ship from './Ship.svelte';
	let container;
	let map;
	let zoom = 12;
	let center = { lat: 60.192059, lng: 24.945831 };
	let location;
	let features = [];
	let markers = [];
	let ship;
	import { onMount } from 'svelte';

	const image = 'icons8-boat-32.png';
	const clickMarker = (mmsi) => {
		ship = mmsi;
	};
	onMount(async () => {
		// eslint-disable-next-line no-undef
		map = new google.maps.Map(container, {
			zoom,
			center
		});
		await (async () => {
			const response = await fetch('https://meri.digitraffic.fi/api/v1/locations/latest');
			const ships = await response.json();
			features = ships.features.filter((a) => a.properties.timestampExternal > Date.now() - 360000);
		})();
		if (navigator.geolocation) {
			navigator.geolocation.getCurrentPosition((position) => {
				location = { lat: position.coords.latitude, lng: position.coords.longitude };
				new google.maps.Marker({
					position: location,
					map: map
				});
			});
		}
	});

	$: (() => {
		markers.forEach((a) => a.setMap(null));
		markers = [];
		features.forEach((a) => {
			const where = { lat: a.geometry.coordinates[1], lng: a.geometry.coordinates[0] };
			const marker = new google.maps.Marker({
				position: where,
				map: map,
				icon: image
			});
			marker.addListener('click', () => {
				clickMarker(a.mmsi);
			});
			markers.push(marker);
		});
	})();
</script>

<button
	on:click={() => {
		map?.setCenter(location);
		map?.setZoom(12);
	}}
	>where am I
</button>
<button on:click={() => (features = [])}>clear</button>
{#if ship}
	<Ship {ship} on:click={() => (ship = undefined)} />
{/if}
<div class="full-screen" bind:this={container} />

<style>
	.full-screen {
		width: 100vw;
		height: 100vh;
	}
</style>
