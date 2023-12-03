<template>
	<div class="input-area">
		<div class="input-title-container" style="width: 15%;">
			<input id="guessTitle" name="guessTitle" type="text" placeholder="Digite o título do artigo">
		</div>
		<div class="input-word-container">
			<input id="guessWord" name="guessWord" type="text" placeholder="Palpite uma palavra">
		</div>
	</div>
</template>

<script>
export default {
	name: 'InputArea'
}
</script>

<style scoped lang="scss">
.input-area {
	background-color: #010409;
    position: absolute;
    bottom: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 75px;
    width: -webkit-fill-available;
	gap: 50px;
}

input {
	width: 100%;
    height: 20px;
    padding: 5px;
    background-color: #21262d;
    color: #fff;
    border: 1px solid #7d8590;
    border-radius: 0.4rem;
    font-weight: 700;
    font-family: monospace;
    font-size: 1rem;
    -moz-text-align-last: center;
    text-align-last: center;
}
</style>