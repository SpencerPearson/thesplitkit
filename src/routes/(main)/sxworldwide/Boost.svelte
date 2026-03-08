<script>
	import QR from './QR.svelte';
	import BoostPage from './BoostPage.svelte';
	import Venmo from './Venmo.svelte';
	import PaymentSelector from './PaymentSelector.svelte';
	import { page } from '$app/stores';

	import { liveBlocks, albyClientId, user } from '$/stores';

	let redirectUrl = `https://getalby.com/oauth?client_id=${albyClientId}`;
	redirectUrl += `&response_type=code&redirect_uri=${$page.url.href}`;
	redirectUrl += `&scope=account:read%20balance:read%20payments:send%20invoices:read`;

	let showModal = false;
	export let broadcastingBlock;
	export let showInstructions;
	export let throwConfetti;
	export let isMobile;
	let paymentType;

	$: console.log(paymentType);
	$: console.log(showModal);
</script>

<div class="container">
	<button
		class="boost-btn boost"
		class:mobile={isMobile}
		on:click={() => {
			showModal = true;
		}}
	>
		🚀
		<p>Boost</p>
	</button>
	<button
		class="boost-btn boost"
		class:mobile={isMobile}
		on:click={() => {
			showModal = true;
		}}
	>
		⚡
		<p>Zap</p>
	</button>
</div>

{#if showModal}
	{#if !paymentType}
		<PaymentSelector bind:paymentType bind:showModal />
	{:else if paymentType === 'venmo'}
		<Venmo bind:showModal bind:paymentType />
	{:else}
		<BoostPage bind:showModal {broadcastingBlock} bind:paymentType {throwConfetti} {isMobile} />
	{/if}
{/if}

<style>
	.container {
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
		width: 100%;
		padding: 1rem;
		gap: 1rem;
		box-sizing: border-box;
		overflow: hidden;
	}

	.boost-btn {
		background: hsl(6, 67%, 50%);
		color: white;
		border: none;
		padding: 0;
		height: 100px;
		width: 100px;
		border-radius: 50px;
		font-size: 48px;
		font-weight: 600;
		cursor: pointer;
		box-shadow: 0 2px 1px 1px hsla(0, 0%, 100%, 0.25);
		display: flex;
		flex-direction: column;
		font-family: 'Limelight', sans-serif;
		font-weight: 400;
		font-style: normal;
	}

	.boost-btn:hover {
		background: hsl(6, 67%, 40%);
	}

	.boost-btn > p {
		font-size: 20px;
		margin: 0;
		padding: 0;
	}
</style>
