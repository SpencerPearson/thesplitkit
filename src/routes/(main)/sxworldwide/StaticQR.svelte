<script>
	import QRCode from 'qrcode';
	import { onMount, onDestroy } from 'svelte';

	let qrCodeCanvas;
	let container;
	export let code = 'tsk-58eb81f8-caeb-4c77-a326-36e406207fe0@boostbeach.xyz';
	export let imgSrc = '';
	let observer;

	function generateQRCode() {
		if (!qrCodeCanvas || !container) return;

		const size = container.clientWidth;

		QRCode.toCanvas(qrCodeCanvas, code, {
			width: size,
			margin: 1,
			color: {
				dark: '#D43A2A',
				light: '#000000' // Fully transparent background
			}
		}).catch(console.error);
	}

	onMount(() => {
		generateQRCode();

		observer = new ResizeObserver(generateQRCode);
		if (container) observer.observe(container);
	});

	onDestroy(() => {
		if (observer && container) observer.unobserve(container);
	});

	function copyCodeToClipboard() {
		navigator.clipboard.writeText(code);
		alert('Copied Link to clipboard');
	}
</script>

<div class="qr-wrapper" bind:this={container} on:click={copyCodeToClipboard}>
	<canvas bind:this={qrCodeCanvas} />
</div>

<style>
	.qr-wrapper {
		width: 100%;
		height: 100%;
		background-color: transparent;
		box-shadow: none;
		aspect-ratio: 1 / 1;
		border-radius: 5px;
	}

	canvas {
		display: block;
		background: transparent;
	}
</style>
