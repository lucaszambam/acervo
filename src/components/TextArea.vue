<template>
	<div class="text-area">
        <div class="title-container">
            <div class="word-container" v-for="word in this.article.title.split(' ')">
                <span class="word-content hidden" :data-hidden-word="encryptWord(word)" :style="{width: getWordWidth(word, 22) + 'px'}" :data-word-length="word.length" @click="displayWordLength"></span>
                <span class="blank-space" :style="{width: getWordWidth(' ') + 'px'}"></span>
            </div>
        </div>

        <div class="content-container">
            <div class="word-container" v-for="word in this.article.words">
                <span class="word-content" v-if="this.visibleWords.includes(word.toLowerCase())">{{ word }}</span>
                <span class="word-content hidden" :data-hidden-word="encryptWord(word)" :style="{width: getWordWidth(word) + 'px'}" :data-word-length="word.length" @click="displayWordLength" v-else></span>
                <span class="blank-space" :style="{width: getWordWidth(' ') + 'px'}"></span>
            </div>
        </div>
	</div>
</template>

<script>
export default {
	name: 'TextArea',
	props: {
        titleGuess: String,
		wordGuess: String
	},
    emits: ['guessCallback'],
    watch: { 
        titleGuess: function (guessInput) {
            this.guessTitle(guessInput); 
        },

        wordGuess: function (guessInput) {
            this.guessWord(guessInput);
        }
    },
    methods: {
        displayWordLength(event) {
            event.currentTarget.innerHTML = event.currentTarget.dataset.wordLength;
        },

        encryptWord(word) {
            return btoa(word.toLowerCase());
        },

        getWordWidth(word, fontSize = 16, fontFamily = 'monospace') {
            const context = document.createElement('canvas').getContext('2d');
            context.font = `${fontSize}px ${fontFamily}`;

            return context.measureText(word).width;
        },

        guessWord(guess) {
            this.showHiddenWord(guess);
        },

        guessTitle(guess) {
            this.showHiddenWord(guess);
        },

        showHiddenWord(word) {
            const foundWords = document.querySelectorAll('.left-area .text-area .content-container .word-content.hidden[data-hidden-word="ZWNvc3Npc3RlbWFz"]');
            this.$emit('guessCallback', {
                word: word,
                occurrences: foundWords.length
            });
            foundWords.forEach(function(wordNode) {
                wordNode.classList.remove('hidden');
                wordNode.removeAttribute('data-hidden-word');
                wordNode.removeAttribute('data-word-length');
                
                wordNode.innerHTML = this.article.words.find((articleWord) => {
                    if (articleWord.toLowerCase() === word.toLowerCase()) {
                        return articleWord;
                    }
                });

                wordNode.parentNode.replaceChild(wordNode.cloneNode(true), wordNode);
            }, this);
        }
    },
    data() {
        return {
            article: {
                title: '',
                words: []
            },
            visibleWords: []
        }
    },
    beforeCreate() {
        import('@/assets/articles.json')
            .then((response) => {
                const currentArticle = response.default[Math.floor(Math.random() * response.default.length)];

                this.article.title = currentArticle.title;
                this.article.words = currentArticle.content.split(' ');
            })
            .catch((error) => {
                console.error('Error fetching articles:', error);
            });

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
$size-title: 22px;
$size-content: 16px;
.text-area {
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 10px;
    padding: 20px;
    .title-container {
        font-size: $size-title;
        height: $size-title;

        .hidden {
            height: $size-title !important;
        }
    }
    .content-container, .title-container {
        display: flex;
        flex-wrap: wrap;
        max-height: 130px;
        overflow: hidden;

        .word-container {
            display: flex;
            .word-content, .blank-space {
                display: block;
                color: #e6edf3;
                &.hidden {
                    background-color: #282a35;
                    border-radius: .2rem;
                    display: flex;
                    margin: 0.1rem 0;
                    height: $size-content;
                }
            }
        }
    }
}
</style>
