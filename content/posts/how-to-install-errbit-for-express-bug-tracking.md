---
title: 'How to install Errbit for Express bug tracking 🐛'
date: 2017-02-15T22:35:00+09:00
draft: false
---

## ENV

* Mac OSX 10.12.3
* MongoDB 3.4.2
* Node.js 7.5.0
* Express 4.14.0

## Install MongoDB

```
$ brew install mongodb
$ brew services start mongodb
```

## Start Errbit at localhost

```
$ git clone https://github.com/errbit/errbit.git
$ bundle install --path vendor/bundle --jobs 4
$ bundle exec rake errbit:bootstrap
Creating an initial admin user:
-- email:    errbit@errbit.example.com
-- password: xxxxxxxxxxxx
Be sure to note down these credentials now!
$ bundle exec rails server -b 0.0.0.0 -p 3001
$ open localhost:3001
```



📝 Errbit run on port 3001, Express run on port 3000

“errbit:bootstrap” rake task to create admin user. Enter the email address and
password displayed on the console.

## Add a new app




## Install client for Airbrake (Errbit) in a Express application

```
$ yarn add airbrake
or
$ npm i -S airbrake
```

## Error Report

Send error information to Errbit when error occurred.



Let’s become a Bug Bounty Hunter 🤠

## Link

* [errbit/errbit](https://github.com/errbit/errbit)
* [airbrake/node-airbrake](https://github.com/airbrake/node-airbrake)
* [Express Exception Handling; Bug & Issue Tracker — Airbrake](https://airbrake.io/languages/express_exception_handling)
