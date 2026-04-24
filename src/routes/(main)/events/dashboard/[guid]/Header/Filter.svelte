<script>
	import MusicIcon from '$lib/icons/Music.svelte';
	import PersonIcon from '$lib/icons/Person.svelte';
	import ChapterIcon from '$lib/icons/Chapter.svelte';
	import PodcastIcon from '$lib/icons/Podcast.svelte';

	export let filterType;
	export let showFilterModal;

	let modalsConfig = {
		podcast: { show: false },
		chapter: { show: false },
		person: { show: false },
		music: { show: false }
		// add other modals here
	};

	function resetModals() {
		for (let key in modalsConfig) {
			modalsConfig[key].show = false;
		}
	}

	function handleSelect(type) {
		resetModals();

		if (modalsConfig.hasOwnProperty(type) || type === 'off') {
			filterType = type;
			showFilterModal = false;
		} else {
			console.error(`Modal type ${type} is not defined`);
		}
	}
</script>

<h2>Filter By:</h2>

<button on:click={handleSelect.bind(this, 'off')}>
	<icon> Off </icon>
	No Filters
</button>
<div>
	<button on:click={handleSelect.bind(this, 'chapter')}>
		<icon>
			<ChapterIcon size="40" />
		</icon>
		Chapter
	</button>
	<button on:click={handleSelect.bind(this, 'music')}>
		<icon>
			<MusicIcon size="40" />
		</icon>
		Music
	</button>
	<button on:click={handleSelect.bind(this, 'podcast')}>
		<icon>
			<PodcastIcon size="40" />
		</icon>
		Podcast
	</button>
	<button on:click={handleSelect.bind(this, 'person')}>
		<icon>
			<PersonIcon size="40" />
		</icon>
		Person
	</button>
</div>

<style>
	div {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 10px;
	}

	button {
		box-shadow: none;
		background-color: transparent;
		color: var(--md-text);
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 6px;
	}

	icon {
		display: block;
		width: 50px;
		height: 50px;
		background-color: var(--md-surface-soft);
		color: var(--md-text);
		border: 1px solid var(--md-border);
		border-radius: 25px;
		box-shadow: var(--md-shadow-soft);
		margin-bottom: 4px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	h2 {
		color: var(--md-text);
		margin: 0 0 6px;
	}
</style>
