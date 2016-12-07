<style lang="stylus">

	$default-color = #000;

	.ch-dkfbasel-search {

		width: 100%;
		position: relative;

		.search-box {
			width: 100%;
			height: 50px;
			padding-left: 54px;
			font-size: 18px;
			border: 1px solid $default-color;
		}

		.magnifying-glass {

			path, circle {
				stroke: $default-color;
				stroke-width: 1px;
				fill: none;
			}
		}

	}

</style>

<template>
	<div class="ch-dkfbasel-search">
		<input class="search-box" type="text" v-model="search_term"
			@input="doInput" @keyup.enter="doSearch" ref="searchbox"></input>

		<svg class="magnifying-glass" width="26px" height="26px" viewBox="-1 -1 26 26"
			version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
			 <circle id="Oval" cx="10" cy="10" r="10"></circle>
			 <path d="M17.5,17.5 L24,24" id="Line" stroke-linecap="square"></path>
		</svg>

		<transition tag="div" name="fade" mode="out-in">

			<div class="hint" v-if="hint_type == 'typing'" key="typing">
				<slot name="hint_typing"></slot>
			</div>

			<div class="hint" v-if="hint_type == 'searching'" key="searching">
				<slot name="hint_searching"></slot>
			</div>

			<div class="hint" v-if="hint_type == 'result'" key="result">
				<slot name="hint_result"><slot>
			</div>

			<slot name="results"></slot>

		</transition>
	</div>
</template>

<script lang="babel">

	module.exports = {

		props: ['hint_search', 'hint_typing', 'hint_result'],

		data() {
			return {
				search_term: ''
			};
		},

		mounted() {
			this.$refs.searchbox.focus();
		},

		methods: {

			doSearch() {

			},

			doInput() {

			}
		}

	}

</script>
