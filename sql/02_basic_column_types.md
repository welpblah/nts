# sql: basic column types

- So far, we've only seen `TEXT` (string) columns. SQL supports other column types, including the ones that we're familiar with from other languages. `INTEGER` is what it sounds like: whole numbers, which can be positive or negative. `REAL` is a real number like 5.17, 0.00002, or 11, stored as an IEEE double precision floating point number. (This is the same data type as the `number` type in JavaScript.)

- When we query an `INTEGER` or `REAL` column, the values come back to us as JavaScript numbers. For example, in the query below, the age comes back as `3`, not `'3'`.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`SELECT * FROM cats`);

/* [{name: 'Ms. Fluff', age: 3}] */
```

Here's a code problem:

Create a `rectangles` table with two columns, `width` and `height`, both of which are `REAL` (instead of `TEXT` or `INTEGER`).

```js
exec(`CREATE TABLE rectangles (width REAL, height REAL)`);

exec(`INSERT INTO rectangles (width, height) VALUES (1.7, 2.1)`);
const queryResults = exec(`SELECT * FROM rectangles`);

/* This query examines the database's schema directly. That lets us
 * (the course's authors) verify that the column is actually a `REAL`.
 * This wouldn't be necessary in most SQL databases, which we'll
 * discuss in a future lesson. You don't need to understand this code
 * if you're not interested! */
const columnTypes = exec(`PRAGMA table_info(rectangles)`).map(
  ({type}) => type.toUpperCase()
);

[queryResults, columnTypes];

/*
[[{width: 1.7, height: 2.1}], ['REAL', 'REAL']]
*/
```

- Those are the only types that we'll see in this course: `TEXT`, `INTEGER`, and `REAL`. However, other database engines have many more types. PostgreSQL has [42 general-purpose data types](https://www.postgresql.org/docs/11/datatype.html), for example. It has special data types for currency, for points on a geometric plane, for individual bits, for IP addresses, for XML, and many more.

- Why so many data types? Because they make it easier to ensure that the data is correct!

- Imagine that we need to store IP addresses. If we store them as strings, that will allow incorrect values like "127.0.1.827262" or "abc123". The database can end up containing "IP addresses" that aren't valid, which can be difficult and expensive to fix after the fact.

- If our application's database has a column with PostgreSQL's IP address type, we can make that problem impossible. When a column has the IP address type, PostgreSQL guarantees us that we'll never be allowed to insert an incorrect address like "abc123" or "127.0.1.827262". If it ever allows us to insert an invalid IP address, that's a bug in PostgreSQL! (PostgreSQL is very reliable; you should never expect to encounter a bug like this.)

- This is one of the biggest selling points of SQL databases. We set up rules for our data's shape: this column is an integer and that one's an IP address. If we write a bug that tries to `INSERT` data that violates the columns' types, the database system will return an error. Runtime errors are annoying, but they're better than allowing invalid data into the database!

---

## review #1

```js
exec(`CREATE TABLE companies (name TEXT, year_founded INTEGER)`);
exec(`INSERT INTO companies (name, year_founded) VALUES ('Shure', 1925)`);
exec(`SELECT * FROM companies`);
/* [{name: 'Shure', year_founded: 1925}] */
```

## review #2

Here's a code problem:

Create a `rectangles` table with two columns, `width` and `height`, both of which are `REAL` (instead of `TEXT` or `INTEGER`).

```js
exec(`CREATE TABLE rectangles (width REAL, height REAL)`);
exec(`INSERT INTO rectangles (width, height) VALUES (1.7, 2.1)`);
const queryResults = exec(`SELECT * FROM rectangles`);

/* This query examines the database's schema directly. That lets us
 * (the course's authors) verify that the column is actually a `REAL`.
 * This wouldn't be necessary in most SQL databases, which we'll
 * discuss in a future lesson. You don't need to understand this code
 * if you're not interested! */
const columnTypes = exec(`PRAGMA table_info(rectangles)`).map(
  ({type}) => type.toUpperCase()
);

[queryResults, columnTypes];
// [[{width: 1.7, height: 2.1}], ['REAL', 'REAL']]
```

## review #3

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`SELECT * FROM cats`);
// [{name: 'Ms. Fluff', age: 3}]
```

