<script>
	import { slide } from 'svelte/transition';
	import { goto } from '$app/navigation';
	import { page } from '$app/stores';
	import AccountIcon from '$lib/icons/Account.svelte';
	import MenuIcon from '$lib/icons/Menu.svelte';

	import { user, albyClientId, remoteServer } from '$/stores';

	let expandMenu = false;

	async function logout() {
		const res = await fetch(remoteServer + '/api/alby/logout', { credentials: 'include' });

		const data = await res.json();
		console.log(data);
		$user = data;
		goto('/');
	}

	const redirectUrl =
		`https://getalby.com/oauth?client_id=${albyClientId}&response_type=code&redirect_uri=${
		encodeURIComponent($page.url.origin + '/')
		}` + `&scope=account:read%20balance:read%20payments:send%20invoices:read`;
</script>

<button
	class="icon"
	on:click={() => {
		expandMenu = true;
	}}
>
	{#if $user.loggedIn}
		<MenuIcon size="40" />
	{:else}
		<AccountIcon size="40" />
	{/if}
</button>

{#if expandMenu}
	<container
		role="button"
		tabindex="0"
		on:click={() => {
			expandMenu = false;
		}}
		on:keydown={(event) => {
			if (event.key === 'Escape' || event.key === 'Enter' || event.key === ' ') {
				expandMenu = false;
			}
		}}
	>
		<menu>
			<account-button-hover />
			<ul transition:slide={{ duration: 200 }}>
				{#if $user.loggedIn}
					<li><button class="menu-item" on:click={logout}>Log Out</button></li>

					<li><a href="/events">Events</a></li>
					{#if $page?.params?.guid}
						<li><a href="/soundboard/{$page.params.guid}">Sound Board</a></li>
					{/if}
					{#if $page?.params?.guid}
						<li><a href="/triggerboard/{$page.params.guid}">Trigger Board</a></li>
					{/if}
					{#if $page?.params?.guid}
						<li><a href="/boostboard/{$page.params.guid}">Boost Board</a></li>
					{/if}
				{:else}
					<li>
						<a href="/login"> Log In </a>
					</li>
				{/if}
			</ul>
		</menu>
	</container>
{/if}

<style>
	container {
		width: 100vw;
		height: 100vh;
		position: absolute;
		overflow: hidden;
		top: 0;
		left: 0;
		z-index: 33;
	}

	button {
		align-self: flex-end;
	}

	account-button-hover {
		display: block;
		width: 56px;
		height: 42px;
		cursor: pointer;
	}
	menu {
		position: relative;
		display: flex;
		flex-direction: column;
		align-items: flex-end;
		right: 20px;
		top: 10px;
		margin: 0;
	}

	ul {
		width: 190px;
		padding: 0;
		margin: 0;
		overflow: hidden;
		background-color: var(--md-surface-elevated);
		border-radius: 14px;
		border: 1px solid var(--md-border);
		box-shadow: var(--md-shadow);
	}

	li {
		padding: 0;
		list-style: none;
		width: 100%;
		text-align: left;
		cursor: pointer;
		font-size: 0.92rem;
		color: var(--md-text);
	}
	li:hover {
		background-color: var(--color-theme-light-blue);
	}

	button.menu-item {
		all: unset;
		box-sizing: border-box;
		display: block;
		width: 100%;
		padding: 11px 12px;
		cursor: pointer;
		color: inherit;
		font-size: 0.92rem;
		font-weight: 500;
	}

	a {
		display: block;
		width: 100%;
		padding: 11px 12px;
		color: inherit;
		text-decoration: none;
	}
</style>
