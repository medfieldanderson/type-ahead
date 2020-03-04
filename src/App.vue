<template>
	<div id="app">
		<provider-type-ahead name="servicing" label="Servicing Provider" :intervalInMS="700" :minLength="1"/>
	</div>
</template>

<script>
	import ProviderTypeAhead from './components/ProviderTypeAhead.vue'

	export default {
		name: 'App',
		components: { ProviderTypeAhead },
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
