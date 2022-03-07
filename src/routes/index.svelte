<script context="module">
	import { googleApi } from '../stores.js';
	if (typeof window !== 'undefined') {
		window.googleMapsInitialized = () => {
			googleApi.set(window.google);
		};
	}
</script>

<script lang="ts">
	import App from '../components/App.svelte';
</script>

<svelte:head>
	<script
		async
		defer
		src="https://maps.googleapis.com/maps/api/js?key={import.meta.env.VITE_GOOGLE_MAP_KEY}"
		on:load={window.googleMapsInitialized()}></script>
</svelte:head>
<App />
