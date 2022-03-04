<script lang="ts">
    import Spin from './Spin.svelte';

    export let ship;
    let data;
    $: (async () => {
        data = undefined;
        const response = await fetch(`https://meri.digitraffic.fi/api/v1/metadata/vessels/${ship}`);
        data = await response.json();
    })();
</script>
<!-- https://svelte.dev/docs#template-syntax-await -->
<div class="modal" on:click>
    <div class="modal-content">
        {#if data}
            <h2>Callname {data.callSign}</h2>
            <h3>{data.name}</h3>
            <ul>
                <li>destination: {data.destination}</li>
                <li>draught: {data.draught}</li>
            </ul>
        {:else}
            <Spin/>
        {/if}
    </div>
</div>

<style>
    .modal {
        display: block; /* Hidden by default */
        position: fixed; /* Stay in place */
        z-index: 1; /* Sit on top */
        left: 0;
        top: 0;
        width: 100%; /* Full width */
        height: 100%; /* Full height */
        overflow: auto; /* Enable scroll if needed */
        background-color: rgb(0, 0, 0); /* Fallback color */
        background-color: rgba(0, 0, 0, 0.2); /* Black w/ opacity */
    }

    /* Modal Content/Box */
    .modal-content {
        background-color: #fefefe;
        margin: 15% auto; /* 15% from the top and centered */
        padding: 20px;
        border: 1px solid #888;
        width: 80%; /* Could be more or less, depending on screen size */
    }
</style>
