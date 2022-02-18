<script>
	import { createEventDispatcher } from 'svelte';

	let theader = ['#', 'Name', 'Price', '1h', '24h', '7d', 'Market Cap', 'Total Vol'];
	const dispatch = createEventDispatcher();
	export let descendingorder;
	export let sortItem = 'market_cap_rank';
</script>

<div class="main">
	<!-- __________________________________________________________________________________________________________________ -->
	<div class:activeonec={sortItem === 'market_cap_rank'} class:onec={sortItem != 'market_cap_rank'} on:click={() => {
		descendingorder = !descendingorder;
		sortItem = 'market_cap_rank';
		dispatch('sortTable', {sortItem, descendingorder});
	}} >{theader[0]}{#if sortItem === 'market_cap_rank' }{@html descendingorder? '&#x25b4;' : '&#x25be;'}{/if}</div>
	<!-- __________________________________________________________________________________________________________________ -->
	<div class="Table"> &nbsp;&nbsp;&nbsp;&nbsp; {theader[1]}</div>
	<!-- __________________________________________________________________________________________________________________ -->
	<div class:activeafteronec={sortItem === 'current_price'} class:afteronec={sortItem != 'current_price'} on:click={() => {
		descendingorder = !descendingorder;
		sortItem = 'current_price';
		dispatch('sortTable', {sortItem, descendingorder});
	}} >{theader[2]}{#if sortItem === 'current_price' }{@html descendingorder? '&#x25b4;' : '&#x25be;'}{/if}</div>
	<!-- __________________________________________________________________________________________________________________ -->
	<div class="percentage">
		<!-- __________________________________________________________________________________________________________________ -->
		<div class:activepercentcell={sortItem === 'price_change_percentage_1h_in_currency'} class:percentcell={sortItem != 'price_change_percentage_1h_in_currency'} on:click={() => {
		descendingorder = !descendingorder;
		sortItem = 'price_change_percentage_1h_in_currency';
		dispatch('sortTable', {sortItem, descendingorder});
		}} >{theader[3]}{#if sortItem === 'price_change_percentage_1h_in_currency' }{@html descendingorder? '&#x25b4;' : '&#x25be;'}{/if}</div>
		<!-- __________________________________________________________________________________________________________________ -->
		<div class:activepercentcell={sortItem === 'price_change_percentage_24h'} class:percentcell={sortItem != 'price_change_percentage_24h'} on:click={() => {
		descendingorder = !descendingorder;
		sortItem = 'price_change_percentage_24h';
		dispatch('sortTable', {sortItem, descendingorder});
		}} >{theader[4]}{#if sortItem === 'price_change_percentage_24h' }{@html descendingorder? '&#x25b4;' : '&#x25be;'}{/if}</div>
		<!-- __________________________________________________________________________________________________________________ -->
		<div class:activepercentcell={sortItem === 'price_change_percentage_7d_in_currency'} class:percentcell={sortItem != 'price_change_percentage_7d_in_currency'} on:click={() => {
		descendingorder = !descendingorder;
		sortItem = 'price_change_percentage_7d_in_currency';
		dispatch('sortTable', {sortItem, descendingorder});
		}} >{theader[5]}{#if sortItem === 'price_change_percentage_7d_in_currency' }{@html descendingorder? '&#x25b4;' : '&#x25be;'}{/if}</div>
		<!-- __________________________________________________________________________________________________________________ -->
	</div>
	<!-- __________________________________________________________________________________________________________________ -->
	<div class:activeafteronec={sortItem === 'market_cap'} class:afteronec={sortItem != 'market_cap'} on:click={() => {
		descendingorder = !descendingorder;
		sortItem = 'market_cap';
		dispatch('sortTable', {sortItem, descendingorder});
	}} >{theader[6]}{#if sortItem === 'market_cap' }{@html descendingorder? '&#x25b4;' : '&#x25be;'}{/if}</div>
	<!-- __________________________________________________________________________________________________________________ -->
	<div class:activeafteronec={sortItem === 'total_volume'} class:afteronec={sortItem != 'total_volume'} on:click={() => {
		descendingorder = !descendingorder;
		sortItem = 'total_volume';
		dispatch('sortTable', {sortItem, descendingorder});
	}} >{theader[7]}{#if sortItem === 'total_volume' }{@html descendingorder? '&#x25b4;' : '&#x25be;'}{/if}</div>
	<!-- __________________________________________________________________________________________________________________ -->
</div>

<style>
	div.main{
		min-height: 2em;
		line-height: 2em;
		font-size: 0.8em;
		display: flex;
		flex-direction: row, column;
		font-weight: bold;
		overflow: hidden;
		border-radius: 1.5em;
		color: black;
		background-color: #d3d3d3;
		padding: 0.2em;
		text-align: center;
		margin-left: 0.8em;
		margin-right: 0.8em;
	}

	/*Div onec for adding padding to the left only for 1st column*/
	div.onec{
		padding-left: 2em;
		text-align: left;
		display: inline;
		flex-grow: 1;
		width: 3%;
		margin-top: 0.2em;
		cursor: pointer;
	}

	div.activeonec{
		color: #E65100;
		padding-left: 2em;
		text-align: left;
		display: inline;
		flex-grow: 1;
		width: 3%;
		margin-top: 0.2em;
		cursor: pointer;
	}

	div.afteronec{
		text-align: left;
		display: inline;
		flex-grow: 1;
		width: 11.5%;
		margin-top: 0.2em;
		cursor: pointer;
	}

	div.activeafteronec{
		color: #E65100;
		text-align: left;
		display: inline;
		flex-grow: 1;
		width: 11.5%;
		margin-top: 0.2em;
		cursor: pointer;
	}

	div.Table{
		text-align: left;
		display: inline;
		flex-grow: 1;
		width: 11.5%;
		min-width: 90px;
		margin-top: 0.2em;
	}

	div.percentage{
		width: 20%;
		text-align: center;
		display: flex;
		flex-direction: row, column;
		padding-right: 7em;
	}

	div.percentcell {
		text-align: center;
		display: inline;
		flex-grow: 1;
		width: 6%;
		min-width: 55px;
		border-radius: 1.5em;
		margin: 0.2em;
		cursor: pointer;
	}

	div.activepercentcell {
		color: #E65100;
		text-align: center;
		display: inline;
		flex-grow: 1;
		width: 6%;
		min-width: 55px;
		border-radius: 1.5em;
		margin: 0.2em;
		cursor: pointer;
	}

</style>