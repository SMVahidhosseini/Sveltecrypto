<script>
	import TradingViewWidget from "svelte-tradingview-widget";
	import { coinStore, pageCoins } from '../Stores/coinStore.js';
	import { onMount } from 'svelte';
	import Searchbar from './Searchbar.svelte';
	import sigDigit from '../Stores/sigDigits.js';

	export let params;
	let coinid = params.coinid;
	let time2;
	let options;

	async function fillCoinStore(num, page) {
		for (var i = 1; i <= page; i++) {
			const res = await fetch(`https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=${num}&page=${i}&sparkline=false&price_change_percentage=1h%2C24h%2C7d`);
			let coins = await res.json();
			$coinStore = [...$coinStore, ...coins];
		}
	}

	onMount(() => fillCoinStore(250, 3));

	async function fillCoinData() {
		const res = await fetch(`https://api.coingecko.com/api/v3/coins/${coinid}?community_data=true`);
		const coin = await res.json();
		let time = new Date((coin.market_data.ath_date.usd).split('T')[0]);
		time2 = await time.toDateString();
		options =  await { symbol: `BINANCE:${coin.symbol}USDT`, theme: "light", autosize: true, locale: "en", timezone: "Asia/Tehran", interval: "60", withdateranges: true };
		return coin;
	}
</script>

{#await fillCoinData()}
{:then coin}

	<header>
		<div class="topdiv">
			<Searchbar />
		</div>
		<h1>{coin.name} Info</h1>
	</header>

	<div class="firstinfo">
		<div class="firstinfoimage">
			<img src={coin.image.large} width="100vw" height="100vw">
		</div>
		<p class="firstinfo_det"><strong>Name:</strong> <span style="font-size:0.8em;">{coin.name} <strong>({coin.symbol})</strong></span><br><strong>Rank:</strong> <span style="font-size:0.8em;">{coin.market_cap_rank}</span><br><strong>Price:</strong> <span style="font-size:0.8em;">{sigDigit(coin.market_data.current_price.usd)}$</span><br><strong>Market Cap:</strong> <span style="font-size:0.8em;">{sigDigit(coin.market_data.market_cap.usd)}$ </span><br><strong>Volume:</strong> <span style="font-size:0.8em;">{sigDigit(coin.market_data.total_volume.usd)}$ </span></p>
	</div>

	<div class="social">
		<div class="content"><a href="{coin.links.homepage[0]}" target="_blank" style="text-decoration:none; color: black;"><table><tr><td><img src="icons/home.svg" width="2vw" height="2vw"> </td><td>Website</td></tr></table></a></div>
		<div class="content"><a href="https://twitter.com/{coin.links.twitter_screen_name}" target="_blank" style="text-decoration:none; color: black;"><table><tr><td><img src="icons/twitter.svg" width="2vw" height="2vw"> </td><td>Twitter</td></tr></table></a></div>
		<div class="content"><a href="https://t.me/{coin.links.telegram_channel_identifier}" target="_blank" style="text-decoration:none; color: black;"><table><tr><td><img src="icons/telegram.svg" width="2vw" height="2vw"> </td><td>Telegram</td></tr></table></a></div>
		<div class="content"><a href="{coin.links.subreddit_url}" target="_blank" style="text-decoration:none; color: black;"><table><tr><td><img src="icons/reddit.svg" width="2vw" height="2vw"> </td><td>Reddit</td></tr></table></a></div>
		<div class="content"><a href="{coin.links.chat_url[1]}" target="_blank" style="text-decoration:none; color: black;"><table><tr><td><img src="icons/discord.svg" width="2vw" height="2vw"> </td><td>Discord</td></tr></table></a></div>
		<div class="content"><a href="{coin.links.blockchain_site[0]}" target="_blank" style="text-decoration:none; color: black;"><table><tr><td><strong style="color: #E65100;"> Explorer:</strong> {((coin.links.blockchain_site[0]).replace("https://", "")).split('/')[0]}</td></tr></table></a></div>
	</div>

	<div class="info">
		<div class="info_det">
			<table>
				<tr class="info_tr">
					<td class="td1"><strong>Market Cap:</strong></td>
					<td class="td2">{sigDigit(coin.market_data.market_cap.usd)}$</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>Total Volume:</strong></td>
					<td class="td2">{sigDigit(coin.market_data.total_volume.usd)}$</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>Fully Diluted Marrket Cap:</strong></td>
					<td class="td2">{sigDigit(coin.market_data.fully_diluted_valuation.usd)}$</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>Circulating Supply:</strong></td>
					<td class="td2">{sigDigit(coin.market_data.circulating_supply)}</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>Max Supply:</strong></td>
					<td class="td2">{sigDigit(coin.market_data.max_supply)}</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>Circulating/Max:</strong></td>
					<td class="td2">{(coin.market_data.circulating_supply/coin.market_data.max_supply).toFixed(2)}</td>
				</tr>
			</table>
		</div>
		<div class="info_det">
			<table>
				<tr class="info_tr">
					<td class="td1"><strong>All-Time High:</strong></td>
					<td class="td2">{sigDigit(coin.market_data.ath.usd)}$ &nbsp;&nbsp; <span class:tdpos={coin.market_data.ath_change_percentage.usd >= 0} class:tdneg={coin.market_data.ath_change_percentage.usd < 0}>{coin.market_data.ath_change_percentage.usd.toFixed(1)}%</span> </td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>All-Time High Date:</strong></td>
					<td class="td2">{time2}</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>24h High:</strong></td>
					<td class="td2">{sigDigit(coin.market_data.high_24h.usd)}$</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>24h Low:</strong></td>
					<td class="td2">{sigDigit(coin.market_data.low_24h.usd)}$</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong></strong></td>
					<td class="td2">&nbsp;</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong></strong></td>
					<td class="td2">&nbsp;</td>
				</tr>
			</table>
		</div>
		<div class="info_det">
			<table>
				<tr class="info_tr">
					<td class="td1"><strong>1h Change:</strong></td>
					<td class="td2" class:tdpos={coin.market_data.price_change_percentage_1h_in_currency.usd >= 0} class:tdneg={coin.market_data.price_change_percentage_1h_in_currency.usd < 0}>{coin.market_data.price_change_percentage_1h_in_currency.usd.toFixed(1)}%</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>24h Change:</strong></td>
					<td class="td2" class:tdpos={coin.market_data.price_change_percentage_24h >= 0} class:tdneg={coin.market_data.price_change_percentage_24h < 0}>{coin.market_data.price_change_percentage_24h.toFixed(1)}%</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>7d Change:</strong></td>
					<td class="td2" class:tdpos={coin.market_data.price_change_percentage_7d >= 0} class:tdneg={coin.market_data.price_change_percentage_7d < 0}>{coin.market_data.price_change_percentage_7d.toFixed(1)}%</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>30d Change:</strong></td>
					<td class="td2" class:tdpos={coin.market_data.price_change_percentage_30d >= 0} class:tdneg={coin.market_data.price_change_percentage_30d < 0}>{coin.market_data.price_change_percentage_30d.toFixed(1)}%</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong>1y Change:</strong></td>
					<td class="td2" class:tdpos={coin.market_data.price_change_percentage_1y >= 0} class:tdneg={coin.market_data.price_change_percentage_1y < 0}>{coin.market_data.price_change_percentage_1y.toFixed(1)}%</td>
				</tr>
				<tr class="info_tr">
					<td class="td1"><strong></strong></td>
					<td class="td2">&nbsp;</td>
				</tr>
			</table>
		</div>
	</div>

<div class="divtradingview">
	<TradingViewWidget {options} />
</div>

{/await}

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

	.firstinfo{
		margin: 1em;
		display: inline-flex;
	}

	.firstinfoimage{
		display: block;
		vertical-align: bottom;
		width: 6em;
		height: 6em;
		margin: auto;
	}

	img {
		width: 100%;
		height: 100%;
	}

	.firstinfo_det{
		vertical-align: middle;
		#position: absolute;
		#top: 50%;
		margin: auto;
		margin-left: 0.2em;
		line-height: 1.2em;
	}

	.social{
		margin-left: 5%;
		margin-right: 5%;
		text-align: center;
	}

	.content{
		margin-top: 2em;
		display: inline-block;
		border: 0.2em solid #202020;
		border-radius: 0.7em;
		background-color: #f5f5f5;
		min-width: 8em;
		padding-left: 0.5em;
		padding-right: 0.5em;
		margin-right: 0.7em;
		margin-left: 0.7em;
		box-shadow:
		-0.2em 0.2em 0.3em 0.1em #E65100;
		text-align: center;
	}

	.content table{
		margin: 0 auto;
		height: 2.5em;
	}

	.content img{
		height: 2em;
	}

	.info{
		font-size: 0.8em;
		text-align: center;
		margin-top: 2em;
		margin-bottom: 2em;
	}
	.info_det{
		background-color: #f5f5f5;
		border: 0.05em solid #202020;
		border-radius: 0.6em;
		display: inline-flex;
		width: 25%;
		margin-left: 1%;
		margin-right: 1%;
		margin-bottom: 2em;
		min-width: 20em;
		text-align: center;
	}

	.info_det table{
		min-height: 15em;
		width: 100%;
		border-collapse: collapse;
		margin-right: 0.2em;
		margin-left: 0.2em;
	}

	.info_tr{
		margin-bottom: 0.2em;
		box-shadow:
		0 0.3em 0.2em -0.2em #E65100;
	}

	.td1{
		text-align: left;
	}

	.td2{
		text-align: right;
	}

	.tdpos{
		color: #37fd12;
	}

	.tdneg{
		color: #fc0003;
	}

	.divtradingview{
		margin-left: 3%;
		margin-right: 3%;
		height: 37em;
		padding-bottom: 2em;
	}

</style>