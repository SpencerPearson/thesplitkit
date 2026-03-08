<script>
	import StaticQR from '../StaticQR.svelte';
	import Chat from '../Chat.svelte';
	import BoostBoard from '../BoostBoard.svelte';
	import Instructions from '../Instructions.svelte';
	import Boost from '../Boost.svelte';
	export let guid;
	export let broadcastingBlock;
	export let throwConfetti;
	let showInstructions = false;
</script>

<img class="background" alt="background" src={'/curtains1.png'} />

<container>
	<BoostBoard />

	<div class="middle">
		<div class="qr">
			<StaticQR />
		</div>
	</div>

	<Chat />
</container>

{#if showInstructions}
	<Instructions bind:showInstructions />
{/if}

<style>
	.background {
		position: absolute;
		height: 100%;
		width: 100%;
		top: 0;
		object-fit: cover;
		z-index: -1;
		background-color: black;
	}

	container {
		height: calc(100% - 16px);
		width: calc(100% - 16px);
		margin: 8px;
		overflow: hidden;
		display: grid;
		grid-template-columns: 350px 1fr 350px;
		grid-template-rows: 1fr;
		grid-column-gap: 8px;
		grid-row-gap: 0px;
	}

	.middle {
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.qr {
		margin-top: 20%;
		width: 400px;
	}
</style>
