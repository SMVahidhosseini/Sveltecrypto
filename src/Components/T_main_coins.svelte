<script>
	import { onMount } from 'svelte';
	import { coinStore, pageCoins } from '../Stores/coinStore.js';
	import sigDigit from '../Stores/sigDigits.js';
	export let pageCoinNo;
	export let pageNo;
	export let getPageCoins;
	//______________________________________________________________________
	// async function getPageCoins(page) {
	// 	const res = await fetch(`https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=${pageCoinNo}&page=${pageNo}&sparkline=false&price_change_percentage=1h%2C24h%2C7d`);
	// 	$pageCoins = await res.json();
	// }

	onMount(() => getPageCoins({pageNo}));
	//______________________________________________________________________
</script>

<!-- {#await getcoinsdata()} -->
<!-- {:then coins} -->
{#each $pageCoins as coin (coin.id)}
	<div class="main">
		{#if coin.market_cap_rank === null}
			<div class="onec">?</div>
		{:else}
			<div class="onec">{coin.market_cap_rank}</div>
		{/if}
		<div class="Table">
			<a href="/#/coins/{coin.id}" target="_blank" style="text-decoration:none; color: black;">
				<table>
					<tr>
						<td rowspan="2" class="breaked_logo"><img src={(coin.image).replace("large", "thumb")} width="30vw" height="30vw"></td>
						<td class="breaked_symbol">{coin.symbol}</td>
					</tr>
					<tr>
						<td class="breaked_name">{coin.name}</td>
					</tr>
				</table>
			</a>
		</div>
		<div class="afteronec">{sigDigit(coin.current_price)}</div>
		<div class="percentage">
			<div class="afteronec" class:positive={(coin.price_change_percentage_1h_in_currency >= 0)} class:negative={(coin.price_change_percentage_1h_in_currency < 0)}>{coin.price_change_percentage_1h_in_currency.toFixed(1)}%</div>
			<div class="afteronec" class:positive={(coin.price_change_percentage_24h >= 0)} class:negative={(coin.price_change_percentage_24h < 0)}>{coin.price_change_percentage_24h.toFixed(1)}%</div>
			<div class="afteronec" class:positive={(coin.price_change_percentage_7d_in_currency >= 0)} class:negative={(coin.price_change_percentage_7d_in_currency < 0)}>{coin.price_change_percentage_7d_in_currency.toFixed(1)}%</div>
		</div>
		<div class="afteronec">{sigDigit(coin.market_cap)}</div>
		<div class="afteronec">{sigDigit(coin.total_volume)}</div>
	</div>
{/each}

<!-- {/await} -->

<style>
	div.main{
		font-size: 0.8em;
		min-height: 2em;
		line-height: 2.5em;
		display: flex;
		flex-direction: row, column;
		color: black;
		padding: 0.2em;
		text-align: center;
		margin-left: 0.8em;
		margin-right: 0.8em;
		border-bottom: 0.1em solid lightgray;
		border-radius: 1.5em;

	}
	/*Div onec for adding padding to the left only for 1st column*/
	div.onec{
		padding-left: 2em;
		text-align: left;
		display: inline;
		flex-grow: 1;
		width: 3%;
	}

	div.afteronec{
		text-align: left;
		display: inline;
		flex-grow: 1;
		width: 11.5%;
	}

	div.Table{
		text-align: left;
		display: inline;
		flex-grow: 1;
		width: 11.5%;
		min-width: 90px;
	}

	.breaked_logo{
		font-size: 1.2em;
		height: 0.7em;
		line-height: 1em;
		vertical-align: middle;
	}

	.breaked_symbol{
		font-size: 1em;
		height: 1em;
		line-height: 0.8em;
		font-weight: bold;
	}

	.breaked_name{
		font-size: 0.8em;
		height: auto;
		line-height: 0.8em;
	}

	div.percentage{
		width: 20%;
		margin: 0;
		text-align: center;
		display: flex;
		flex-direction: row, column;
		padding-right: 7em;
	}

	div.positive {
		text-align: center;
		display: inline;
		flex-grow: 1;
		width: 6%;
		min-width: 55px;
		background-color: #37fd12;
		color: black;
		border-radius: 1.5em;
		margin: 0.2em;
		display: inline;
		flex-grow: 1;
	}

	div.negative {
		text-align: center;
		display: inline;
		flex-grow: 1;
		width: 6%;
		min-width: 55px;
		background-color: #fc0003;
		color: white;
		border-radius: 1.5em;
		margin: 0.2em;
		display: inline;
		flex-grow: 1;
	}
	
</style>