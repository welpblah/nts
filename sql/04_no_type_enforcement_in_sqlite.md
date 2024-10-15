# sql: no type enforcement in sqlite

- SQLite is different from other database systems in many ways. Most of those ways make it "lite", as its name suggests. That makes it a good database for learning.

- One of SQLite's most notable differences is that it doesn't enforce column types. It will happily let us insert an integer into a `TEXT` column, or insert a string into a `REAL` column.

Here's a code problem:

Finish this partial `INSERT` statement. It should insert a rectangle with a width of `'oh'` and a height of `'no'`. Many SQL databases won't allow this, but SQLite will.

```js
exec(`CREATE TABLE rectangles (width REAL, height REAL)`);
exec(`INSERT INTO rectangles (width, height) VALUES ('oh', 'no')`);
exec(`SELECT * FROM rectangles`);

/* [{width: 'oh', height: 'no'}] */
```

- A related quirk is that everything inserted into a text column is converted to a string. For example, inserting a number `7` into a text column will convert the number into a string, so it becomes `'7'`.

- In the example below, we mix up the name and age fields: we insert the name as the age, and the age as the name. Unfortunately, SQLite allows it.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
/* Remember: SQLite allows us to violate the columns' types. Most other
 * SQL databases don't allow this. Both columns will come back as
 * strings. */
exec(`INSERT INTO cats (name, age) VALUES (7, 'Catsup')`);
exec(`SELECT * FROM cats`);
/* [{name: '7', age: 'Catsup'}] */
```

- This SQLite quirk has a big downside for this course. We can't actually see the database rejecting incorrect data types. In other databases like PostgreSQL or MySQL, inserting a string into an integer column produces an error. Here's a PostgreSQL session where we try to make the same error:

```js
> CREATE TABLE cats (name TEXT, age INTEGER);
CREATE TABLE
> INSERT INTO cats (name, age) VALUES (7, 'Catsup');
ERROR:  invalid input syntax for integer: "Catsup"
LINE 1: INSERT INTO cats (name, age) VALUES (7, 'Catsup');
```

- Good news: the databases that you're likely to use in production applications will all enforce data types in this way!

---

## review #1

Here's a code problem:

Finish this partial `INSERT` statement. It should insert a rectangle with a width of `'oh'` and a height of `'no'`. Many SQL databases won't allow this, but SQLite will.

```sql
exec(`CREATE TABLE rectangles (width REAL, height REAL)`);
exec(`INSERT INTO rectangles (width, height) VALUES ('oh', 'no')`);
exec(`SELECT * FROM rectangles`);
[{width: 'oh', height: 'no'}]
```

## review #1

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
/* Remember: SQLite allows us to violate the columns' types. Most other
 * SQL databases don't allow this. Both columns will come back as
 * strings. */
exec(`INSERT INTO cats (name, age) VALUES (7, 'Catsup')`);
exec(`SELECT * FROM cats`);
// [{name: '7', age: 'Catsup'}]
```

## review #2

Here's a code problem:

Finish this partial `INSERT` statement. It should insert a rectangle with a width of `'oh'` and a height of `'no'`. Many SQL databases won't allow this, but SQLite will.

```js
exec(`CREATE TABLE rectangles (width REAL, height REAL)`);
exec(`INSERT INTO rectangles (width, height) VALUES ('oh', 'no')`);
exec(`SELECT * FROM rectangles`);
// [{width: 'oh', height: 'no'}]
```