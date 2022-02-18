<script>
	import { coinStore } from '../Stores/coinStore.js';
	import {clickOutside} from '../Stores/clickOutside.js';

	let search = '';
	let results = [];
	let showSearch = false;

	const getSearchResults = (e) => {
		showSearch = true;
		search = e.target.value;		
		results = $coinStore.filter(object => {
			if (search != "") {
				let arr = (object.symbol.indexOf(search.toLowerCase()) === 0 || object.id.indexOf(search.toLowerCase()) === 0);
				return arr;
				showSearch = false;
			}
		});
	}

	function handleClickOutside(event) {
		showSearch = false;
		search = '';
	}

</script>

<div use:clickOutside on:click_outside={handleClickOutside}>
	<input class="searchbar" type="text" placeholder=" &#128270; Search" autocomplete="off" bind:value = {search} on:input={getSearchResults}>
	{#if showSearch}
		<div class="searchlist">
			{#each results as coin (coin.id)}
				<a href="/#/coins/{coin.id}" target="_blank" style="text-decoration:none; color: black;">
					<div class="searchlist2" >
						<img src={(coin.image).replace("large", "thumb")} width="20vw" height="20vw">
						<div class = "divinfo">
							&nbsp;{coin.name} <span style="font-size:0.8em;"><strong>({coin.symbol})</strong> &nbsp; #{coin.market_cap_rank} </span>
						</div>
					</div><br>
				</a>
			{/each}
		</div>
	{/if}
</div>


<style>

	.searchbar{
		background-color: lightgray;
		border-radius: 0.7em;
		border-color: gray;
		width: 15em;
		color: #202020;
		margin-bottom: 0.1em;
	}

	.searchbar:focus{
		background-color: lightgray;
		border-radius: 0.7em;
		border-color: gray;
		width: 15em;
		color: #202020;
		outline: none;
	}

	.searchlist{
		background-color: #f5f5f5;
		max-height: 20em;
		width: 15em;
		overflow-y: auto;
		overflow-x: hidden;
		border-radius: 0.4em;
		font-size: 1em;
		box-shadow:
		-0.2em 0.2em 0.3em 0.1em #202020;
	}

	.searchlist2{
		background-color: #f5f5f5;
		overflow-y: auto;
		overflow-x: hidden;
		padding: 0.2em;
		margin-left: 0.3em;
		margin-top: 0.25em;
		width: auto;
		font-size: 0.9em;
		border-bottom: 0.1em solid #E65100;
		display: inline-flex;
		box-shadow:
		0 4px 6px -6px #E65100;
	}

	.divinfo{
		margin: auto;
		margin-top: 0;
	}

	::-webkit-scrollbar {
	  width: 0.4em;
	}

	/* Track */
	::-webkit-scrollbar-track {
	  box-shadow: inset 0 0 5px grey; 
	  border-radius: 0.4em;
	}
	 
	/* Handle */
	::-webkit-scrollbar-thumb {
	  background: #E65100; 
	  border-radius: 0.4em;
	}

	/* Handle on hover */
	::-webkit-scrollbar-thumb:hover {
	  background: #b30000; 
	}
	
</style>