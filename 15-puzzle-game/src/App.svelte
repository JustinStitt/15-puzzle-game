<script>
import { missing_component } from "svelte/internal";

	import Grid from "./components/Grid.svelte"
	export let size = 9;

	let value = 1
	let user_def = 3

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

	const getNearestSquare = (n) => {
		var f = Math.floor(n)
		var c = Math.ceil(n)
		var f2 = f*f
		var c2 = c*c
		return (Math.abs(n-f2) > Math.abs(n-c2)) ? c2 : f2
	}

	$: {
		size = getNearestSquare(user_def)
	}


</script>

<main>
	<h1>Solve</h1>
	<div class='outer'>
		<Grid {size}/>
		<div class='options'>
			{#each options as option}
			<button on:click={() => size=option.sz}>
				{option.name}
			</button>
			{/each}
			<input min=3 placeholder='Custom' type="number" bind:value={user_def}/>
		</div>
	</div>
	
</main>

<style>
	input {
		background-color: #48919e;
		color: aliceblue;
		border-radius: .5rem;
		padding: .5rem;
		margin: .5rem;
		width: 60%;
	}
	::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
  		color: aliceblue;
 	 	opacity: 1; /* Firefox */
	}
	button {
		color: aliceblue;
		background-color: #477998;
		border-radius: .5rem;
		padding: .5rem;
		margin: .5rem;
		width: 60%;
	}

	.options {
		display: flex;
		width: 26%;
	}

	main {
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
	}
	.outer{
		display: flex;
		flex-direction: column;
		align-items: center;
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