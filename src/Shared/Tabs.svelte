<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let items;
	export let activeItem;
</script>

<div class="tabs">
	<ul>
		{#each items as item}
			<li on:click={() => dispatch('tabChange', item)}>
				<div class:active = {item === activeItem}>{item}</div>
			</li>
		{/each}
	</ul>
</div>

<style>
	.tabs{
		display: inline;
		flex-grow: 1;
		background-color: #202020;
		color: #E65100;
		margin: 0;
		width: auto;
		margin-left: 8%;
	}

	ul{
		display: flex;
		justify-content: left;
		list-style-type: none;
	}

	li{
		margin: 0 0.2em;
		cursor: pointer;
	}

	div{
		padding: 0.4em 0.8em;
		font-weight: bold;
	}

	.active{
		padding: 0.4em 0.8em;
		background-color: #E65100;
		color: #202020;
		border-radius: 0.5em
	}

</style>