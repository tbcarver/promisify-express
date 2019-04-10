# promisify-express

This package allows you to wrap express so that you can use promises as handlers, and any promise error will be neatly passed to express error handlers.

## Getting Started

Install the module with: `npm install promisify-express --save`.

## Documentation

Middleware and route handlers can simply return a promise. If the promise is rejected, ```promisify-express``` will
call ```next``` with the reason. This functionality removes the need to explicitly define a rejection handler.

```javascript
// Without promisify-express
const express = require('express')
const app = express()

app.get('/url', async (req, res, next) => {
  try {
    throw new Error('something went wrong')
  } catch (e) {
    next(e)
  }
})

app.use((err, req, res, next) => {
  console.error(err)
  res.status(500).send(err.message)
})

// With promisify-express
const express = require('express')
const promisifyExpress = require('promisify-express')
const app = express()
promisifyExpress(app)

app.get('/url', async (req, res, next) => {
  throw new Error('something went wrong')
})

app.use((err, req, res, next) => {
  console.error(err)
  res.status(500).send(err.message)
})

```
