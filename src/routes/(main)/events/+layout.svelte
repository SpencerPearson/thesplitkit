<script>
	import { page } from '$app/stores';
	import { onMount } from 'svelte';
	import Spinner from '$lib/loaders/Spinner.svelte';

	import { user, loaded, userReady, liveBlocks, activePageGuid } from '$/stores';

	let showLoading = false;
	let unmounted = true;

	$: console.log(unmounted);
	$: console.log(showLoading);

	const guid = $page.params.guid;
	onMount(() => {
		if (!$liveBlocks.length || $activePageGuid !== guid) {
			showLoading = true;
		}
		unmounted = false;
	});

	$: if ($loaded) {
		setTimeout(() => (showLoading = false), 2000);
	}
</script>

{#if $page.route.id !== '/(main)/events/[guid]'}
	{#if !unmounted}
		{#if showLoading}
			<loading>
				<Spinner size={48} label="Loading event data" />
			</loading>
		{:else if !$user?.loggedIn && guid !== 'test'}
			<div class="login-required page-surface">
				<h2>Please <a href="/login"> Log In </a> to Continue</h2>
			</div>
		{:else}
			<slot />
		{/if}
	{/if}
{:else}
	<slot />
{/if}

<style>
	div {
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 40px 16px 16px;
	}

	.login-required {
		padding: 22px;
		border-radius: 16px;
	}

	loading {
		display: flex;
		width: 100%;
		height: 100%;
		align-items: center;
		justify-content: center;
		position: relative;
		min-height: 220px;
	}
</style>
