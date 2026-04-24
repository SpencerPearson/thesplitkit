<script>
	import { mainSettings } from '$/stores';
</script>

<div class="iframe-container">
	<iframe title="Boost board" src={$mainSettings?.boostBoard || ''} />
</div>

<style>
	.iframe-container {
		width: 100%; /* Mobile width */
		height: 100%; /* Mobile height */
		overflow: hidden;
		position: relative;
		display: flex;
		align-items: center;
		justify-content: center;
		border-radius: 14px;
		background: color-mix(in oklab, var(--md-surface), transparent 12%);
		border: 1px solid var(--md-border);
		box-shadow: var(--md-shadow-soft);
	}

	.iframe-container iframe {
		border: none;
		height: 100%;
		width: 100%;
		background: var(--md-surface);
	}
</style>
