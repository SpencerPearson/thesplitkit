<script>
	import { mainSettings } from '$/stores';
</script>

{#if $mainSettings?.liveChatEnclosure}
	<div>
		<img src="/chat-top3.png" alt="Chat frame top" />
		<iframe title="Live chat" src={$mainSettings?.liveChatEnclosure || ''} />
		<img class="bottom" src="/chat-bottom.png" alt="Chat frame bottom" />
	</div>
{/if}

<style>
	div {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		position: relative;
	}

	img {
		width: 100%;
		opacity: 0.9;
	}
	iframe {
		width: calc(100% - 6px);
		border: none;
		border-left: 4px solid color-mix(in oklab, var(--md-surface-elevated), black 35%);
		border-right: 3px solid color-mix(in oklab, var(--md-surface-elevated), black 35%);
		flex-grow: 1;
		border-radius: 0 0 8px 8px;
		background: var(--md-surface);
	}

	.bottom {
		position: absolute;
		bottom: 0;
		width: 100%;
	}
</style>
