# better-sqlite-sqlcipher

This package is a fork from **better-sqlite3 v5.4.3**, a custom build for sqlcipher on top of **sqlcipher v4.1.0** and **openssl v1.0.2s**

## install

```sh
npm install better-sqlite3-sqlcipher
```

## usage

```js
const db = new Database('./data.db');
db.pragma('key = "123"'); // if it was first time, it will set password
db.prepare('CREATE TABLE IF NOT EXISTS people (id INTEGER PRIMARY KEY, name TEXT)').run();
db.prepare('INSERT INTO people (name) VALUES (@name)').run({ name: 'jack' });

// next time
const db = new Database('./data.db');
db.pragma('key = "1234"'); // if you pass another password
db.prepare('SELECT * FROM people').all(); // this will throw
```


# better-sqlite3 [![Build Status](https://travis-ci.org/JoshuaWise/better-sqlite3.svg?branch=master)](https://travis-ci.org/JoshuaWise/better-sqlite3) [![Build status](https://ci.appveyor.com/api/projects/status/ilk8hb8v95m54v6f/branch/master?svg=true)](https://ci.appveyor.com/project/JoshuaWise/better-sqlite3/branch/master)


The fastest and simplest library for SQLite3 in Node.js.

- Full transaction support
- High performance, efficiency, and safety
- Easy-to-use synchronous API *(faster than an asynchronous API... yes, you read that correctly)*
- Support for user-defined functions, aggregates, and extensions
- 64-bit integers *(invisible until you need them)*

## Help this project stay strong! &#128170;

`better-sqlite3` is used by thousands of developers and engineers on a daily basis. Long nights and weekends were spent keeping this project strong and dependable, with no ask for compensation or funding, until now. If your company uses `better-sqlite3`, ask your manager to consider supporting the project:

- [Become a backer on Patreon](https://www.patreon.com/joshuawise)
- [Make a one-time donation on PayPal](https://www.paypal.me/joshuathomaswise)

## How other libraries compare

|   |select 1 row &nbsp;`get()`&nbsp;|select 100 rows &nbsp;&nbsp;`all()`&nbsp;&nbsp;|select 100 rows `iterate()` 1-by-1|insert 1 row `run()`|insert 100 rows in a transaction|
|---|---|---|---|---|---|
|better-sqlite3|1x|1x|1x|1x|1x|
|[sqlite](https://www.npmjs.com/package/sqlite) and [sqlite3](https://www.npmjs.com/package/sqlite3)|10.4x slower|2.2x slower|18.8x slower|2.8x slower|12.9x slower|

> You can verify these results by [running the benchmark yourself](./docs/benchmark.md).

## Installation

```bash
npm install --save better-sqlite3
```

> If you have trouble installing, check the [troubleshooting guide](./docs/troubleshooting.md).

## Usage

```js
const db = require('better-sqlite3')('foobar.db', options);

const row = db.prepare('SELECT * FROM users WHERE id=?').get(userId);
console.log(row.firstName, row.lastName, row.email);
```

## Why should I use this instead of [node-sqlite3](https://github.com/mapbox/node-sqlite3)?

- `node-sqlite3` uses asynchronous APIs for tasks that are either CPU-bound or serialized. That's not only bad design, but it wastes tons of resources. It also causes [mutex thrashing](https://en.wikipedia.org/wiki/Resource_contention) which has devastating effects on performance.
- `node-sqlite3` exposes low-level (C language) memory management functions. `better-sqlite3` does it the JavaScript way, allowing the garbage collector to worry about memory management.
- `better-sqlite3` is simpler to use, and it provides nice utilities for some operations that are very difficult or impossible in `node-sqlite3`.
- `better-sqlite3` is much faster than `node-sqlite3` in most cases, and just as fast in all other cases.

#### When is this library not appropriate?

In most cases, if you're attempting something that cannot be reasonably accomplished with `better-sqlite3`, it probably cannot be reasonably accomplished with SQLite3 in general. For example, if you're executing queries that take one second to complete, and you expect to have many concurrent users executing those queries, no amount of asynchronicity will save you from SQLite3's serialized nature. Fortunately, SQLite3 is very *very* fast. With proper indexing, we've been able to achieve upward of 2000 queries per second with 5-way-joins in a 60 GB database, where each query was handling 5–50 kilobytes of real data.

If you have a performance problem, the most likely causes are inefficient queries, improper indexing, or a lack of [WAL mode](./docs/performance.md)—not `better-sqlite3` itself. However, there are some cases where `better-sqlite3` could be inappropriate:

- If you expect a high volume of concurrent reads each returning many megabytes of data (i.e., videos)
- If you expect a high volume of concurrent writes (i.e., a social media site)
- If your database's size is near the terabyte range

For these situations, you should probably use a full-fledged RDBMS such as [PostgreSQL](https://www.postgresql.org/).

# Documentation

- [API documentation](./docs/api.md)
- [Performance](./docs/performance.md) (also see [benchmark results](./docs/benchmark.md))
- [64-bit integer support](./docs/integer.md)
- [SQLite3 compilation](./docs/compilation.md)

# License

[MIT](./LICENSE)
