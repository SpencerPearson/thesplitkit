<blurred-background>
	<modal>
		<modal-content>
			<slot />
		</modal-content>
	</modal>
</blurred-background>

<style>
	blurred-background {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
		position: absolute;
		background: color-mix(in oklab, var(--md-bg), transparent 65%);
		top: 0;
		right: 0;
		z-index: 101;
		backdrop-filter: blur(5px);
	}

	modal {
		display: block;
		position: relative;
		padding: 20px;
		background-color: var(--md-surface-elevated);
		border-radius: 14px;
		border: 1px solid var(--md-border);
		color: var(--md-text);
		box-shadow: var(--md-shadow-soft);
	}
</style>
