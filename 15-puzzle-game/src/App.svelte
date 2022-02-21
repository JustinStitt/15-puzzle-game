<script>
import { fix_and_destroy_block, missing_component } from "svelte/internal";

	import Grid from "./components/Grid.svelte"
	export let size = 9

	let value = 1
	let user_def = 3
	let win = false

	class Rect {
		constructor(x, y) { 
			this.x = x
			this.y = y
		}
	}

	const options = [
		{
			name: '3x3',
			sz: 9,
		},
		{
			name: '4x4',
			sz: 16,
		},
		{
			name: '5x5',
			sz: 25,
		},
	]

	const getNearestSquare = (n) => {
		var f = Math.floor(n)
		var c = Math.ceil(n)
		var f2 = f*f
		var c2 = c*c
		return (Math.abs(n-f2) > Math.abs(n-c2)) ? c2 : f2
	}

	const handleWin = (event) => {
		win = event.detail.win
	}

	$: {
		size = getNearestSquare(user_def)
	}

</script>

<main>
	<h1 class:win={win}>Solve{#if win}d{/if}</h1>
	
	<!-- <h2 class='win' class:show={win}>Win</h2> -->
	<div class='outer'>
		<Grid {size} on:win={handleWin}/>
		<div class='options'>
			{#each options as option}
			<button on:click={() => size=option.sz}>
				{option.name}
			</button>
			{/each}
			<input max=10 min=3 placeholder='Custom' type="number" bind:value={user_def}/>
		</div>
		<!-- <button on:click={() => win=true}/> -->
	</div>
	
</main>

<style>
	.win {
		width: 30%;
		color: burlywood;
		background-color: darkolivegreen;
		border-radius: 3rem;
		border-color: aliceblue solid 2px;
	}

	/* .win.show {
		visibility: visible;
		margin: 0 auto;
		width: 30%;
		color: burlywood;
		background-color: darkolivegreen;
		border-radius: 3rem;
		border-color: aliceblue solid 2px;
	} */

	input {
		background-color: #48919e;
		color: aliceblue;
		border-radius: .5rem;
		padding: .5rem;
		margin: .5rem;
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
		width: 100%;
	}

	main {
		display: flex;
		flex-direction: column;
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
		justify-content: center;
		align-items: center;
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
		margin-bottom: .25rem;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>