<template>
	<h1>Acervo</h1>
	<TextArea />
	<InputArea />
</template>

<script>
import TextArea from './components/TextArea.vue';
import InputArea from './components/InputArea.vue';

export default {
	name: 'App',
	components: {
		TextArea,
		InputArea
	}
}
</script>

<style lang="scss">
body {
	margin: 0;
	background-color: #0d1117;
}
#app {
	font-family: monospace, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
</style>
