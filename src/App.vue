<template>
	<div id="app">
		<provider-type-ahead name="servicing" label="Servicing Provider" :intervalInMS="700" :minLength="1" :showSummary="true"/>
		<tx-provider-type-ahead name="tx-prov" label="Texas Provider" :intervalInMS="400" :minLength="1" :showSummary="true"/>
	</div>
</template>

<script>
	import ProviderTypeAhead from './components/ProviderTypeAhead.vue'
	import TxProviderTypeAhead from './components/TxProviderTypeAhead.vue'

	export default {
		name: 'App',
		components: { TxProviderTypeAhead, ProviderTypeAhead },
	}
</script>

<style>
	#app {
		font-family: Verdana, Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: left;
		color: #2c3e50;
		margin-top: 10px;
	}
</style>
