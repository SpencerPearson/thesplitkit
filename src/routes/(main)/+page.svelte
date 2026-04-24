<section class="landing">
	<div class="hero page-surface">
		<div class="hero-left">
			<div class="kicker">Podcasting 2.0 Live Splits</div>
			<h2>Design and switch live value blocks in seconds.</h2>
			<p>
				The Split Kit gives podcasters a smooth control room for live `valueTimeSplit` workflows:
				build split blocks, switch the active destination during the show, and keep listeners boosting
				the right people.
			</p>
			<div class="actions">
				<a class="button-link primary" href="/login">Log In</a>
				<a class="button-link secondary" href="/events">View Events</a>
			</div>
		</div>
		<div class="hero-right">
			<img src="/splitB300.png" alt="The Split Kit logo" />
		</div>
	</div>

	<div class="flow page-surface">
		<h3>How it works</h3>
		<ol>
			<li>
				<strong>Build value blocks</strong>
				<span>Define recipients, percentages, and metadata for each show state.</span>
			</li>
			<li>
				<strong>Attach to event pages</strong>
				<span>Pair artwork, links, and split configurations for promotion and playback.</span>
			</li>
			<li>
				<strong>Switch live without stress</strong>
				<span>Move between host blocks, artist blocks, and custom moments as the show evolves.</span>
			</li>
			<li>
				<strong>Receive and route boostagrams</strong>
				<span>Supporters send sats + messages while payouts follow your active split logic.</span>
			</li>
		</ol>
	</div>
</section>

<style>
	.landing {
		overflow: auto;
		padding: 12px 8px 20px;
		display: flex;
		flex-direction: column;
		gap: 14px;
	}

	.hero,
	.flow {
		padding: 20px;
	}

	.hero {
		display: grid;
		grid-template-columns: 1.25fr 0.75fr;
		gap: 18px;
		align-items: center;
	}

	.kicker {
		font-size: 0.8rem;
		font-weight: 700;
		letter-spacing: 0.12em;
		text-transform: uppercase;
		color: var(--color-theme-blue);
	}

	h2 {
		margin: 0 0 10px;
		font-size: clamp(1.5rem, 2.8vw, 2.2rem);
	}

	p {
		margin: 0;
		font-size: 1rem;
		line-height: 1.55;
		color: var(--color-text-1);
	}

	.actions {
		display: flex;
		flex-wrap: wrap;
		gap: 10px;
		margin-top: 18px;
	}

	.button-link {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		text-decoration: none;
		border-radius: 999px;
		padding: 10px 18px;
		font-weight: 600;
		border: 1px solid transparent;
	}

	.button-link.primary {
		background: var(--md-primary);
		color: var(--md-on-primary);
		box-shadow: var(--md-shadow-soft);
	}

	.button-link.secondary {
		background: var(--md-surface-soft);
		color: var(--md-text);
		border-color: var(--md-border);
	}

	.hero-right {
		display: flex;
		justify-content: center;
	}

	img {
		object-fit: contain;
		width: min(320px, 100%);
		filter: drop-shadow(0 8px 16px rgba(0, 0, 0, 0.22));
	}

	h3 {
		margin: 0 0 14px;
		font-size: 1.2rem;
	}

	ol {
		padding: 0;
		margin: 0;
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
		gap: 10px;
	}

	li {
		list-style: none;
		background: var(--md-surface-soft);
		border: 1px solid var(--md-border);
		border-radius: 14px;
		padding: 12px;
		display: flex;
		flex-direction: column;
		gap: 6px;
	}

	li strong {
		font-size: 0.95rem;
	}

	li span {
		color: var(--color-text-1);
		font-size: 0.9rem;
		line-height: 1.4;
	}

	@media (max-width: 900px) {
		.hero {
			grid-template-columns: 1fr;
		}
	}
</style>
