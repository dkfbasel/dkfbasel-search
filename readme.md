# vue-search
vue-search is a fancy search component for vue.js 2.1+ taking advantage of the
newly implemented scoped slots in vue. Just pass your own search command and
define the presentation of hints and results as you like.

## Requirements
vue 2.1+
dynamics.js

## Installation
```
$ npm install vue-search --save
```

## Usage
Import vue-search into your vue component:

```javascript
import Search from 'vue-search';
```

Place the vue-search component inside your template and define the result
presentation and optionally a content to be presented from the beginning:

```javascript
<search autofocus :query="yourSearchQuery">

	<template slot="start">
		... // any html code, e.g.:
		<div class="start-info">Welcome to vue-search. What are you looking for?</div>
	</template>

	<template slot="result" scope="props">
		<li class="single-result" :key="props.index" :data-index="props.index">
			... // any html code
			// use {{props.result}} to access the result based on your data model
		</li>
	</template>

</search>
```

Optional you can also edit the hints for the following actions:

While user is typing:
```javascript
<template slot="hint_typing">
	... // any html code
</template>
```

While search is running:
```javascript
<template slot="hint_searching">
	... // any html code
</template>
```

When results are presented:
```javascript
<template slot="hint_result" scope="props">
	...
	// use {{props.term}} to display the term your were searching for
	// use {{props.amount}} to display the number of results
</template>
```

An fully functional example is provided in the test directory using
docker and webpack.

## Contributions
Any comments or suggestions are very welcome.
