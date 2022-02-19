<script>
	export let name;
	export let id;
	$: both = name + ' ' + id // macro

	let users = [
		{
			name: 'Justin',
			id: 1,
		},
		{
			name: 'Mark',
			id: 2,
		}
	]

	const toggle = () => {
		id += 1
	}

	const isEven = (n) => {
		return !(n & 1)
	}

	const create_user = (_name) => {
		users = [...users, {name: _name, id: users[users.length-1].id+1}]
	}

</script>

<main>
	<h1>Hello {both} </h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
	<button on:click={toggle}>Click</button>
	{#if isEven(id)}
		<h4>
			Even!
		</h4>
	{/if}
	<h2>Users: </h2>
	{#each users as user}
		<h3>{user.name}: {user.id}</h3>
	{/each}
	<input type="text" bind:value={name}/>
	<button on:click={create_user(name)}>Add User</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
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