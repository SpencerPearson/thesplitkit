<script>
	import Settings from '$lib/icons/Settings.svelte';
	import Share from '$lib/icons/Share.svelte';
	import AddBlocks from '$lib/icons/AddBlocks.svelte';
	import { page } from '$app/stores';
	import remoteSave from '$lib/functions/remoteSave.js';
	import Save from '$lib/icons/Save.svelte';

	export let showShareModal;
	export let showMainSettingsModal;
	export let showSelectBlock;

	import clone from 'just-clone';
	import SaveModal from '$lib/Modal/SaveModal.svelte';

	import { liveBlocks, defaultBlockGuid, changeDefault, mainSettings } from '$/stores';

	let initialized = false;
	let savedBlocks = [];
	let showSaved = false;
	let saveTimeout;
	let savingText = '';

	$: compareBlocks($liveBlocks);

	function compareBlocks(blocks) {
		if (JSON.stringify(blocks) !== JSON.stringify(savedBlocks)) {
			savedBlocks = clone(blocks);
			if (initialized) {
				clearTimeout(saveTimeout);
				saveTimeout = setTimeout(saveState, 1000);
			} else {
				initialized = true;
			}
		}
	}

	function saveState() {
		remoteSave($liveBlocks, $page.params.guid);
		savingText = `Last Saved: ${formatTime(new Date())}`;
	}

	function formatTime(date) {
		const hours = date.getHours();
		const minutes = date.getMinutes().toString().padStart(2, '0');
		const seconds = date.getSeconds().toString().padStart(2, '0');
		const ampm = hours >= 12 ? 'pm' : 'am';
		const hour12 = hours % 12 || 12; // Convert 0 hour to 12
		return `${hour12}:${minutes}:${seconds} ${ampm}`;
	}
</script>

<header class="ui-topbar ui-surface motion-fade-in">
	<button class="save ui-btn ui-btn-muted" type="submit" on:click={saveState}>
		<Save size="32" />
	</button>
	<button class="ui-btn ui-btn-muted" on:click={() => (showShareModal = true)}>
		<Share size="32" />
	</button>
	<button
		class="add-block ui-btn ui-btn-accent"
		class:disabled={false}
		on:click={() => {
			showSelectBlock = true;
			if (!$defaultBlockGuid && $mainSettings.broadcastMode !== 'edit') {
				$changeDefault = true;
			}
		}}
	>
		<AddBlocks size="36" />
	</button>

	<button class="ui-btn ui-btn-muted" on:click={() => (showMainSettingsModal = true)}>
		<Settings size="32" />
	</button>
	<spacer />
	<p>{savingText}</p>
</header>

{#if showSaved}
	<SaveModal>
		<h2>Saved</h2>
	</SaveModal>
{/if}

<style>
	header {
		width: 100%;
		max-width: 350px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin: 0 auto;
		height: 72px;
		position: relative;
		padding: 8px;
	}

	button {
		display: flex;
		align-items: center;
		justify-content: center;
		height: 50px;
		width: 50px;
		padding: 0;
		margin: 0 6px;
	}

	.add-block {
		height: 60px;
		width: 60px;
		border-radius: 30px;
		color: #fff;
	}

	.disabled {
		background-color: color-mix(in oklab, var(--md-surface-soft), transparent 22%);
		color: var(--md-text-muted);
	}

	spacer {
		width: 60px;
	}

	p {
		position: absolute;
		text-align: center;
		font-size: 0.9em;
		bottom: -24px;
		margin: 0;
		padding: 0;
		width: 100%;
		color: var(--md-text-muted);
	}

	@keyframes heartbeat {
		0% {
			transform: scale(1);
			box-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
		}
		50% {
			transform: scale(1.1);
			box-shadow: 0 0 10px rgba(0, 0, 0, 0.8);
		}
		100% {
			transform: scale(1);
			box-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
		}
	}
</style>
