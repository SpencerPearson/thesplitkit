<script>
	import Share from '$lib/Share/Share.svelte';
	import Modal from '$lib/Modal/Modal.svelte';
	import SaveModal from '$lib/Modal/SaveModal.svelte';

	import { defaultBlockGuid, changeDefault, mainSettings } from '$/stores';

	export let block;
	let showCopied = false;

	let showShareModal = false;
</script>

<top>
	{#if $defaultBlockGuid === block?.blockGuid}
		<button
			class="change-default"
			on:click={() => {
				$changeDefault = true;
			}}
		>
			<p>Set as Default</p>
		</button>
	{/if}
</top>

{#if !$mainSettings?.lowBandwidth?.images}
	<img src={block?.image || '/splitkit300.png'} alt="the artwork for this block" />
{:else}
	<img src={'/splitkit300.png'} alt="the artwork for this block" />
{/if}
<card-text>
	<h3>
		{block?.title || ''}
	</h3>
	<p>
		{block?.line?.[0] === 'Text - click to edit' ? '' : block?.line?.[0] || ''}
	</p>
</card-text>

{#if !block?.value?.destinations?.length}
	<h3 class="warning">This item has no value blocks</h3>
{:else}
	{#if block?.value?.destinations?.reduce((acc, person) => acc + parseFloat(person.fee ? 0 : person.split), 0) !== 100}
		<h3 class="warning">Splits do not add up to 100%</h3>
		{block?.value?.destinations?.reduce(
			(acc, person) => acc + parseFloat(person.fee ? 0 : person.split),
			0
		)}
	{/if}
	{#if block?.value?.destinations?.some((item) => !item.address)}
		<h3 class="warning">Value Blocks are missing addresses.</h3>
	{/if}
{/if}

{#if showShareModal}
	<Modal bind:showModal={showShareModal}>
		<Share eventGuid={block.eventGuid} blockGuid={block?.blockGuid} promotion={true} />
	</Modal>
{/if}

{#if showCopied}
	<SaveModal>
		<h2>Copied</h2>
	</SaveModal>
{/if}

<style>
	top {
		display: flex;
		width: calc(100% - 8px);
		justify-content: space-between;
		height: 40px;
		padding: 4px;
		align-items: center;
		position: relative;
	}

	top button {
		background-color: var(--md-surface-soft);
		color: var(--md-text);
		display: flex;
		align-items: center;
		justify-content: center;
		height: 40px;
		width: 40px;
		border-radius: 25px;
		padding: 0;
		margin: 0;
		box-shadow: var(--md-shadow-soft);
		border: 1px solid var(--md-border);
	}

	img {
		width: 200px;
		height: 200px;
		border-radius: 8px;
	}

	card-text {
		width: 100%;
		padding: 4px;
		display: flex;
		flex-direction: column;
		align-items: center;
		flex: 1;
	}

	h3 {
		margin: 4px 0;
	}
	p {
		padding: 0;
		margin: 0;
	}

	h3.warning {
		color: #ff8c8c;
	}

	.change-default {
		color: var(--md-on-primary);
		background-color: color-mix(in oklab, var(--md-secondary) 65%, var(--md-primary) 35%);
		position: absolute;
		top: 64px;
	}

	.change-default p {
		font-size: 0.75em;
	}

	card-text h3,
	card-text p {
		color: var(--md-text);
	}
</style>
