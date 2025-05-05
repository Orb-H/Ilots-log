<script lang="ts">
	import type { Score } from '$lib/types/score';
	import { calculateSongRating } from '$lib/utils/rating';
	import { scores } from '$lib/states/score.svelte';

	let { score = $bindable() }: { score: Score } = $props();
	let saveTimeout: number;

	// 점수가 변경될 때마다 해당 차트의 rating만 계산하고 debounce된 저장 실행
	const updateRating = (chart: Score['charts'][0]) => {
		if (chart.score === 0) {
			chart.rating = 0;
		} else if (chart.score !== undefined && chart.score !== null) {
			chart.rating = calculateSongRating(chart.difficultyDecimal, chart.score);
		}

		// Debounce save operation
		clearTimeout(saveTimeout);
		saveTimeout = setTimeout(() => {
			scores.save();
		}, 1000) as unknown as number;
	};
</script>

<div class="score-card">
	<div class="score-header">
		<div class="img-container">
			<img
				class="score-image"
				alt={`File: songs ${score.id}.png`}
				loading="lazy"
				src={`https://wiki.rotaeno.cn/${score.imageUrl}`}
			/>
		</div>
		<div class="score-info">
			<div class="score-title">
				<h3>
					{#each Object.entries(score.title_localized) as info, idx}
						{info[1]}
						{#if idx == Object.entries(score.title_localized).length - 1}{:else}{' / '}
						{/if}
					{/each}
				</h3>
				<span class="score-id">ID: {score.id}</span>
			</div>
			<div class="song-credits">
				<span class="artist">
					<svg
						class="icon"
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 24 24"
						fill="none"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							d="M9 9l10.5-3m0 6.553v3.75a2.25 2.25 0 01-1.632 2.163l-1.32.377a1.803 1.803 0 11-.99-3.467l2.31-.66a2.25 2.25 0 001.632-2.163zm0 0V2.25L9 5.25v10.303m0 0v3.75a2.25 2.25 0 01-1.632 2.163l-1.32.377a1.803 1.803 0 01-.99-3.467l2.31-.66A2.25 2.25 0 009 15.553z"
						/>
					</svg>
					{score.artist}
				</span>
			</div>
		</div>
	</div>

	<div class="charts-container">
		{#each score.charts as chart, idx (chart.difficultyLevel)}
			<div class="chart-row">
				<div class="chart-info">
					<div class="difficulty-badge">
						<span class={`level ${chart.difficultyLevel}`}>{chart.difficultyLevel}</span>
						<span class={`decimal ${chart.difficultyLevel}`}>{chart.difficultyDecimal}</span>
						<span class={`rating ${chart.difficultyLevel}`}>
							({(chart.rating ?? 0).toFixed(3)})
						</span>
					</div>
				</div>
				<div class="score-container">
					<input
						class="score-input"
						type="number"
						bind:value={chart.score}
						oninput={() => updateRating(chart)}
					/>
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	.score-card {
		background: white;
		border-radius: 1rem;
		padding: 1.5rem;
		margin-bottom: 1.5rem;
		box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
	}

	.score-header {
		display: flex;
		gap: 1.5rem;
		margin-bottom: 1.5rem;
	}

	.img-container {
		height: 8rem;
		width: 8rem;
		overflow: hidden;
		border-radius: 50%;
		flex-shrink: 0;
		box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
	}

	.img-container > img {
		height: 100%;
		width: 100%;
		object-fit: cover;
	}

	.score-info {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		flex: 1;
	}

	.score-title {
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	.score-title h3 {
		margin: 0;
		font-size: 1.5rem;
		font-weight: 600;
		color: #1a1a1a;
		overflow-wrap: break-word;
	}

	.score-id {
		color: #666;
		font-size: 0.875rem;
	}

	.song-credits {
		display: flex;
		gap: 1rem;
		color: #4b5563;
		font-size: 0.875rem;
	}

	.artist,
	.chart-designer {
		display: flex;
		align-items: center;
		gap: 0.25rem;
	}

	.icon {
		width: 1rem;
		height: 1rem;
	}

	.charts-container {
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.chart-row {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0.5rem 15px;
		border-radius: 0.5rem;
		background: #f8f9fa;
		width: calc(100% - 15px);
	}

	.chart-info {
		display: flex;
		align-items: center;
		flex: 1;
	}

	.difficulty-badge {
		display: flex;
		align-items: center;
		gap: 0.5rem;
	}

	.level {
		font-weight: 600;
		font-size: 1.25rem;
	}

	.decimal {
		padding: 0.25rem 0.5rem;
		border-radius: 0.25rem;
		font-weight: 500;
	}

	.rating {
		font-size: 0.875rem;
		font-weight: 500;
		color: inherit;
	}

	.chart-designer {
		color: #4b5563;
		font-size: 0.875rem;
	}

	.score-container {
		display: flex;
		align-items: center;
		justify-content: flex-end;
		max-width: 40%;
	}

	.calculated-score {
		min-width: 4rem;
		text-align: right;
		font-weight: 600;
		color: #1a1a1a;
		font-size: 1.125rem;
	}

	.score-input {
		width: 70%;
		padding: 0.5rem 0.75rem;
		border: 1px solid #e2e8f0;
		border-radius: 0.375rem;
		font-size: 1.125rem;
		text-align: right;
	}

	.score-input:focus {
		outline: none;
		border-color: #3b90ff;
		color: #1cda1b;
	}

	.level.I {
		color: #1cda1b;
	}
	.decimal.I {
		background-color: #1cda1b;
	}

	.level.II {
		color: #3b90ff;
	}
	.decimal.II {
		background-color: #3b90ff;
	}

	.level.III {
		color: #ef960d;
	}
	.decimal.III {
		background-color: #ef960d;
	}

	.level.IV {
		color: #dc43e5;
	}
	.decimal.IV {
		background-color: #dc43e5;
	}

	.level.IV-α {
		color: #bd6eff;
	}
	.decimal.IV-α {
		background-color: #bd6eff;
	}

	.rating.I {
		color: #1cda1b;
	}
	.rating.II {
		color: #3b90ff;
	}
	.rating.III {
		color: #ef960d;
	}
	.rating.IV {
		color: #dc43e5;
	}
	.rating.IV-α {
		color: #bd6eff;
	}

	/* 모바일 대응 */
	@media (max-width: 640px) {
		.score-header {
			flex-direction: column;
			align-items: center;
		}
		.chart-info {
			flex-direction: column;
			align-items: flex-start;
		}

		.score-info {
			width: 100%;
		}
	}
</style>
