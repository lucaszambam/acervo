<template>
	<div>
        <div class="title">
            {{ this.article.title }}
        </div>

        <div class="content">
            {{ this.article.content }}
        </div>
	</div>
</template>

<script>
export default {
	name: 'TextArea',
	props: {
		word: String
	},
    data() {
        return {
            article: {}
        }
    },
    created() {
        import('@/assets/articles.json')
            .then((response) => {
                this.article = response.default[Math.floor(Math.random() * response.default.length)];
            })
            .catch((error) => {
                console.error('Error fetching articles:', error);
            });
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
	margin: 40px 0 0;
}

ul {
	list-style-type: none;
	padding: 0;
}

li {
	display: inline-block;
	margin: 0 10px;
}

a {
	color: #42b983;
}</style>
