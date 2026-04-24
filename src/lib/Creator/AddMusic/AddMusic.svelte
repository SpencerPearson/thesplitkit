<script>
	import { remoteServer, mainSettings, cachedAlbums } from '$/stores';
	import { onMount } from 'svelte';
	import VirtualList from 'svelte-tiny-virtual-list';
	import Modal from '$lib/Modal/Modal.svelte';
	import SongSelect from './SongSelect.svelte';
	import Spinner from '$lib/loaders/Spinner.svelte';
	export let addFeed = () => {};
	let selectedFeed = {};
	let podcastIndexSearchResults = [];
	let episodes = [];
	let showSongs = false;
	let searchQuery = '';
	let loadError = '';
	let isLoading = false;
	let searchTimer;
	const SEARCH_DEBOUNCE_MS = 320;
	const DEFAULT_RESULT_LIMIT = 120;
	const SEARCH_RESULT_LIMIT = 150;
	let cacheText = '-Results are cached. Click here to fetch latest albums.-';

	onMount(() => {
		if ($cachedAlbums?.length) {
			podcastIndexSearchResults = $cachedAlbums;
		} else {
			runAlbumSearch('', { force: true });
		}
		return () => clearTimeout(searchTimer);
	});

	function sortByPubDate(arr) {
		arr.sort((a, b) => (a.newestItemPubdate < b.newestItemPubdate ? 1 : -1));
		return arr;
	}

	function scheduleSearch(event) {
		searchQuery = event.target.value;
		clearTimeout(searchTimer);
		searchTimer = setTimeout(() => {
			runAlbumSearch(searchQuery);
		}, SEARCH_DEBOUNCE_MS);
	}

	async function runAlbumSearch(term = '', options = {}) {
		loadError = '';
		isLoading = true;

		try {
			const normalizedTerm = term.trim();
			const max = normalizedTerm ? SEARCH_RESULT_LIMIT : DEFAULT_RESULT_LIMIT;
			const res = await fetch(
				remoteServer +
					`/api/queryindex/search?term=${encodeURIComponent(normalizedTerm)}&max=${max}`
			);
			const data = await res.json();
			if (!res.ok) {
				throw new Error(data?.message || data?.description || `queryindex failed (${res.status})`);
			}
			if (!Array.isArray(data?.feeds)) {
				throw new Error('Podcast Index search returned an invalid response.');
			}

			episodes = [];
			podcastIndexSearchResults = sortByPubDate(data.feeds);
			if (!normalizedTerm || options.force) {
				$cachedAlbums = podcastIndexSearchResults;
			}
		} catch (error) {
			console.error('runAlbumSearch error:', error);
			podcastIndexSearchResults = [];
			loadError = error?.message || 'Failed to load albums from Podcast Index.';
		} finally {
			isLoading = false;
		}
	}

	function upsertEpisodes(feedGuid, nextEpisodes) {
		const feed = podcastIndexSearchResults.find((v) => v.podcastGuid === feedGuid);
		if (feed) feed.episodes = nextEpisodes;
		const cached = $cachedAlbums?.find((v) => v.podcastGuid === feedGuid);
		if (cached) cached.episodes = nextEpisodes;
	}

	async function fetchEpisodes(guid, index, feed) {
		if (!feed.episodes) {
			showSongs = true;
			let feedUrl =
				remoteServer + `/api/queryindex?q=${encodeURIComponent(`/podcasts/byguid?guid=${guid}`)}`;
			let episodesUrl =
				remoteServer +
				`/api/queryindex?q=${encodeURIComponent(`/episodes/bypodcastguid?guid=${guid}&max=1000`)}`;

			Promise.all([fetch(feedUrl), fetch(episodesUrl)])
				.then(async ([feedRes, episodesRes]) => {
					let data = await feedRes.json();
					let feed = data.feed;

					let episodesData = await episodesRes.json();
					let episodesResults = [].concat(episodesData.items);

					feed.episodes = episodesResults;

					episodes = feed.episodes || [];
					selectedFeed = feed;

					upsertEpisodes(feed.podcastGuid, episodes);
				})
				.catch((err) => {
					console.error('Error fetching episodes:', err);
				});
		} else {
			showSongs = true;
			selectedFeed =
				podcastIndexSearchResults.find((v) => v.podcastGuid === feed.podcastGuid) || feed;
			episodes = selectedFeed.episodes;
		}
	}

	let listHeight = 500;
	let headerHeight = 16;
	let sectionHeight;
	let sectionWidth;
	let virtualList;
	let scrollToIndex = 0;
	$: if (sectionHeight && headerHeight) {
		listHeight = sectionHeight - headerHeight;
	}
</script>

<albums>
	<albums-top>
		<input class="ui-input" bind:value={searchQuery} on:input={scheduleSearch} placeholder="filter albums" />
		{#if $cachedAlbums}
			<button class="refresh ui-btn ui-btn-muted" on:click={() => runAlbumSearch(searchQuery, { force: true })}>
				{cacheText}
			</button>
		{/if}
	</albums-top>
	<section bind:clientHeight={sectionHeight} bind:clientWidth={sectionWidth}>
		{#if loadError}
			<h2>{loadError}</h2>
		{:else if isLoading}
			<loading-state>
				<Spinner size={36} label="Loading songs" />
			</loading-state>
		{:else if podcastIndexSearchResults.length}
			<VirtualList
				bind:this={virtualList}
				bind:height={listHeight}
				width="100%"
				{scrollToIndex}
				itemCount={podcastIndexSearchResults.length}
				itemSize={podcastIndexSearchResults.map((v) => {
					let charPerRow = Math.floor((sectionWidth - 86) / 14);
					let rows = Math.ceil((v?.title.length || 0) / charPerRow) + 1;
					let titleHeight = 25 * rows;
					return titleHeight + 32;
				})}
				overscanCount={5}
			>
				<div slot="item" let:index let:style {style} class="row">
					<div class="albums">
						<card
							role="button"
							tabindex="0"
							on:click={fetchEpisodes.bind(
								this,
								podcastIndexSearchResults[index]?.podcastGuid,
								index,
								podcastIndexSearchResults[index]
							)}
							on:keydown={(event) => {
								if (event.key === 'Enter' || event.key === ' ') {
									event.preventDefault();
									fetchEpisodes(
										podcastIndexSearchResults[index]?.podcastGuid,
										index,
										podcastIndexSearchResults[index]
									);
								}
							}}
						>
							{#if !$mainSettings?.lowBandwidth?.images}
								<img
									src={podcastIndexSearchResults[index]?.artwork ||
										podcastIndexSearchResults[index]?.image}
									alt={podcastIndexSearchResults[index]?.title}
									width="60"
									height="60"
								/>
							{/if}
							<div>
								<h3>{podcastIndexSearchResults[index]?.title}</h3>
								<p>{podcastIndexSearchResults[index]?.author}</p>

								<p>
									{podcastIndexSearchResults[index]?.newestItemPubdate
										? 'Latest release: ' +
										  new Date(
												podcastIndexSearchResults[index]?.newestItemPubdate * 1000
										  ).toLocaleDateString()
										: ''}
								</p>
							</div>
							<div style={'width: 40px;'} />
						</card>
					</div>
				</div>
			</VirtualList>
		{:else}
			<h2>No results found</h2>
		{/if}
	</section>
</albums>

{#if showSongs}
	<Modal bind:showModal={showSongs}>
		<SongSelect {addFeed} {episodes} feed={selectedFeed} />
	</Modal>
{/if}

<style>
	albums {
		display: flex;
		flex-direction: column;
		overflow: hidden;
		width: calc(100%);
		height: calc(100% - 42px);
	}

	albums-top {
		display: flex;
		flex-direction: column;
		gap: 8px;
		width: calc(100% - 16px);
		position: relative;
		margin: 0 8px;
		min-height: 72px;
	}

	input {
		width: 100%;
		box-sizing: border-box;
	}

	button.refresh {
		align-self: center;
		padding: 6px 12px;
		font-size: 0.85rem;
		min-height: 32px;
	}

	section {
		padding: 0;
		margin: 0 8px;
		width: calc(100% - 8px);
		height: calc(100% - 72px);
		overflow: auto;
		position: relative;
	}

	h2 {
		text-align: center;
		color: var(--md-text);
	}

	loading-state {
		display: flex;
		align-items: center;
		justify-content: center;
		padding-top: 36px;
	}

	h3 {
		margin: 0;
	}
	p {
		margin: 0;
	}

	card {
		display: flex;
		align-items: center;
		padding: 8px;
		box-shadow: var(--md-shadow-soft);
		border-radius: 12px;
		width: calc(100% - 24px);
		margin: 4px 8px;
		background: var(--md-surface);
		border: 1px solid var(--md-border);
		color: var(--md-text);
	}

	img {
		border: 1px solid var(--md-border);
		margin-right: 0.5em;
		border-radius: 6px;
	}
	h3 {
		margin: 0;
	}

	@media (max-width: 799px) {
	}
</style>
