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
                <span class="word-content" v-if="this.visibleWords.includes(word)">{{ word }}</span>
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
		articleGuess: String
	},
    data() {
        return {
            article: {
                title: '',
                words: {
                    id: 0,
                    content: ''
                }
            },
            visibleWords: [
                // artigos
                'o', 'a', 'os', 'as', 'um', 'uma', 'uns', 'umas',

                // conectivos
                'e', 'ou', 'mas', 'porque', 'como', 'quando', 'se', 
                'além', 'apesar', 'até', 'depois', 'desde', 'quando',
                'onde', 'pois', 'logo', 'enquanto', 'para', 'por', 
                'caso', 'embora', 'ainda', 'assim', 'senão', 'porquanto',
                'contudo', 'entretanto', 'porém', 'mesmo', 'talvez', 'também',
                'todavia', 'logo', 'isto', 'isso', 'aquilo', 'eis', 'quer', 
                'seja', 'ora', 'nem', 'portanto', 'quanto', 'tão', 
                'tanto', 'bastante', 'quais', 'quer', 'certo', 'realmente',
                'ainda', 'mesmo', 'bem', 'sempre', 'visto', 'uma'
            ]
        }
    },
    methods: {
        displayWordLength(event) {
            event.currentTarget.innerHTML = event.currentTarget.dataset.wordLength;
        },

        encryptWord(word) {
            return btoa(word);
        },

        getWordWidth(word, fontSize = 16, fontFamily = 'monospace') {
            const context = document.createElement('canvas').getContext('2d');
            context.font = `${fontSize}px ${fontFamily}`;

            return context.measureText(word).width;
        }
    },
    created() {
        import('@/assets/articles.json')
            .then((response) => {
                const currentArticle = response.default[Math.floor(Math.random() * response.default.length)];

                this.article.title = currentArticle.title;
                this.article.words = currentArticle.content.split(' ');
            })
            .catch((error) => {
                console.error('Error fetching articles:', error);
            });
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$size-title: 22px;
$size-content: 16px;
.text-area {
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 10px;
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
