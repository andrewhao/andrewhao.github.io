---
layout: post
title: "Knex.js and PostGIS cheat sheet"
date: 2016-04-08 12:33
comments: true
categories: 
- Javascript
- Knex.js
- PostGIS
---

As follows are some code snippets for using [Knex.js](http://knexjs.org/) for executing
Postgres and PostGIS queries.

### Execute raw SQL in migration

I often find this useful for fancy SQL, like creating views.

```js
exports.up = function(knex, Promise) {
  return knex.raw(`YOUR RAW SQL`);
};
```

### Add a PostGIS Point type to a table in a migration:

```js
return knex.schema.table('events', function(table) {
  table.specificType('point', 'geometry(point, 4326)');
})
```

### Add a foreign key to another table.

```js
return knex.schema.table('events', function(table) {
	table.integer('device_id').references('id').inTable('devices');
});
```

### Add a multi-column unique index

```js
return knex.schema.table('events', function(table) {
  table.unique(['start_time', 'end_time', 'start_location', 'end_location', 'distance_miles']);
});
```

### Find a collection

```js
knex.select('*')
.from('participants')
.where({ name: 'Jason' })
.andWhere('age', '>=', 20)
```

### Custom operations in SELECT clause

```js
knex('trips')
.select(knex.raw('miles * passengers as passenger_miles'))
.select(knex.raw("CONCAT('Hello, ', name) as greeting_message"))
```

### Return PostGIS data from a spatial column:

We use [knex-postgis](https://github.com/jfgodoy/knex-postgis) to gain access to PostGIS functions in Postgres. Here, we return a 'point' column with `ST_AsGeoJSON`:

```js
const knexPostgis = require('knex-postgis')(knex);

knex('events')
.select('*', knexPostgis.asGeoJSON('point'));
```

See [knex-postgis](https://github.com/jfgodoy/knex-postgis) documentation for a list of other PostGIS functions that are supported.
