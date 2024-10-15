# sql: inserting multiple rows

- When we insert many rows with separate `INSERT` statements, we're putting heavy load on the database. The database has to parse each insert statement, execute it safely by acquiring and releasing locks, and store its result on disk. All of those processes happen for every insert. There are many ways to mitigate those costs, but there's one way that reduces them all at once: multi-row inserts.

- The syntax is straightforward: we write an insert statement as normal, but with multiple rows of data after `VALUES`. Each one becomes a separate row in the database. Now, the database only has to parse, lock, execute, and store data once. The amount of data stored is the same, but the overhead is much smaller.

```js
exec(`CREATE TABLE users (name TEXT NOT NULL)`);
exec(`
  INSERT INTO users (name) VALUES
    ('Amir'),
    ('Betty'),
    ('Cindy')
`);
exec(`SELECT * FROM users`);
/* [{name: 'Amir'}, {name: 'Betty'}, {name: 'Cindy'}] */
```

Here's a code problem:

Use a multi-row insert statement to insert two cats, Ms. Fluff and Keanu.

```js
exec(`CREATE TABLE cats (name TEXT NOT NULL)`);
exec(`INSERT INTO cats (name) VALUES 
	 ('Ms. Fluff'), 
	 ('Keanu')
`);
exec(`SELECT * FROM cats`);

/* [{name: 'Ms. Fluff'}, {name: 'Keanu'}] */
```

- As usual, the database will still enforce constraints like `NOT NULL`.

```js
exec(`CREATE TABLE users (name TEXT NOT NULL)`);
exec(`
  INSERT INTO users (name) VALUES
    ('Amir'),
    (null),
    ('Cindy')
`);
/* Error: NOT NULL constraint failed: users.name */
```

---

## review #1

Here's a code problem:

Use a multi-row insert statement to insert two cats, Ms. Fluff and Keanu.

```js
exec(`CREATE TABLE cats (name TEXT NOT NULL)`);
exec(`INSERT INTO cats (name) VALUES
    ('Ms. Fluff'),
    ('Keanu')
`);
exec(`SELECT * FROM cats`);
/* [{name: 'Ms. Fluff'}, {name: 'Keanu'}] */
```

## review #2

```js
exec(`CREATE TABLE users (name TEXT NOT NULL)`);
exec(`
  INSERT INTO users (name) VALUES
    ('Amir'),
    (null),
    ('Cindy')
`);
// Error: NOT NULL constraint failed: users.name
```

## review #3

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
// []
```

## review #4

Here's a code problem:

Use a multi-row insert statement to insert two cats, Ms. Fluff and Keanu.

```js
exec(`CREATE TABLE cats (name TEXT NOT NULL)`);
exec(`INSERT INTO cats (name) VALUES ('Ms. Fluff'), ('Keanu')`);
exec(`SELECT * FROM cats`);
// [{name: 'Ms. Fluff'}, {name: 'Keanu'}]
```