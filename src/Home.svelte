<script>
	import Header from './components/Header.svelte';
	import Body from './components/Body.svelte';
	import Footer from './components/Footer.svelte';
	import { coinStore, pageCoins } from './Stores/coinStore.js';

	let items = ['Home', 'Portfolio'];
	let activeItem = 'Home';
	let pageCoinNo = 50;
	let pageNo = 1;
	let coinperpageshow = false;
	let activePage = 1;
	let finalPage = 240;
	let sortItem;
	let descendingorder = true;

	async function getPageCoins(e) {
		if (e.detail != undefined) {
			pageCoinNo = await e.detail.pageCoinNo;
			pageNo = await e.detail.activePage;
			activePage = pageNo;
			finalPage = await (12000/e.detail.pageCoinNo);
			if (pageNo > finalPage) {
				finalPage = pageNo;
			}
		}
		const res = await fetch(`https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=${pageCoinNo}&page=${pageNo}&sparkline=false&price_change_percentage=1h%2C24h%2C7d`);
		$pageCoins = await res.json();
	}

	const tabChange = (e) => {
		activeItem = e.detail;
		sortItem = 'market_cap_rank';
		coinperpageshow = !coinperpageshow;
		if (activeItem == "Home") {
			coinperpageshow = false;
		}
		if (activeItem == "Portfolio") {
			coinperpageshow = true;
		}
	}

	const sortTable = (e) => {
		sortItem = e.detail.sortItem;
		descendingorder = e.detail.descendingorder;
		$pageCoins.sort((a, b) => {
			if (descendingorder) {
				return (a[sortItem] - b[sortItem]);
			}
			else {
				return (b[sortItem] - a[sortItem]);
			}
		});
		$pageCoins = [ ...$pageCoins]
	}
</script>

<Header {activePage} {coinperpageshow} {pageCoinNo} {pageNo} {activeItem} {items} on:tabChange = {tabChange} on:getPageCoins = {getPageCoins} />

<Body {descendingorder} {sortItem} {finalPage} {activePage} {getPageCoins} {pageCoinNo} {pageNo} {activeItem} on:sortTable = {sortTable} on:getPageCoins = {getPageCoins} />

<Footer />

<style>

</style>