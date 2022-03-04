<script lang="ts">
	import Ship from './Ship.svelte';
	import { onMount } from 'svelte';

	let container;
	let map;
	let zoom = 12;
	let center = { lat: 60.192059, lng: 24.945831 };
	let location;
	let features = [];
	let markers = [];
	let mmsi;

	const image = 'icons8-boat-32.png';
	const clickMarker = (input) => {
		mmsi = input;
	};
	const getShips = async () => {
		const response = await fetch('https://meri.digitraffic.fi/api/v1/locations/latest');
		const ships = await response.json();
		features = ships.features.filter((a) => a.properties.timestampExternal > Date.now() - 360000);
	};
	onMount(async () => {
		// eslint-disable-next-line no-undef
		map = new google.maps.Map(container, {
			zoom,
			center
		});
		await getShips();
		if (navigator.geolocation) {
			navigator.geolocation.getCurrentPosition((position) => {
				location = { lat: position.coords.latitude, lng: position.coords.longitude };
				// eslint-disable-next-line no-undef
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
			// eslint-disable-next-line no-undef
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

{#if mmsi}
	<Ship {mmsi} on:click={() => (mmsi = undefined)} />
{/if}
<div class="container">
	<div class="map element" bind:this={container} />
	<div class="details element">
		<button
			on:click={() => {
				map?.setCenter(location);
				map?.setZoom(12);
			}}
			>where am I
		</button>
		<button on:click={() => (features = [])}>clear all ships</button>
		<button on:click={() => getShips()}>reload ships</button>
	</div>
</div>

<style>
	.container {
		display: flex;
	}
	.element {
		padding: 10px;
	}
	.map {
		flex: 2;
		height: 90vh;
	}
	.details {
		flex: 1;
	}
</style>
