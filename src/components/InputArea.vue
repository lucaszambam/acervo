<template>
	<div class="input-area">
		<div class="input-container input-title-container">
			<input id="guessTitle" name="guessTitle" type="text" required placeholder="Digite o título do artigo" @keyup.enter="submitGuess($event)" data-input-type="title">
            <button class="submit-container" data-input-type="title" @click="onClickSubmitGuess"><img height="25" width="25" src="@/assets/send.svg" alt="Send"></button>
		</div> 
		<div class="input-container input-word-container">
			<input id="guessWord" name="guessWord" type="text" required placeholder="Palpite uma palavra" @keyup.enter="submitGuess($event)" data-input-type="word">
            <button class="submit-container" data-input-type="word" @click="onClickSubmitGuess"><img height="25" width="25" src="@/assets/send.svg" alt="Send"></button>
		</div>
	</div>
</template>

<script>
export default {
	name: 'InputArea',
    emits: ['wordGuess', 'titleGuess'],
    methods: {
        submitGuess(event) {
            this.$emit(`${event.target.dataset.inputType}Guess`, event.target.value);
            event.target.value = '';
        },

        onClickSubmitGuess(event) {
            const inputElement = document.querySelector(`input[data-input-type="${event.currentTarget.dataset.inputType}"]`);
            if (inputElement) {
                inputElement.dispatchEvent(new KeyboardEvent('keyup', {
                    key: 'Enter',
                    code: 'Enter',
                    keyCode: 13,
                    which: 13,
                    bubbles: true
                }));
            }
        }
    }
}
</script>

<style scoped lang="scss">
.input-area {
	background-color: #010409;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 75px;
    width: -webkit-fill-available;
	gap: 50px;
    border-top: 2px solid #21262d;

    .input-container {
        display: flex;
        justify-content: center;
        align-items: center;
        &.input-title-container {
            min-width: 270px;
            position: fixed;
            left: 10px;
        }

        .submit-container {
            display: flex;
            background: none;
            border: 0;
            cursor: pointer;
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
        }
    }
}
</style>