<script lang="ts">
	import { onMount } from 'svelte';
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	import { googleApi } from '../stores.js';

	let map;
	let zoom = 12;
	export let center = { lat: 60.192059, lng: 24.945831 };
	let location;
	export let viewPortShips = [];
	let shipMarkers = [];
	export let track = [];
	let trackMarkers = [];
	const boat = 'icons8-boat-32.png';
	const dot = 'dot.png';
	const clickMarker = (input) => {
		dispatch('mmsi', input);
	};
	let container;
	onMount(async () => {
		map = new $googleApi.maps.Map(container, {
			zoom,
			center
		});
		if (navigator.geolocation) {
			navigator.geolocation.getCurrentPosition((position) => {
				location = { lat: position.coords.latitude, lng: position.coords.longitude };
				new $googleApi.maps.Marker({
					position: location,
					map: map
				});
			});
		}
	});
	$: map?.setCenter(center);

	$: (() => {
		shipMarkers.forEach((a) => a.setMap(null));
		shipMarkers = [];
		viewPortShips.forEach((a) => {
			const where = { lat: a.geometry.coordinates[1], lng: a.geometry.coordinates[0] };
			const marker = new $googleApi.maps.Marker({
				position: where,
				map: map,
				icon: boat
			});
			marker.addListener('click', () => {
				clickMarker(a.mmsi);
			});
			shipMarkers.push(marker);
		});
	})();
	$: (() => {
		trackMarkers.forEach((a) => a.setMap(null));
		trackMarkers = [];
		track.forEach((a) => {
			const where = { lat: a.geometry.coordinates[1], lng: a.geometry.coordinates[0] };
			const marker = new $googleApi.maps.Marker({
				position: where,
				map: map,
				icon: dot
			});
			marker.addListener('click', () => {
				clickMarker(a.mmsi);
			});
			trackMarkers.push(marker);
		});
	})();
</script>

<div class="map" bind:this={container} />

<style>
	.map {
		padding: 0.1rem;
		flex: 2;
		height: 90vh;
		border-radius: 0.5rem;
	}
</style>
