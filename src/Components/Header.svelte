<script>
	import { createEventDispatcher } from 'svelte';
	import { onMount } from 'svelte';
	import { coinStore, pageCoins } from '../Stores/coinStore.js';
	import Tabs from '../Shared/Tabs.svelte';
  import Searchbar from './Searchbar.svelte';

	export let items;
	export let activeItem;
	export let coinperpageshow;
	export let pageCoinNo;
	export let pageNo;
	export let activePage;
	const dispatch = createEventDispatcher();

	async function fillCoinStore(num, page) {
		for (var i = 1; i <= page; i++) {
			const res = await fetch(`https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=${num}&page=${i}&sparkline=false&price_change_percentage=1h%2C24h%2C7d`);
			let coins = await res.json();
			$coinStore = [...$coinStore, ...coins];
		}
	}

	onMount(() => fillCoinStore(250, 3));

</script>

<header>
  <div class="topdiv">
    <Searchbar />
  </div>
	<h1>Cryptocurrency Info</h1>

	<div class="headercomponents">
		<div class="divperpage">
			<form class:perpagedispnone = {coinperpageshow} class="formperpage" on:submit|preventDefault = {() => dispatch('getPageCoins', {pageCoinNo, activePage})}>
				<label class="labelperpage">Coins Per Page</label>
				<input class="inputperpage" type="number" name="CoinsPerPage" bind:value = {pageCoinNo}>
				<input class="submitperpage" type="submit" value="&#8635;">
			</form>
		</div>
		<Tabs {items} {activeItem} on:tabChange />
	</div>
</header>

<style>
  header{
    background: #202020;
    padding-top: 2em;
    padding-bottom: 0.5em;
  }

  div.topdiv{
    width: 100%;
    padding-left: 1em;
    color: #202020;
    max-height: 40px;
    display: block;
    width: 12em;
    z-index: 3;
    position: absolute;
    background-color: #202020;
  }

  h1{
  	display: block;
    color: #E65100;
    text-align: center;
    z-index: 2;
    position: relative;
    margin-top: 1.2em;
  }

  div.headercomponents{
  	display: flex;
  	flex-direction: row, column;
  	text-align: center;
  	background: #202020;
  	width: 100%;
  }

  div.divperpage{
	display: flex;
	flex-direction: row, column;
  	width: 30%;
	margin-left: 2%;
	text-align: left;
  }

  .formperpage{
  	height: auto;
  	flex-direction: row, column;
  	padding: 0;
  	margin: 0;
  	flex-grow: 1;
  }

  .perpagedispnone{
  	display: none;
  	height: auto;
  	flex-direction: row, column;
  	padding: 0;
  	margin: 0;
  	flex-grow: 1;
  }

  .labelperpage{
  	margin-top: 0.7em;
  	flex-grow: 1;
  	color: #E65100;
  	font-weight: bold;
  }

  .inputperpage{
  	height: 1.5em;
  	line-height: 1.5em;
  	width: 5em;
  	font-family: times;
  	font-weight: bold;
  	color: black;
  	background-color: #d3d3d3;
  	border-radius: 0.4em;
  }

  .submitperpage{
  	color: #E65100;
  	background-color: #202020;
  	border-color: #202020;
  	font-weight: bold;
  	font-size: 1.2em;
  	padding: 0;
    cursor: pointer;
  }
</style>