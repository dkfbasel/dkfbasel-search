<style lang="stylus">

	$default-color = #000;

	.ch-dkfbasel-search-fade-enter-active, .ch-dkfbasel-search-fade-leave-active {
		transition: opacity 1s ease;
		opacity: 1;
	}

	.ch-dkfbasel-search-fade-enter {
		opacity: 0;
	}

	.ch-dkfbasel-search-fade-leave, .ch-dkfbasel-search-fade-leave-active {
		transition: none;
		opacity: 0;
	}

	.ch-dkfbasel-search {

		box-sizing: border-box;

		*, *:after, *:before {
			box-sizing: border-box;
		}

		width: 100%;
		position: relative;

		[quotation] {
			&::before, &::after {
				content: '"'
			}
		}

		.ch-dkfbasel-search-box {

			width: 100%;
			position: relative;
			border: 1px solid $default-color;

			input {
				width: 100%;
				height: 50px;
				padding-left: 54px;
				font-size: 18px;
				outline: none;
				border: none;
			}

			.ch-dkfbasel-search-icon {

				position: absolute;
				top: 17.5px;
				left: 18.5px;

				path, circle {
					stroke: $default-color;
					stroke-width: 1.25px;
					fill: none;
				}
			}
		}

		.ch-dkfbasel-search-hint {
			padding-top: 20px;
			font-size: 14px;

			svg {
				margin-right: 8px;
				margin-left: 8px;
			}
		}

		.ch-dkfbasel-search-result-item {
			display: block;
		}

	}

</style>

<template>
	<div class="ch-dkfbasel-search">

		<div class="ch-dkfbasel-search-box">

			<input type="text" v-model="search_term" @blur="onBlur"
				@input="onInput" @keyup.enter="onSearch" ref="searchbox"></input>

			<svg class="ch-dkfbasel-search-icon" width="18px" height="18px" viewBox="0 0 18 18" version="1.1"
				xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
				<circle cx="7.5" cy="7.5" r="6.5"></circle>
				<path d="M12.5,12.5 L16,16" stroke-linecap="round"></path>
			</svg>

		</div>

		<div class="ch-dkfbasel-search-hint">

			<transition tag="div" name="ch-dkfbasel-search-fade" mode="out-in">

				<div v-if="hint == 'typing'" key="typing">

					<svg xmlns="http://www.w3.org/2000/svg" width="5" height="10">
						<path fill="none" stroke="#000" stroke-width="1" d="M0 0 L5 5 L0 10"></path>
					</svg>

					<slot name="hint_typing">
						Drücken Sie die Eingabetaste, um die Suche zu starten
					</slot>

				</div>

				<div v-if="hint == 'searching'" key="searching">

					<svg xmlns="http://www.w3.org/2000/svg" width="5" height="10">
						<path fill="none" stroke="#000" stroke-width="1" d="M0 0 L5 5 L0 10"></path>
					</svg>

					<slot name="hint_searching">
						Suche läuft ..
					</slot>

				</div>

				<div v-if="hint == 'result'" key="result">

					<svg xmlns="http://www.w3.org/2000/svg" width="5" height="10">
						<path fill="none" stroke="#000" stroke-width="1" d="M0 0 L5 5 L0 10"></path>
					</svg>

					<slot name="hint_result">
						Ihre Suchanfrage für
						<span quotation>{{search_term}}</span>
						hat {{results.length}} Ergebnisse geliefert
					</slot>

				</div>

			</transition>

		</div>

		<slot name="start" v-if="results.length <= 0 && search_term.length <= 0"></slot>

		<transition-group class="ch-dkfbasel-search-results" tag="ul"
			@leave="leave"  @enter="enter" @before-enter="beforeEnter">

			<slot v-for="(item, index) in results" name="result" :result="item"
				:index="index">
				<li :key="index">
					No template provided: {{item}}
				</li>
			</slot>

		</transition-group>

	</div>
</template>

<script lang="babel">

	import dynamics from 'dynamics.js';

	module.exports = {

		props: ['autofocus', 'query'],

		data() {
			return {
				search_term: '',
				hint: '',
				results: []
			};
		},

		mounted() {
			if (this.autofocus === '') {
				this.$refs.searchbox.focus();
			}
		},

		methods: {

			onInput() {
				// inform the user that pressing enter will start the search
				if (this.hint != 'typing') {
					this.hint = 'typing';
					this.results = [];
				}
			},

			onSearch() {
				// inform the user that query is running
				this.hint = 'searching';

				// launch the search query
				let request = this.query();

				let delay = new Promise((resolve) => setTimeout(resolve, 1000));

				// delay the result display for 1 second if the search query is faster
				Promise.all([request, delay])
				.then((responses) => {
					this.results = responses[0];
					this.hint = 'result';
				})
			},

			onBlur() {
				// clear hint if the search box is empty
				if (this.search_term == '') {
					this.hint = '';
				}
			},

			// vue transition handling for the result group
			beforeEnter(el) {
				// set the opacity and translation to the initial value before
				// the animation starts
				dynamics.css(el, {
					opacity: 0,
					translateY: 50
				});

			},

			enter(el, done) {
				// accelerate the staggering with each result item
				var delay = Math.sqrt(el.dataset.index) * 50 + 150;
				// animate in result
				setTimeout(() => {
					dynamics.animate(el, {
						opacity: 1,
						translateY: 0
					}, {
						type: dynamics.easeOut,
						duration: 400,
						friction: 200
					});
				}, delay);
			},

			leave(el, done) {
				// stagger the result items animation out linear
				var delay = el.dataset.index * 50;
				// animate out result
				setTimeout(() => {
					dynamics.animate(el, {
						opacity: 0,
						translateY: 0
					}, {
						type: dynamics.easeOut,
						duration: 100,
						friction: 100
					});
				}, delay);
			}
		}

	}

</script>
