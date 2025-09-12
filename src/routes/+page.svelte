<script lang="ts">
	import leagues from '$lib/assets/leagues.json';

	// Format league name nicely
	function formatName(raw: string) {
		return raw
			.split('-')
			.map((w) => w.charAt(0).toUpperCase() + w.slice(1))
			.join(' ');
	}

	// Format datetime
	function formatDate(dt: string) {
		const d = new Date(dt.replace(' ', 'T'));
		return d.toLocaleString(undefined, {
			weekday: 'short',
			day: 'numeric',
			month: 'short',
			hour: '2-digit',
			minute: '2-digit'
		});
	}

	// Helper: get prediction keys dynamically
	function getPredictionKeys(pred: Record<string, any>) {
		return Object.keys(pred).filter(
			(k) => !k.startsWith('odds') && !['datetime', 'home_team', 'away_team'].includes(k)
		);
	}
</script>

<h1>FootAI - alpha</h1>

{#each leagues as league}
	<section>
		<h2>{formatName(league.name)}</h2>

		<div class="league-content">
			<div class="fixtures-container">
				<h3>Upcoming Fixtures</h3>
				<ul class="fixtures">
					{#each league.preds as p}
						<li>
							<div class="fixture-header">
								<strong>{formatDate(p.datetime)}</strong> — {p.home_team} vs {p.away_team}
							</div>
							<div class="predictions">
								<b>Predictions:</b>
								{#each getPredictionKeys(p) as key}
									<span>{key}: {p[key]}</span>
								{/each}
							</div>
							<div class="odds">
								<b>Odds:</b> H {p.odds_home_win} | D {p.odds_draw} | A {p.odds_away_win}
							</div>
						</li>
					{/each}
				</ul>
			</div>
			<div class="standings-container">
				<h3>Standings</h3>
				<table class="standings">
					<thead>
						<tr>
							<th>Team</th>
							<th>Pts</th>
							<th>P</th>
							<th>W</th>
							<th>D</th>
							<th>L</th>
							<th>GD</th>
							<th>Form</th>
						</tr>
					</thead>
					<tbody>
						{#each league.standings as s}
							<tr>
								<td>{s.team}</td>
								<td>{s.points}</td>
								<td>{s.p}</td>
								<td>{s.w}</td>
								<td>{s.d}</td>
								<td>{s.l}</td>
								<td>{s.gd}</td>
								<td>{s.form}</td>
							</tr>
						{/each}
					</tbody>
				</table>
			</div>
		</div>
	</section>
{/each}

<style>
	h1 {
		text-align: center;
		margin-bottom: 2rem;
	}

	section {
		margin: 0 auto 3rem;
		max-width: 1200px; /* prevent full-width stretching */
		padding: 0 1rem;
	}

	h2 {
		color: #2a4d8f;
		margin-bottom: 0.5rem;
	}

	h3 {
		margin: 1rem 0 0.5rem;
		color: #444;
	}

	/* Flex layout for standings + fixtures */
	.league-content {
		display: flex;
		gap: 2rem;
		flex-wrap: wrap; /* stack on narrow screens */
	}

	.standings-container,
	.fixtures-container {
		flex: 1;
		min-width: 300px; /* keep them from squishing */
	}

	table.standings {
		width: 100%;
		border-collapse: collapse;
		margin-bottom: 1rem;
		font-size: 0.9rem;
	}

	table.standings th,
	table.standings td {
		border: 1px solid #ccc;
		padding: 0.4rem 0.6rem;
		text-align: center;
	}

	table.standings th {
		background: #f2f2f2;
	}

	ul.fixtures {
		list-style: none;
		padding: 0;
	}

	ul.fixtures li {
		margin-bottom: 1rem;
		padding: 0.6rem;
		border: 1px solid #ddd;
		border-radius: 6px;
		background: #fafafa;
	}

	.fixture-header {
		font-weight: bold;
		margin-bottom: 0.3rem;
	}

	.predictions span {
		display: inline-block;
		margin-right: 0.8rem;
	}

	.odds {
		margin-top: 0.3rem;
		font-size: 0.9rem;
		color: #555;
	}
</style>
