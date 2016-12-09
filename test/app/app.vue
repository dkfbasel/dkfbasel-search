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

		<search autofocus :query="startSearch" v-model="search_term">

			<template slot="start">
				<div class="start-info">This is starting text</div>
			</template>

			<template slot="hint_result" scope="props">
				Your search for
				<span quotation>{{props.term}}</span>
				has returned {{props.amount}} results
			</template>

			<template slot="result" scope="props">
				<li class="single-result" :key="props.index"
				:data-index="props.index">{{props.result.title}}</li>
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
				props: [],
				search_term: ''
			};
		},

		methods: {

			startSearch() {

				let books = [
					{ title: 'Cook book', description: 'Yammy' },
					{ title: 'Book of tales', description: 'Woah' },
					{ title: 'Everything for dummies', description: 'Ahhh!' }
				]

				let result = books.filter((book) => {
					return book.title.indexOf(this.search_term) > -1;
				});

				return new Promise((resolve) => setTimeout(() => {
					return resolve(result)
				}, 500));
			}

		}

	};

</script>
