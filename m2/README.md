
# concepts

## backend development
- module 2 project = backend rendered app (old school)
- http request/response cycle
- http headers & body
- http status codes
- data modelling
- nodejs (v8 in the backend)
- node modules & packages
- express & mongoose frameworks

## es6
- arrow functions have no context
- arrow functions usually as callbacks
- single line arrow functions (no brackets, implicit return)
- variables and constants
  - use const always
  - use let when const not possible
  - don't use var anymore
- es6 classes
- es6 with inheritance
- spread operator ...arr
- object shortcuts (implicit value)
- template (and multiline) strings

## nodejs
- **IT'S NOT A FRAMEWORK!**
- runtime environment for running javascript in the backend (v8 engine)
- app can be an http server (runs "forever")
- runs javascript, same as browser (but no window, no DOM)
- start apps with "node app.js"
- node callbacks convection (err, result) => { ... }
- has some built-in modules like `fs`, `process`, `path` and `http`+

## node modules
- every js file is a module
- every file has it's own scope (no global scope)
- npm packages are also modules
- for our files:
  - in mymodule.js do module.exports = ...
  - const mymodule = require('./folder/mymodule')
- for npm packages:
  - const express = require('express')

# npm
- http://npmjs.org
- npm init (new projects only)
- npm install (after cloning existing project)
- npm install --save package-name
- npm install --save-dev package-name
- npm install -g package-name (may require sudo)
- package.json (every node project needs one)
- always add "node_modules" to .gitignore

## package.json
- "scripts" are shortcuts
- "dependencies" are the packages the project needs to be executed
- "devDependencies" are the packages the developers need to work on the project

```json
"scripts": {
  "start": "node app.js",
  "start-dev": "nodemon --inspect app.js"
}
```

## http
- request, response
- request = headers + body (optional)
- response = headers + body (optional)
- url
  - `https://localhost:3000/homepage?foo=bar&baz=123#fragment`
  - `scheme://hostname:port/path?querystring` 
  - fragment is never sent to the server
- method: GET, POST, PUT, DELETE [and others](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- [status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status):
  - 2xx - success (e.g. 200 OK, 204 No Content)
  - 3xx - redirection (e.g. 301 moved permanently)
    - used with response header `Location: http://....` indicating where it moved too
  - 4xx - user error (e.g.: 404 not found, 401 not authorized)
  - 5xx - server error (e.g.: 500 internal error, 504 timeout)


## nodemon
- npm install -g nodemon
- nodemon --inspect app.js
- in package.json scripts
  - "start": "nodemon app.js"
  - "start-dev": "nodemon --inspect app.js"


## express generator
- [docs](https://expressjs.com/en/starter/generator.html)
- npm install -g express-generator
- express my-project --view=hbs --git
- add start-dev to package.json scripts
- add launcher.json
- add .eslintrc.json (eslint --init)
- git init
- add .gitignore with node_modules
- add our [error handling snippets](./express-apps/app.js) to app.js 

## express
- http server framework
- pipeline of middlewares, followed by routes
- see snippets

## auth
- use expression session (see snippet)
- 2x routes for login (get & post)
- 2x routes for signup (get & post)
- use post for logout
- signup: req.session.currentUser = newUser
- login: req.session.currentUser = user
- logout: req.session.currentUser = null
- user in views: req.locals.user = req.session.currentUser

## passport
- config (see snippet)
  - serialize
  - deserialize
  - use(new Strategy)
  - app.use(...);
  - app.use(...);
- req.login(newUser)
- req.logout()
- if (!req.user) { ... }
- if (!req.isAuthenticated()) { ... }

## mongodb
- document database (as opposedd to relational database)
- stores data as documents, schema free, but relationships still exist
- install mongodb, make sure it is running

## mongo shell
- $ mongo
- show dbs
- use databaseName
- db.help()
- db.createCollection("animals")
- show collections
- db.animals.help()
- db.animals.find().pretty()
- db.animals.insert({})
- [read operations](https://docs.mongodb.com/manual/crud/#read-operations)

## mongo import
- mongoimport  --db databaseName  --collection collectionName --file fileName

## mongoose
- npm install --save mongoose
- object document mapper
- bring schemas into our use of mongodb
- see example schemas in `./snippets`
- types: String, Number, Date, Boolean, Array, Mixed, Objectid

# best practices

## node



## express

- app.js middlewares before routes!
- separate your routes by prefix (e.g. '/auth', '/beers', ...)
- POST routes
  - check for authorization (e.g. `if (req.session.currentUser) ... ` 
  - always validate the POST body (e.g. `if (!req.body.username) ....`
  - always `res.redirect()` never `res.render()`
- GET routes
  - check for authorization (e.g. `if (req.session.currentUser) ... ` 
  - when loading items by id, check if DB returns a doc, and if it doesn't `return next()` to send to 404 middleware
  - use a `const data` object to send to `res.render('template', data)`
- always `.catch(next)` 
