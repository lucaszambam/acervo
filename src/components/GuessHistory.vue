<template>
	<div class="guess-history">
		<span class="header" v-html="getHeaderContent()"></span>
		<div class="guesses">
			<div v-for="guess in this.guesses" class="guess-container" :class="getGuessContainerClass(guess.word, guess.occurrences)">
				<div title="Palavra">{{ guess.word }}</div>
				<div title="Ocorrências">{{ guess.occurrences }}</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'GuessHistory',
	props: {
		guesses: Array
	},
	data() {
		return {
			visibleWords: ''
		}
	},
	methods: {
		getGuessContainerClass(word, occurrences) {
			if (this.visibleWords.includes(word)) {
				return 'not-valid';
			}

			if (occurrences > 0) {
				return 'incident';
			}

			return 'not-incident';
		},

		getHeaderContent() {
			this.scrollDownContainer();

			const guessesLength = this.guesses.length;
			if (guessesLength === 0) {
				return 'palpites';
			} else if (guessesLength === 1) {
				return guessesLength + ' palpite';
			} else {
				return guessesLength + ' palpites';
			}
		},

		scrollDownContainer() {
			const guessesContainer = document.querySelector('.guesses');
            if (guessesContainer) {
                guessesContainer.scrollTo({
                    top: guessesContainer.scrollHeight,
                    behavior: 'smooth'
                });
            }
		}
	},
	beforeCreate() {
		import('@/assets/visible-words.json')
            .then((response) => {
                this.visibleWords = response.default;
            })
            .catch((error) => {
                console.error('Error fetching visible words:', error);
            });
	}
}
</script>

<style scoped lang="scss">
$incident: #2ecc71;
$not-incident: #e74c3c;
$not-valid: #f7dc6f;

.guess-history {
	background-color: #161b22;
    width: 100%;
    height: 100%;
	.header {
		background-color: #010409;
		display: flex;
		height: 30px;
		align-items: center;
		padding: 10px;
		border-bottom: 2px solid #21262d;
		color: #e6edf3;
	}
	.guesses {
		max-height: calc(100vh - 51.6px);
    	overflow: auto;
		.guess-container {
			display: flex;
			padding: 15px;
			justify-content: space-between;
			transition: all .2s;
			cursor: pointer;
			&:hover {
				background-color: rgba(52, 152, 219, 0.1);
			}
			&.incident {
				color: $incident;
			}
			&.not-incident {
				color: $not-incident;
			}
			&.not-valid {
				color: $not-valid;
			}
		}
	}
}
</style>
