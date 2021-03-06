[![Build Status](https://travis-ci.org/topleft/coin-api.svg?branch=master)](https://travis-ci.org/topleft/coin-api)


### Coin API

Authenticated API that allows the consumer to compare crypto currency prices between exchanges.

If you would like a front-end to consume this API, go over to [github.com/topleft/coin-client](https://github.com/topleft/coin-client).


### Dev Notes

#### Base Code:  

The basic scaffolding of this Node Express API started with a custom Yeomon express generator, and custom JWT Auth that I wrote for a previous project.

For this project I swapped out the existing Postgres/Knex configuration with Mongo/Mongoose and did the necessary refactoring.

All of this application was written using a Test Driven approach. The repository is synced with Travis-CI.

Some things that I would like to expand upon:
  - refactor to use async/await
  - expand available currency type by pulling from the Bittrex API
  - add more currency exchanges to compare rates
  - compare rates over a time span


### Run locally:

1. `$ git clone git@github.com:topleft/coin-api.git`
1. `$ cd coin-api`
1. `$ npm install`
1. in another terminal window fire up mongodb --> `$ mongod`
  - don't have mongodb installed? --> [how to install mongodb](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)
1. export the following env vars:
  ```sh

    export NODE_ENV=development
    export TOKEN_SECRET=super_secret
    export COIN_DB_DEV=mongodb://localhost/api
  ```
1. `$ gulp`


### End Points:

---
> ##### POST /auth/register

```
body  

      {
        user: {
          username: String, // > 6 characters
          password: String  // > 6 characters
        }
      }

response

      {
        message: String,
        token: JWT Token,
      }

```

---

> ##### POST /auth/login

```
body  

      {
        user: {
          username: String,
          password: String  
        }
      }

response

      {
        message: String,
        token: JWT Token,
      }

```
---

> ##### GET /auth/current_user

```
header  

      Authorization: 'Bearer ' + token

response

      {
        message: String,
        data: {
          username: String,
          _id: String
        }
      }

```

---
> ##### GET /currency/getLowestRate/:currencyCode

```
parameter

      currencyCode: String // ETH, LTC, DASH, BTC

header  

      Authorization: 'Bearer ' + token

response

      {
        message: String,
        data: {
          name: String,
          abbreviation: String,
          USDValueInPennies: Number,
          source: String
        }
      }

```

### Run the tests:

_Glad you asked :)_

1. In the root dir of the project, create a file called _.env-test_ and add the following code:
  ```sh

    export NODE_ENV=development
    export TOKEN_SECRET=super_secret
    export COIN_DB_DEV=mongodb://localhost/api
  ```
1. `$ npm test`
