<script>
	import Video from '../Video.svelte';
	import Boost from '../Boost.svelte';
	import Chat from '../Chat.svelte';
	import BoostBoard from '../BoostBoard.svelte';
	import Instructions from '../Instructions.svelte';
	import StaticQR from '../StaticQR.svelte';
	export let guid;
	export let broadcastingBlock;
	export let throwConfetti;
	let showInstructions = false;
</script>

<img class="background" alt="background" src={'/curtains1.png'} />

<container>
	<div class="left">
		<BoostBoard />
		<Boost {guid} {broadcastingBlock} bind:showInstructions {throwConfetti} />
	</div>

	<Video />

	<div class="right">
		<Chat />
		<div class="qr-container">
			<img src="/qr-frame.png" />
			<div class="static-qr-container">
				<StaticQR />
			</div>
		</div>
	</div>
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
		grid-template-columns: 322px 1fr 370px;
		grid-template-rows: 1fr;
		grid-column-gap: 0px;
		grid-row-gap: 0px;
	}

	.left {
		height: 100%;
		width: 322px;
		display: grid;
		grid-template-columns: 1fr;
		grid-template-rows: 1fr 188px;
		place-items: center;
	}

	.right {
		height: 100%;
		width: 100%;
		display: grid;
		grid-template-columns: 1fr;
		grid-template-rows: 1fr 216px;
		position: relative;
	}

	.qr-container {
		position: relative;
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.qr-container > img {
		height: 200px;
	}

	.static-qr-container {
		position: absolute;
		width: 110px;
	}
</style>
