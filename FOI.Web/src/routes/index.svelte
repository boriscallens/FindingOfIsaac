<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import Camera from '$lib/Camera/Camera.svelte';
</script>

<svelte:head>
	<title>Snap</title>
</svelte:head>

<section>
	<Camera></Camera>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: stretch;
		flex: 1;
	}
</style>
