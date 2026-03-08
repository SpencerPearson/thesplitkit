<script>
	import Boost from './Boost.svelte';
	import StaticQR from './StaticQR.svelte';
	import Instructions from './Instructions.svelte';
	export let guid;
	export let broadcastingBlock;
	export let throwConfetti;
	let showInstructions = false;
</script>

<div>
	<StaticQR />
	<Boost isMobile={true} {guid} {broadcastingBlock} bind:showInstructions {throwConfetti} />
</div>

{#if showInstructions}
	<Instructions bind:showInstructions />
{/if}

<style>
	div {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: flex-end;
		align-items: center;
		position: relative;
	}
</style>
