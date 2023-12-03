<template>
	<div class="content-container">
		<div class="left-area">
			<div>
				<h1>Acervo</h1>
				<TextArea :title-guess="this.titleGuess" :word-guess="this.wordGuess" @guess-callback="setGuesses"/>
			</div>
			<InputArea @word-guess="setWordGuess" @title-guess="setTitleGuess"/>
		</div>
		<div class="right-area">
			<GuessHistory :guesses="this.guesses"/>
		</div>
	</div>
</template>

<script>
import TextArea from './components/TextArea.vue';
import InputArea from './components/InputArea.vue';
import GuessHistory from './components/GuessHistory.vue';

export default {
	name: 'App',
	components: {
    TextArea,
    InputArea,
    GuessHistory
},
	data() {
		return {
			titleGuess: '',
			wordGuess: '',
			guesses: []
		}
	},
	methods: {
		setWordGuess(wordGuess) {
			this.wordGuess = wordGuess;
		},

		setTitleGuess(titleGuess) {
			this.titleGuess = titleGuess;
		},

		setGuesses(guess) {
			this.guesses.push(guess);
		} 
	}
}
</script>

<style lang="scss">
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background-color: rgba(0, 0, 0, 0.1); 
}

::-webkit-scrollbar-thumb {
  background-color: #282a35;
  border-radius: 5px;
}

/* Hide scrollbar buttons (arrows) */
::-webkit-scrollbar-button {
  display: none;
}

body {
	margin: 0;
	background-color: #0d1117;
	height: 100vh;
}
#app {
	font-family: monospace, Helvetica, Arial, sans-serif;
	text-align: center;
	color: #2c3e50;
	height: 100%;
}
.content-container {
	display: grid;
    grid-template-columns: 82% 18%;
	height: 100%;

	.left-area {
		display: flex;
		flex-direction: column;
		justify-content: space-between;

		h1 {
			color: #e6edf3;
		}
	}
}
</style>
