<script>
	import { createEventDispatcher } from 'svelte';
	import T_main_h from './T_main_h.svelte';
	import T_main_coins from './T_main_coins.svelte';
	import { pageCoins } from '../Stores/coinStore.js';
	// import Tabs from '../Shared/Tabs.svelte'

	// //Tabs:
	// let items = ['Home', 'Portfolio'];
	// let activeItem = 'Home';
	// const tabChange = (e) => activeItem = e.detail;
	export let descendingorder;
	const dispatch = createEventDispatcher();
	export let activeItem;
	export let pageCoinNo;
	export let pageNo;
	export let getPageCoins;
	export let activePage;
	export let finalPage;
	export let sortItem;
	
</script>

<body>
	{#if activeItem === 'Home'}
		<T_main_h {descendingorder} {sortItem} on:sortTable />
		<div class="divmaincoins">
			<T_main_coins {pageCoinNo} {pageNo} {getPageCoins} />
		</div>
		<!-- Page number____________________________________________________________________________ -->
		<div class="divperpage">
			<form class="formperpage" on:submit|preventDefault = {() => dispatch('getPageCoins', {pageCoinNo, activePage})}>
				<label class="labelperpage">Page</label>
				<input class="inputperpage" type="number" name="CoinsPerPage" bind:value = {activePage} >
				<input class="submitperpage" type="submit" value=" &#10551 ">
				<input class:hidefirstpagetag={pageNo === 1} class="numberperpage" type="button" name="first" value={pageNo-1} on:click|preventDefault = {() => {
					activePage--;
					dispatch('getPageCoins', {pageCoinNo, activePage});
				} }>
				<input class="activenumber" type="button" name="second" value={pageNo}>
				<input class:hidefirstpagetag={pageNo >= 240} class="numberperpage" type="button" name="third" value={pageNo+1} on:click|preventDefault = {() => {
					activePage++;
					dispatch('getPageCoins', {pageCoinNo, activePage});
				} }>
				<div class:hidefirstpagetag={pageNo >= finalPage-2} class="threepoint" >...</div>
				<input class:hidefirstpagetag={pageNo >= finalPage-1} class="numberperpage" type="button" name="last" value={finalPage} on:click|preventDefault = {() => {
					activePage = finalPage;
					dispatch('getPageCoins', {pageCoinNo, activePage});
				} }>
			</form>
		</div>
		<!-- Page number____________________________________________________________________________ -->
	{:else if activeItem === 'Portfolio'}
		<p style="text-align: center" >I am blackboard</p>
	{/if}
</body>

<style>
	body{
		margin-top: 1em;
		background: white;
		height: auto;
		min-height: 26.41em;
	}

	div.divmaincoins{
		min-height: 300px;
	}

	div.divperpage{
		margin-top:  0.7em;
		flex-direction: row, column;
		width: 100%;
		text-align: center;
	}

	.formperpage{
		margin-top: 0.6em;
		margin-bottom: 0.6em;
		display: inline-block;
		padding-top: 0.4em;
		width: 50%;
		height: auto;
		flex-direction: row, column;
		border-radius: 0.4em;
	}

	.labelperpage{
		font-size: 0.8em;
		display: inline-block;
		flex-grow: 1;
		color: #202020;
		font-weight: bold;
	}

	.inputperpage{
		height: 1.5em;
		width: 4em;
		font-family: times;
		font-weight: bold;
		color: black;
		background-color: white;
		border-radius: 0.4em;
	}

	.submitperpage{
		color: #202020;
		background-color: gray;
		font-size: 1.2em;
		padding: 0;
		padding-bottom: 0.1em;
		font-weight: bold;
		border-radius: 0.4em;
		border-color: lightgray;
		cursor: pointer;
	}

	.hidefirstpagetag{
		display: none;
		visibility: hidden;
	}

	.numberperpage{
		color: #202020;
		background-color: gray;
		font-size: 0.8em;
		font-weight: bold;
		border-radius: 0.4em;
		border-color: lightgray;
		cursor: pointer;
	}

	.threepoint{
		display: inline-block;
	}

	.activenumber{
		color: #202020;
		background-color: #E65100;
		font-size: 0.8em;
		font-weight: bold;
		border-radius: 0.4em;
		border-color: #E65100;
		cursor: pointer;
	}

</style>