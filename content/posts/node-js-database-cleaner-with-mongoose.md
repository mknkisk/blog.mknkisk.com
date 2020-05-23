---
title: 'Node.js Database Cleaner with Mongoose'
date: 2017-02-15T22:46:34+09:00
draft: false
---

Env
 * Mac OSX 10.12.3
 * MongoDB 3.4.2
 * Node.js 7.5.0
 * Mocha 3.2.0
 * database-cleaner 1.2.0


--------------------------------------------------------------------------------

Install database-cleaner 🚿
$ yarn add -D database-cleaner
or
$ npm install -D database-cleaner


test/mocha.opts

--reporter spec
--recursive
--require test/setup


test/setup.js

const mongoose = require('mongoose')
const Cleaner = require('database-cleaner')
const dbCleaner = new Cleaner('mongodb')
global.dbCleaner = dbCleaner


test/models/user.js

const mongoose = require('mongoose')
beforeEach((done) => {
  // create test documents
  done()
})
afterEach((done) => {
  dbCleaner.clean(mongoose.connection.db, () => {
    done()
  })
})



--------------------------------------------------------------------------------

Running test 💚
$ npm test



--------------------------------------------------------------------------------

Link
 * emerleite/node-database-cleaner
   [https://github.com/emerleite/node-database-cleaner]