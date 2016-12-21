# dkfbasel-search
dkfbasel-search is a search component for vue.js 2.1+ handling search inputs
and result rendering based on best practice experiences in terms of usability.
For the most flexibility we are taking advantage of the newly implemented scoped
slots in vue combined with vue transitions for high performance animations.

Its simple usage makes it the best choice for your search implementation.
Just pass your own search command and define the presentation of hints and
results as you like.

## Requirements
vue 2.1+
dynamics.js

## Installation
```
$ npm install dkfbasel-search --save
```

## Usage
Import dkfbasel-search into your vue component:

```javascript
import Search from 'dkfbasel-search';
```

Place the dkfbasel-search component inside your template and define the result
presentation and optionally a content to be presented from the beginning:

```javascript
<search autofocus :query="yourSearchQuery">

	<template slot="start">
		... // any html code, e.g.:
		<div class="start-info">Welcome to dkfbasel-search. What are you looking for?</div>
	</template>

	<template slot="result" scope="props">
		... // any html code
		// use {{props.result}} to access the result based on your data model, e.g.
		// if your data model looks like {title: "Cook book", description: "A book with lots of yammy food"}:
		<div class="result-item">
			<h2>{{props.result.title}}</h2>
			<span>{{props.result.description}}</span>
		</div>
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

Additional information before and after the search results can be added optionally:

To be shown before the result
```javascript
<template slot="before_result">
	... // any html code
</template>
```

To be shown after the result:
```javascript
<template slot="after_result">
	... // any html code
</template>
```

An fully functional example is provided in the test directory using
docker and webpack.

## Contributions
Any comments or suggestions are very welcome.
