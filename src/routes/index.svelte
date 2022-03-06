<script lang="ts">
	import Map from '../components/Map.svelte';
</script>

<svelte:head>
	<script src="https://maps.googleapis.com/maps/api/js?key={import.meta.env.VITE_GOOGLE_MAP_KEY}"></script>
</svelte:head>
<Map />
