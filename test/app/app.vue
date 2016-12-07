<style lang="stylus">

	.wrapper {
		width: 600px;
		margin: 40px auto;
	}

	.start-info {
		margin-top: 40px;
	}

</style>

<template>

	<div class="wrapper">

		<search autofocus :query="startSearch">

			<template slot="start">
				<div class="start-info">This is starting text</div>
			</template>

			<template slot="result" scope="results">
				<li class="single-result">My custom: {{results.title}}</li>
			</template>

		</search>

	</div>

</template>

<script lang="babel">

	import Search from './search.vue';

	module.exports = {

		name: 'App',

		components: {
			search: Search
		},

		data() {
			return {
				props: []
			};
		},

		methods: {

			startSearch() {

				let result = {
					books: [
						{ title: 'Cook book', description: 'Yammy' },
						{ title: 'Book of tales', description: 'Woah' },
						{ title: 'Everything for dummies', description: 'Ahhh!' }
					]
				};

				return new Promise((resolve) => setTimeout(() => {
					return resolve(result.books)
				}, 500));
			}

		}

	};

</script>
