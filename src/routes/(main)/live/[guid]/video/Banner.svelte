<script>
	import clone from 'just-clone';

	import { page } from '$app/stores';
	import { goto } from '$app/navigation';

	import { user, albyClientId } from '$/stores';

	export let block = {};
	export let showModal;
	export let activeBlock;

	let redirectUrl = `https://getalby.com/oauth?client_id=${albyClientId}`;
	redirectUrl += `&response_type=code&redirect_uri=${encodeURIComponent($page.url.origin + '/')}`;
	redirectUrl += `&scope=account:read%20balance:read%20payments:send%20invoices:read`;
</script>

<div>
	<button
		class="banner-button"
		on:click={() => {
			if ($user.loggedIn) {
				showModal = true;
				activeBlock = clone(block);
			} else {
				goto(redirectUrl);
			}
		}}
	>
		<img src={block.image} alt={block.title} />
	</button>
</div>

<style>
	div {
		display: flex;
		align-items: center;
		justify-content: center;
		max-width: 200px;
		border-radius: 16px;
		background: color-mix(in oklab, var(--md-surface), transparent 18%);
		border: 1px solid var(--md-border);
		box-shadow: var(--md-shadow-soft);
		padding: 6px;
	}
	.banner-button {
		all: unset;
		cursor: pointer;
		display: block;
		width: 100%;
	}
	img {
		max-width: 100%;
		max-height: 100%;
		object-fit: contain;
		border-radius: 10px;
	}
</style>
