<script lang="ts">
	import { scores } from '$lib/states/score.svelte';
</script>

<div class="home-container">
	<section class="hero">
		<h1>Ilot's Log</h1>
		<p class="subtitle">
			A rating calculator and song recommender for Rotaeno players.<br /> Based on the music added
			in Version {scores.scores.at(-1)?.releaseVersion}.
		</p>
	</section>

	<section class="features">
		<h2>Features</h2>
		<div class="feature-grid">
			<a href="/best" class="feature-card">
				<h3>/best</h3>
				<p>View and track your top 40 ratings with detailed performance statistics.</p>
			</a>
			<a href="/report" class="feature-card">
				<h3>/report</h3>
				<p>Get personalized song recommendations to improve your overall rating.</p>
			</a>
			<a href="/scores" class="feature-card">
				<h3>/scores</h3>
				<p>Easily input and manage your rhythm game scores.</p>
			</a>
			<a href="/backup" class="feature-card">
				<h3>/backup</h3>
				<p>Securely backup and restore your progress data.</p>
			</a>
		</div>
	</section>

	<section class="development">
		<h2>Development</h2>
		<div class="info-card">
			<p>
				Source code: <a href="https://github.com/dodok8/Ilots-log"
					>https://github.com/dodok8/Ilots-log</a
				>
			</p>
			<p>Bug reports and contributions are always welcome.</p>
		</div>
	</section>

	<section class="credits">
		<h2>Credits</h2>
		<div class="info-card">
			<p>Song data source: <a href="https://wiki.rotaeno.cn">Rotaeno Chinese Wiki</a></p>
			<p>
				Special thanks to GalvinGao's excellent <a href="https://rotaeno.imgg.dev/">RotaenoKit</a>.
				This site was developed with inspiration from GalvinGao's website's features.
			</p>
		</div>
	</section>
</div>

<style>
	.home-container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 2rem;
	}

	section {
		margin-bottom: 3rem;
	}

	.hero {
		text-align: center;
		padding: 3rem 0;
	}

	.hero h1 {
		font-size: 2.5rem;
		color: #1a1a1a;
		margin-bottom: 1rem;
	}

	.subtitle {
		font-size: 1.1rem;
		color: #4b5563;
		max-width: 600px;
		margin: 0 auto;
	}

	h2 {
		font-size: 1.75rem;
		color: #1a1a1a;
		margin-bottom: 1.5rem;
	}

	.feature-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		gap: 1.5rem;
	}

	.feature-card {
		background: white;
		padding: 1.5rem;
		border-radius: 0.75rem;
		box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
		transition:
			transform 0.2s,
			box-shadow 0.2s;
	}

	.feature-card:hover {
		transform: translateY(-2px);
		box-shadow: 0 6px 8px -1px rgb(0 0 0 / 0.15);
	}

	.feature-card h3 {
		font-size: 1.25rem;
		color: #2563eb;
		margin-bottom: 0.75rem;
	}

	.feature-card p {
		color: #4b5563;
		line-height: 1.5;
	}

	.info-card {
		background: white;
		padding: 1.5rem;
		border-radius: 0.75rem;
		box-shadow: 0 2px 4px -1px rgb(0 0 0 / 0.1);
	}

	.info-card p {
		margin: 0.5rem 0;
		color: #4b5563;
	}

	a {
		color: #2563eb;
		text-decoration: none;
		transition: color 0.2s;
	}

	a:hover {
		color: #1d4ed8;
		text-decoration: underline;
	}

	@media (max-width: 768px) {
		.home-container {
			padding: 1rem;
		}

		.hero {
			padding: 2rem 0;
		}

		.hero h1 {
			font-size: 2rem;
		}

		.feature-grid {
			grid-template-columns: 1fr;
		}
	}
</style>
