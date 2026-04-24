<script>
	import { onMount } from 'svelte';
	import Close from '../../lib/icons/Close.svelte';

	export let closeModal = () => {};
	export let showModal = false;
	export let unsaved = false;
	export let isFeedDownload = false;
	export let dark = false;
	export let imgSrc;
	export let width;
	export let height;
	export let backgroundColor;

	onMount(() => {
		const handleKeyDown = (event) => {
			if (event.key === 'Escape') {
				onClose();
			}
		};

		document.addEventListener('keydown', handleKeyDown);

		return () => document.removeEventListener('keydown', handleKeyDown);
	});

	function onClose() {
		closeModal();

		if (unsaved) {
			const userConfirmation = window.confirm('Your changes are unsaved. Do you want to continue?');
			if (userConfirmation) {
				showModal = false;
				unsaved = false;
			}
		} else if (isFeedDownload) {
			const userConfirmation = window.confirm(
				'Do you want to leave without downloading your feed? \nYour progress will be lost.'
			);
			if (userConfirmation) {
				showModal = false;
			}
		} else {
			showModal = false;
		}
	}
</script>

<blurred-background
	role="button"
	tabindex="0"
	on:mousedown|self={onClose}
	on:touchend|self={onClose}
	on:keydown={(event) => {
		if (event.key === 'Escape' || event.key === 'Enter' || event.key === ' ') onClose();
	}}
>
	<modal
		class:dark
		style={`
			${
				imgSrc
					? `background-image: url('${imgSrc}'); background-size: cover; background-position: center;`
					: ''
			}
			${width ? `width: ${width};` : ''}
			${height ? `height: ${height};` : ''}
			${backgroundColor ? `background-color: ${backgroundColor};` : ''}
		`}
	>
		<button class="close" on:click={onClose}>
			<Close size="24" />
		</button>

		<container>
			<slot />
		</container>
	</modal>
</blurred-background>

<style>
	blurred-background {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
		position: fixed;
		background: color-mix(in oklab, var(--md-bg), transparent 45%);
		top: 0;
		right: 0;
		z-index: 100;
		backdrop-filter: blur(5px);
	}

	modal {
		display: block;
		position: relative;
		width: calc(100% - 16px);
		height: calc(100% - 32px);
		max-width: 991px;
		overflow: hidden;
		border-radius: 16px;
		background-color: var(--md-surface);
		color: var(--md-text);
		border: 1px solid var(--md-border);
		box-shadow: 0px 5px 10px 2px rgba(0, 0, 0, 0.75);
		background-size: cover;
		background-position: center;
		background-repeat: no-repeat;
	}

	.dark {
		background-color: var(--md-surface-elevated);
		color: var(--md-text);
		box-shadow: var(--md-shadow);
	}

	button {
		color: white;
		padding: 4px 12px;
		border: 1px solid transparent;
		border-radius: 25px;
		font-size: 1.05em;
		cursor: pointer;
		height: 50px;
		position: absolute;
	}

	.dark > button {
		color: #e0e0e0;
	}

	container {
		display: block;
		margin-top: 58px;
		width: 100%;
		height: calc(100% - 58px);
	}

	.close {
		position: absolute;
		top: 0;
		right: 0;
		background-color: transparent;
		padding: 8px;
		color: var(--md-text-muted);
		z-index: 33;
		border: 1px solid transparent;
		box-shadow: none;
	}

	@media screen and (max-width: 992px) {
		modal {
			position: relative;
			width: 100%;
			height: 100%;
			overflow-y: auto;
			overflow-x: hidden;
			border-radius: 0;
		}

		fade-bottom {
			display: none;
		}
	}
</style>
