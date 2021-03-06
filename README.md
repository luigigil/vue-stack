# vue-stack
Nuxt, Vue.js, Vuetify, Express, NanoSQL, Node.js FullStack Boilerplate

[![NPM](https://nodei.co/npm/vue-stack.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/vue-stack/)

## Installation

```sh
git clone https://github.com/samuelnovaes/vue-stack.git
cd vue-stack
npm install
npm run dev
```

## Express back end

- There is a `server` directory with Express routes.

## NanoSQL models

- There is a `nsql` directory with NanoSQL models.

## HTTP request example

### You have to use `this.$axios` or import the `axios` module to make http request

pages/index.vue

```html
<template>
	<v-container>
		<v-btn @click="sendMsg">Send message to server</v-btn>
	</v-container>
</template>

<script>
	export default {
		methods: {
			sendMsg(){
				this.$axios.post('/msg', {msg: 'Hello Server!'}).then((res)=>{
					alert(res.data)
				})
			}
		}
	}
</script>
```

Or

```html
<template>
	<v-container>
		<v-btn @click="sendMsg">Send message to server</v-btn>
	</v-container>
</template>

<script>
	import axios from 'axios'
	export default {
		methods: {
			sendMsg(){
				axios.post('/msg', {msg: 'Hello Server!'}).then((res)=>{
					alert(res.data)
				})
			}
		}
	}
</script>
```

server/index.js

```javascript
module.exports = (app) => {
	app.post('/msg', (req, res)=>{
		console.log(req.body.msg)
		res.send("Hello client!")
	})
}
```

## How to use NanoSQL in vue-stack? (example)

nsql/index.js

```javascript
module.exports = (nSQL)=>{
	nSQL('people').model([
		{key: 'id', type: 'int', props: ['pk', 'ai']},
		{key: 'name', type: 'string'},
		{key: 'age', type: 'int'}
	])
}
```

server/index.js

```javascript
const {nSQL} = require('nano-sql') //Import nano-sql module

module.exports = (app) => {

	//Add person
	app.post('/add', (req, res)=>{
		nSQL('people').query('upsert', {name: req.body.name, age: req.body.age}).exec().then(()=>{
			res.end()
		})
	})

	//List people
	app.get('/list', (req, res)=>{
		nSQL('people').query('select').exec().then((rows)=>{
			res.json(rows)
		})
	})

}
```

## Some express request attributes

- `request.query` GET url query values
- `request.body` POST body values
- `request.files` multiparty files
- `request.session` manage sessions

## Commands

| Command | Description |
|---------|-------------|
| npm run dev | Start server in development with Nuxt.js in dev mode (hot reloading). Listen on [http://localhost:8080](http://localhost:8080). |
| npm run build | Build application for production. |
| npm start | Start server in production. |

## Documentation

- [ExpressJS](http://expressjs.com)
- [Nuxt.js](https://nuxtjs.org)
- [Vue.js](http://vuejs.org)
- [Vuetify](https://vuetifyjs.com)
- [NanoSQL](https://github.com/ClickSimply/Nano-SQL)
- [Node.js](https://nodejs.org)
- [Axios](https://github.com/mzabriskie/axios)
