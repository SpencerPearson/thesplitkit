<script>
	import Modal from './Modal.svelte';
	import { onMount } from 'svelte';

	export let showModal;
	export let paymentType;

	let isDesktop = true;
	let venmoWindow;

	function openVenmo() {
		const url = 'https://account.venmo.com/u/sxworldwide';

		if (venmoWindow && !venmoWindow.closed) {
			venmoWindow.focus();
		} else {
			venmoWindow = window.open(url, '_blank');
		}
	}

	onMount(() => {
		const media = window.matchMedia('(min-width: 992px)');
		const update = () => (isDesktop = media.matches);
		update();

		media.addEventListener('change', update);
		return () => media.removeEventListener('change', update);
	});
</script>

<Modal
	bind:showModal
	bind:paymentType
	showNewPaymentType={true}
	backgroundColor="white"
	closeColor="black"
	width={isDesktop ? '300px' : undefined}
	height={isDesktop ? '480px' : undefined}
>
	<div>
		<img src="/sxww-venmo.jpg" alt="Venmo QR" />
		<button on:click={openVenmo}>Open Venmo</button>
	</div>
</Modal>

<style>
	div {
		display: flex;
		flex-direction: column;
		gap: 12px;
		align-items: center;
		font-family: 'Limelight', sans-serif;
		font-weight: 400;
		font-style: normal;
	}

	img {
		max-width: 100%;
		max-height: 100%;
		width: auto;
		height: auto;
	}

	button {
		font-size: 1em;
		cursor: pointer;
		margin: 8px;
		height: 50px;
		box-shadow: none;
		width: 180px;
		background: hsl(6, 67%, 50%);
		color: white;
		border: none;
		padding: 12px 24px;
		border-radius: 10px;
		font-weight: 600;
		cursor: pointer;
		transition: background 0.2s;
		font-family: 'Limelight', sans-serif;
		font-weight: 400;
		font-style: normal;
	}

	button:hover {
		background: hsl(6, 67%, 40%);
	}
</style>
