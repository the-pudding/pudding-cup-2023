<script>
	import rawData from "$data/projects.csv";
	import Project from "$components/Project.svelte";
	import ProjectGroup from "$components/ProjectGroup.svelte";

	const data = rawData.map((d) => ({
		...d,
		year: +d.year
	}));

	export let category;
	let year =
		category == "current"
			? Math.max(...data.map((d) => d.year))
			: Math.max(...data.map((d) => d.year)) - 1;
	const maxYear = Math.max(...data.map((d) => d.year));

	$: years = [...new Set(data.map((d) => d.year))];
	$: projects = data.filter((d) => d.year === year);
</script>

{#if category == "archive"}
	<div>
		<p>See past winners from</p>
		<select bind:value={year}>
			{#each years as y}
				{#if y < maxYear}
					<option value={y}>{y}</option>
				{/if}
			{/each}
		</select>
	</div>
{/if}

<ProjectGroup {projects} {year} />

<style>
	div {
		position: sticky;
		top: 0;
		width: 100%;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		background: var(--color-gray-100);
		padding: 16px 0;
		border-bottom: 1px solid var(--color-gray-200);
		z-index: var(--z-overlay);
	}
	p {
		font-family: var(--sans);
		padding: 0;
		margin: 0;
	}
	select {
		margin-left: 8px;
		font-weight: 700;
	}
</style>
