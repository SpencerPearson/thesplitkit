<script>
	import Video from '../Video.svelte';
	import BoostBoard from '../BoostBoard.svelte';
	import MobileBoost from '../MobileBoost.svelte';
	import Chat from '../Chat.svelte';
	import Instructions from '../Instructions.svelte';
	let currentScreen = 'video';

	function changeScreen(screenName) {
		currentScreen = screenName;
	}

	export let guid;
	export let broadcastingBlock;
	export let throwConfetti;
	let showInstructions = false;
</script>

<img class="background" alt="background" src={'./curtains1.png'} />

<nav>
	<button on:click={changeScreen.bind(this, 'boostBoard')}>Leader Board</button>
	<button on:click={changeScreen.bind(this, 'video')}>Video</button>
	<button on:click={changeScreen.bind(this, 'boost')}>Boost/Zap</button>
	<button on:click={changeScreen.bind(this, 'chat')}>SuperChat</button>
</nav>

<container>
	<div class:show={currentScreen === 'video'}>
		<Video />
	</div>

	<div class="boost-board" class:show={currentScreen === 'boostBoard'}>
		<div class="boost-board-container">
			<BoostBoard />
		</div>
	</div>
	<div class="boost" class:show={currentScreen === 'boost'}>
		<div class="boost-container">
			<MobileBoost {guid} {broadcastingBlock} bind:showInstructions {throwConfetti} />
		</div>
	</div>

	<div class="chat" class:show={currentScreen === 'chat'}>
		<div class="chat-container">
			<Chat />
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
	}
	nav {
		display: flex;
		align-items: center;
		justify-content: space-around;
		background-color: hsla(0, 1%, 33%, 0.5);
	}

	nav > button {
		background-color: transparent;
		box-shadow: none;
		color: rgb(255, 255, 255);
		padding: 12px 0;
	}

	container {
		width: 100%;
		height: calc(100% - 40px);
		display: flex;
		align-items: center;
		justify-content: center;
	}

	div {
		display: none;
		width: 100%;
		height: calc(100% - 40px);
	}

	.boost-board {
		flex-direction: column;
		height: 100%;
		width: 100%;
	}

	.boost-board-container,
	.chat-container {
		display: block;
		width: 333px;
	}

	.boost-container {
		display: block;
		width: 100%;
	}

	.chat {
		margin: 0px;
		width: 100%;
		height: 100%;
	}

	.show {
		display: flex;
		align-items: center;
		justify-content: center;
	}
</style>
