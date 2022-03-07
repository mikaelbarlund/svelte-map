<script lang="ts">
	import { onMount } from 'svelte';
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	import { googleApi } from '../stores.js';

	let map;
	let zoom = 12;
	let center = { lat: 60.192059, lng: 24.945831 };
	let location;
	export let features = [];
	let markers = [];
	const image = 'icons8-boat-32.png';
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

	$: (() => {
		markers.forEach((a) => a.setMap(null));
		markers = [];
		features.forEach((a) => {
			const where = { lat: a.geometry.coordinates[1], lng: a.geometry.coordinates[0] };
			const marker = new $googleApi.maps.Marker({
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

<div class="map" bind:this={container} />

<style>
	.map {
		padding: 0.1rem;
		flex: 2;
		height: 90vh;
		border-radius: 0.5rem;
	}
</style>
