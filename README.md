# Rails Ranger
### Exploring the routes and paths of Rails APIs
> This library development is in a very early stage and the API is **VERY** unstable

#### [Github Repository](https://github.com/victor-am/rails-ranger) | [Documentation](https://victor-am.github.io/rails-ranger)

[![npm version](https://badge.fury.io/js/rails-ranger.svg)](https://badge.fury.io/js/rails-ranger)
[![Travis build status](http://img.shields.io/travis/victor-am/rails-ranger.svg?style=flat)](https://travis-ci.org/victor-am/rails-ranger)
[![Test Coverage](https://codeclimate.com/github/victor-am/rails-ranger/badges/coverage.svg)](https://codeclimate.com/github/victor-am/rails-ranger)
[![Dependency Status](https://david-dm.org/victor-am/rails-ranger.svg)](https://david-dm.org/victor-am/rails-ranger)
[![devDependency Status](https://david-dm.org/victor-am/rails-ranger/dev-status.svg)](https://david-dm.org/victor-am/rails-ranger#info=devDependencies)
[![Stories in Ready](https://badge.waffle.io/victor-am/rails-ranger.png?label=ready&title=Ready)](https://waffle.io/victor-am/rails-ranger?utm_source=badge)

Rails Ranger is a thin layer on top of [Axios](https://github.com/mzabriskie/axios), which gives you an opinionated interface to query APIs built with Ruby on Rails.

## Installation
```bash
npm install --save rails-ranger
```

**or**

```bash
yarn add rails-ranger
```
<br>

## How does it work?

The following should serve as a simple illustration of the library API:

```javascript
import RailsRanger from 'rails-ranger'

const api = new RailsRanger({
  axios: { baseURL: 'http://api.myapp.com' }
})

api.list('users').then((response) => {
  const users = response.data
})
```

And here is a commented version:

```javascript
import RailsRanger from 'rails-ranger'

const api = new RailsRanger({
  // The options below will be handed down to the Axios library
  axios: { baseURL: 'http://api.myapp.com' }
})

// This makes a GET request to http://api.myapp.com/users/
api.list('users').then((response) => {
  // Your JSON keys were already converted to camelCase, ready for use ;)
  const users = response.data
})
```

The `list` method above will make a request to the **index** path of the **users** resource, following Rails routing conventions. This means a `GET` request to the `/users` path.

> **Observation:** you can use `api.index('users')` as well. The `list` function is just an alias for it.
<br>

### A sightly more complex example:

```javascript
api.show('users', { id: 1, expanded: false })
// => GET request to /users/1?expanded=false
```
<br>

## Options
As the first argument when creating a new instance of Rails Ranger you can pass an object of options to customize the behavior of Rails Ranger.

### dataTransform
**default: true**

By default RailsRanger will convert camelCased keys in your jsons to snake_case when sending a request to Rails, and will convert the Rails response from snake_case to camelCase for you to use in your front-end.

You can disable this behavior by setting `dataTransform` to false:

```javascript
const api = new RailsRanger({ dataTransform: false })
```

### axios
**default: {}**

Any object passed to the `axios` option will be handled to **Axios**.
Here are some usage examples:

#### Base URL
```javascript
const api = new RailsRanger({ axios: { baseUrl: 'http://myapp.com/api' } })

api.list('users')
// => GET request to http://myapp.com/api/users
```

#### Timeout
```javascript
const api = new RailsRanger({ axios: { timeout: 3000 } })

api.list('users') // => Will timeout within 3000 miliseconds
```

#### See more in the [Axios documentation](https://github.com/mzabriskie/axios#request-config)
<br>

## Using Rails Ranger just for building routes
You don't need to use Rails Ranger as an ajax client if you don't want to. It can also be used just to generate the routes from your resources. To use Rails Ranger this way you can do the following:

```javascript
import { RouteBuilder } from RailsRanger
const route = new RouteBuilder

route.create('users', { name: 'John' })
// => { path: '/users', params: { name: 'John' }, method: 'post' }

route.show('users', { id: 1, hidePassword: true })
// => { path: '/users/1?hide_password=true', params: {}, method: 'get' }

route.get('/:api/documentation', { api: 'v1', page: 3 })
// => { path: 'v1/documentation?page=3', params: {}, method: 'get' }
```

## Actions (pending)

- list/index
- show
- new
- create
- edit
- update
<br>

## Methods (pending)

- GET
- POST
- PATCH
- PUT
- DELETE
<br>
