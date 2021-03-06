# vue-cookie-bar

[![NPM](https://badgen.net/npm/v/vue-cookie-bar)](https://www.npmjs.com/package/vue-cookie-bar)
[![GitHub](https://badgen.net/github/release/rafalolszewski94/vue-cookie-bar)](https://github.com/rafalolszewski94/vue-cookie-bar)

## Usage
1. `npm install vue-cookie-bar` or `yarn add vue-cookie-bar`
2. Add `import VueCookieBar from 'vue-cookie-bar` somewhere in root of your application (for example `App.vue`
	```js
	<template>
	  <div id="app">
		<img alt="Vue logo" src="./assets/logo.png" />
		<HelloWorld msg="Welcome to Your Vue.js App" />
		<cookie-bar />
	  </div>
	</template>

	<script>
	import HelloWorld from "./components/HelloWorld.vue";
	import CookieBar from "vue-cookie-bar";

	export default {
	  name: "App",
	  components: {
		HelloWorld,
		CookieBar
	  }
	};
	</script>
	```

## Options
- `text`
- `acceptText`
- `linkText`
- `link`
- `customLink`
- `containered`
- `containerWidth`

## Contributing
- Run locally: `vue serve --open src/components/CookieBar.vue`
- Submit PR
