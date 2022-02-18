<script>
	import Router from 'svelte-spa-router';
	import Home from './Home.svelte';
	import CoinPage from './Pages/coinPage.svelte';
</script>

<Router routes={{
	'/': Home,
	'/coins/:coinid': CoinPage
}} />