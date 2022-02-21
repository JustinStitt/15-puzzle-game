<script>
	import Grid from "./components/Grid.svelte"
    const a = '\xa0'
	export let size = 9;

	let value = 1

	const options = [
		{
			name: '3x3',
			sz: 9
		},
		{
			name: '4x4',
			sz: 16
		},
		{
			name: '5x5',
			sz: 25
		},
	]

</script>

<main>
	<h1>Solve</h1>
	<div class='outer'>
		<Grid {size}/>
		<!-- <input type='text' bind:value={size}/> -->
	</div>

	{#each options as option}
		<button on:click={() => size=option.sz}>
			{option.name}
		</button>
	{/each}
</main>

<style>
	main {
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
	}
	.outer{
		display: flex;
		justify-content: center;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>