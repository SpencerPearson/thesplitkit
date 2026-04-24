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

<div
	class="card"
	role="button"
	tabindex="0"
	on:click={() => {
		if ($user.loggedIn) {
			showModal = true;
			activeBlock = clone(block);
		} else {
			goto(redirectUrl);
		}
	}}
	on:keydown={(event) => {
		if (event.key === 'Enter' || event.key === ' ') {
			event.preventDefault();
			if ($user.loggedIn) {
				showModal = true;
				activeBlock = clone(block);
			} else {
				goto(redirectUrl);
			}
		}
	}}
>
	<album>
		<div class="artwork">
			<img src={block.image} alt={block.title} />
		</div>

		<img class="border" src="/boo-card3.png" alt="cover art border" />
	</album>
</div>

<style>
	.card {
		display: inline-flex;
		flex-direction: column;
		align-items: center;
		overflow: hidden;
		cursor: pointer;
		margin: 0 8px;
		align-items: center;
		justify-content: center;
		max-width: 160px;
		border-radius: 14px;
		background: color-mix(in oklab, var(--md-surface), transparent 15%);
		border: 1px solid var(--md-border);
		box-shadow: var(--md-shadow-soft);
		padding: 6px;
	}

	album {
		display: inline-block;
		position: relative;
	}
	.border {
		width: 100%;
		opacity: 0.86;
	}

	.artwork {
		width: 97.5%;
		height: 90%;
		overflow: hidden;
		position: absolute;
		top: 6.5%;
		left: 1.25%;
		z-index: 1;
		border-radius: 0 0 3px 3px;
		object-fit: cover;
		/* height: 90%;
		width: 90%;
		position: relative;
		top: 0px;
		left: 8px; */
	}

	.artwork > img {
		object-fit: cover;
		height: 100%;
		width: 100%;
	}
</style>
