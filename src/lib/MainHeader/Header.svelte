<script>
	import MainMenu from './MainMenu.svelte';
	import BackArrow from '$lib/icons/BackArrow.svelte';
	import PlayIcon from '$lib/icons/PlayArrow.svelte';
	import StopIcon from '$lib/icons/Stop.svelte';
	import { user, liveEnclosure, liveMode } from '$/stores';
	import { page } from '$app/stores';
	import { goto } from '$app/navigation';
	export let theme = 'light';
	export let toggleTheme = () => {};
	$: guid = $page.params.guid;
	$: route = $page.route.id;

	let player;
	let isStopped = true;

	$: showBackButton = [
		'/(main)/events/dashboard/[guid]',
		'/(main)/events/creator',
		'/(main)/events/import',
		'/(main)/remotevalue/[guid]',
		'/(main)/chapters/[guid]',
		'/(main)/soundboard/[guid]'
	].find((v) => v === route);

	$: showPlayButton = ['/(main)/live/[guid]'].find((v) => v === route) && player?.src;

	$: if (['playlist', 'podcast'].find((v) => $liveMode === v) && $liveEnclosure) {
		player = new Audio();
		player.src = $liveEnclosure;
	}

	function handleAudio() {
		if (player) {
			if (isStopped) {
				// The audio is stopped, so start playing it.
				player.src = $liveEnclosure;
				player.play();
				isStopped = false;
			} else {
				// The audio is playing, so stop it.
				player.src = '';
				isStopped = true;
			}
		}
	}

	function handleBackButton() {
		if (
			['/(main)/events/dashboard/[guid]', '/(main)/events/creator', '/(main)/events/import'].find(
				(v) => v === route
			)
		) {
			goto('/events');
		}

		if (
			['/(main)/remotevalue/[guid]', '/(main)/chapters/[guid]', '/(main)/soundboard/[guid]'].find(
				(v) => v === route
			)
		) {
			goto(`/events/dashboard/${guid}`);
		}
	}
</script>

{#if route !== '/(main)/boostboard/[guid]'}
	<header>
		{#if showBackButton}
			<button class="icon" on:click={handleBackButton}><BackArrow size="40" /></button>
		{:else if showPlayButton}
			<button class="play" on:click={handleAudio}
				>{#if isStopped}
					<PlayIcon size="40" />
				{:else}
					<StopIcon size="40" />
				{/if}</button
			>
		{:else}
			<spacer />
		{/if}

		<div class="center">
			<h1>The Split Kit</h1>
			<p>{$user.name || ''}</p>
		</div>
		{#if route === '/(main)/soundboard/[guid]'}
			<a href="/soundboard/instructions">Sound Board Instructions</a>
		{/if}
		{#if route === '/(main)/triggerboard/[guid]'}
			<a href="/triggerboard/instructions">Trigger Board Instructions</a>
		{/if}
		<button
			class="theme-toggle"
			on:click={toggleTheme}
			aria-label={theme === 'dark' ? 'Switch to light theme' : 'Switch to dark theme'}
			title={theme === 'dark' ? 'Switch to light theme' : 'Switch to dark theme'}
		>
			{#if theme === 'dark'}
				<svg
					aria-hidden="true"
					viewBox="0 0 24 24"
					width="18"
					height="18"
					fill="none"
					stroke="currentColor"
					stroke-width="2"
					stroke-linecap="round"
				>
					<circle cx="12" cy="12" r="4.25" />
					<line x1="12" y1="1.8" x2="12" y2="5" />
					<line x1="12" y1="19" x2="12" y2="22.2" />
					<line x1="1.8" y1="12" x2="5" y2="12" />
					<line x1="19" y1="12" x2="22.2" y2="12" />
					<line x1="4.7" y1="4.7" x2="7" y2="7" />
					<line x1="17" y1="17" x2="19.3" y2="19.3" />
					<line x1="17" y1="7" x2="19.3" y2="4.7" />
					<line x1="4.7" y1="19.3" x2="7" y2="17" />
				</svg>
			{:else}
				<span aria-hidden="true">☾</span>
			{/if}
		</button>
		<MainMenu />
	</header>
{/if}

<style>
	header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 10px;
		width: 100%;
		margin: 0;
		height: 62px;
		padding: 0 14px;
		box-sizing: border-box;
		border-radius: 18px;
		background: var(--md-surface);
		border: 1px solid var(--md-border);
		box-shadow: var(--md-shadow-soft);
	}

	h1 {
		margin: 0;
		font-size: 1rem;
		font-weight: 700;
		line-height: 1.2;
	}

	p {
		padding: 0;
		margin: 0;
		font-size: 0.82rem;
		color: var(--md-text-muted);
	}

	.center {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: flex-start;
		flex: 1;
		min-width: 0;
	}

	button {
		padding: 0;
	}

	spacer {
		display: block;
		width: 44px;
	}

	button.play {
		color: var(--color-text-0);
		background-color: transparent;
		display: flex;
		align-items: center;
		justify-content: center;
		box-shadow: none;
		padding: 0;
		position: relative;
		left: -4px;
		top: 2px;
	}

	button.theme-toggle {
		padding: 0;
		min-width: 42px;
		height: 36px;
		width: 42px;
		border-radius: 50%;
		background: var(--md-surface-soft);
		color: var(--md-text);
		border: 1px solid var(--md-border);
		box-shadow: none;
		font-size: 1.05rem;
		font-weight: 600;
	}
</style>
