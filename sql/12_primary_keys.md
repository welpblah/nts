# sql: primary keys

- Suppose that we have a table with two different people named Amir. That will make it difficult to find a specific Amir.

```js
exec(`CREATE TABLE people (name TEXT)`);
exec(`INSERT INTO people (name) VALUES ('Amir')`);
exec(`INSERT INTO people (name) VALUES ('Amir')`);
exec(`SELECT * FROM people WHERE name = 'Amir'`);
// [{name: 'Amir'}, {name: 'Amir'}]
```

- This puts us in a difficult situation as application developers. For example: how do we create user profile pages for these users? We can't have two different users' profiles at "/users/Amir"; that doesn't make sense.

- To solve this, most database tables have numerical IDs. With IDs, one Amir is person #1; the other is person #2.

```js

exec(`CREATE TABLE people (id INTEGER NOT NULL, name TEXT NOT NULL)`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Amir')`);
exec(`INSERT INTO people (id, name) VALUES (2, 'Amir')`);
exec(`SELECT * FROM people WHERE id = 1`);
// [{id: 1, name: 'Amir'}]
```

- Now our code can treat the Amirs differently. On our site, each Amir can have their own profile page: one at "/users/1" and the other at "/users/2".

- Most real-world tables have an integer ID because it makes referencing rows easy. But manually assigning those IDs is tedious. And what would happen if we accidentally assigned two users the same ID? Then we'd have the same problem that we started with: no way to distinguish between users with the same ID.

- It's much better to let the database choose IDs for us. In SQLite, we can do that by making the ID column a `PRIMARY KEY`.

- In databases, "key" means "a column or set of columns that is always unique". We've already seen keys in the form of uniqueness constraints. If we put a uniqueness constraint on one column, that makes it a key. If the constraint requires two columns together to be unique, then those two columns together are a key. Database systems themselves don't usually care about keys; they're more useful for humans as a shorthand for "a set of columns that's always unique".

- However, databases do care very much about primary keys. The primary key is a key that we've declared to be special: it's the main (primary) key for the table. In most real-world databases, the primary key is an integer column, often named `id`.

- When we tell the database that a column is the primary key, it does several things. First, the primary key column is automatically `UNIQUE`, so it can never have duplicate values. Second, in most databases, the primary key is also `NOT NULL`. This isn't true in SQLite, but let's ignore that for a moment.

- If we create a `PRIMARY KEY` column, then we won't be allowed to insert duplicates.

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT NOT NULL)`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Amir')`);
exec(`SELECT * FROM people`);
// [{id: 1, name: 'Amir'}]
```

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT NOT NULL)`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Amir')`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Betty')`);
exec(`SELECT * FROM people`);
// Error: UNIQUE constraint failed: people.id
```

- Unfortunately, `PRIMARY KEY`s are nullable in SQLite. There's an interesting story behind that, and it reiterates why ideas like nullability matter.

- An early version of SQLite had a bug that made primary keys nullable. SQLite has a strict backwards-compatibility policy, so it still maintains backwards compatibility with that bug. This is exactly the kind of problem we can have when we forget `NOT NULL`. Omitting `NOT NULL` might forever burden us with invalid `NULL`s in our database. We'll have to maintain backwards compatibility with our earlier mistake, supporting those `NULL`s forever. When the SQLite authors forgot to make `PRIMARY KEY` imply `NOT NULL`, they burdened themselves in a similar way. Now they have to maintain backwards compatibility with that bug forever.

- Let's see primary key nullability in action. We'll create a table with a primary key on a "name" text field. Unfortunately, SQLite will allow the primary key to be null.

```js
exec(`CREATE TABLE people (name TEXT PRIMARY KEY)`);
exec(`INSERT INTO people (name) VALUES (NULL)`);
exec(`SELECT * FROM people`);
// [{name: null}]
```

- To avoid the problem we just saw, always make your primary keys `NOT NULL`!

Here's a code problem:

Create a table "people" with a primary key on a "name" text column. Make sure the primary key is `NOT NULL`.

```js
exec(`CREATE TABLE people (name TEXT PRIMARY KEY NOT NULL)`);
/* This query examines the database's schema directly. That lets us
 * (the course's authors) verify that the column is `NOT NULL` and a
 * `PRIMARY KEY`. */
exec(`PRAGMA table_info(people)`).map(
  ({name, notnull, pk, type}) =>
    ({name, notnull, pk, type: type.toUpperCase()}
  )
);
// [{name: 'name', notnull: 1, pk: 1, type: 'TEXT'}]
```

- Most primary keys are auto-incrementing integer IDs. "Auto-incrementing" means that each new ID increases by 1 automatically. When we insert a record, we don't have to specify the ID. The first record inserted will get an ID of 1. After that, the database will always automatically choose the next unused integer. It's kind of like a default value, except it automatically changes with every new record.

- The exact syntax for this varies depending on the database. Other databases might require us to explicitly say that the column auto-increments. In SQLite, an `INTEGER PRIMARY KEY` column will auto-increment.

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY NOT NULL, name TEXT)`);
exec(`INSERT INTO people (name) VALUES ('Amir')`);
exec(`SELECT * FROM people`);
// [{id: 1, name: 'Amir'}]
```

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY NOT NULL, name TEXT)`);
exec(`INSERT INTO people (name) VALUES ('Amir')`);
exec(`INSERT INTO people (name) VALUES ('Betty')`);
exec(`SELECT * FROM people`);
// [{id: 1, name: 'Amir'}, {id: 2, name: 'Betty'}]
```

- Auto-incrementing integer primary keys are why you see many URLs on the web like "/users/71526". When you see that URL, you can guess that users have an auto-incrementing primary key ID. The user at "/users/71526" was probably the 71,526th user to register. You can guess at how many users exist by creating an account and looking at the numeric ID in your profile page's URL.

- We saw earlier that SQLite allows nulls in primary keys for backwards compatibility. However, SQLite tries to help us avoid that problem for `INTEGER PRIMARY KEY` columns. If we try to insert a `NULL`, SQLite will ignore the `NULL` and insert the next value in the ID sequence instead. For example, if the highest assigned ID is 1001, then our `NULL` will be replaced with 1002.

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT)`);
exec(`INSERT INTO people (id, name) VALUES (1001, 'Amir')`);
exec(`SELECT * FROM people`);
// [{id: 1001, name: 'Amir'}]
```

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT)`);
exec(`INSERT INTO people (id, name) VALUES (1001, 'Amir')`);
exec(`INSERT INTO people (id, name) VALUES (NULL, 'Amir')`);
exec(`SELECT * FROM people`);
// [{id: 1001, name: 'Amir'}, {id: 1002, name: 'Amir'}]
```

- One last detail about primary keys. A table can have only one primary key. If we try to create two primary keys in the same table, the database will error.

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT)`);
// []
```

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT PRIMARY KEY)`);
// Error: table "people" has more than one primary key
```

Primary keys in a nutshell:

- Each table can only have one primary key.
- Primary key values must be unique.
- Primary key values can't be `NULL` (but SQLite will allow `NULL` in some situations for backwards compatibility).
- Primary key columns can have any type, but most are automatically-incrementing integers starting at 1. In SQLite, an `INTEGER PRIMARY KEY` will automatically increment.

- Primary keys combine a lot of complicated ideas, but using them is relatively simple. Except in rare situations, your tables should always have an auto-incrementing integer primary key. You never need to include it in your inserts; the database will do it for you. Just by typing `INTEGER PRIMARY KEY`, you get an easy-to-use, automatically-managed ID column.

---

## review #1

Here's a code problem:

Create a table "people" with a primary key on a "name" text column. Make sure the primary key is NOT NULL.

```js
exec(`CREATE TABLE people (name TEXT PRIMARY KEY NOT NULL)`);
/* This query examines the database's schema directly. That lets us
 * (the course's authors) verify that the column is `NOT NULL` and a
 * `PRIMARY KEY`. */
exec(`PRAGMA table_info(people)`).map(
  ({name, notnull, pk, type}) =>
    ({name, notnull, pk, type: type.toUpperCase()}
  )
);
// [{name: 'name', notnull: 1, pk: 1, type: 'TEXT'}]
```

## review #2

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT NOT NULL)`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Amir')`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Betty')`);
exec(`SELECT * FROM people`);
// Error: UNIQUE constraint failed: people.id
```

## review #3

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY NOT NULL, name TEXT)`);
exec(`INSERT INTO people (name) VALUES ('Amir')`);
exec(`INSERT INTO people (name) VALUES ('Betty')`);
exec(`SELECT * FROM people`);
// [{id: 1, name: 'Amir'}, {id: 2, name: 'Betty'}]
```

## review #4

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT)`);
exec(`INSERT INTO people (id, name) VALUES (1001, 'Amir')`);
exec(`INSERT INTO people (id, name) VALUES (NULL, 'Amir')`);
exec(`SELECT * FROM people`);
// [{id: 1001, name: 'Amir'}, {id: 1002, name: 'Amir'}]
```

## review #5

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT PRIMARY KEY)`);
// Error: table "people" has more than one primary key
```

## review #6

Here's a code problem:

Create a table "people" with a primary key on a "name" text column. Make sure the primary key is `NOT NULL`.

```js
exec(`CREATE TABLE people (name TEXT PRIMARY KEY NOT NULL)`);
/* This query examines the database's schema directly. That lets us
 * (the course's authors) verify that the column is `NOT NULL` and a
 * `PRIMARY KEY`. */
exec(`PRAGMA table_info(people)`).map(
  ({name, notnull, pk, type}) =>
    ({name, notnull, pk, type: type.toUpperCase()}
  )
);
// [{name: 'name', notnull: 1, pk: 1, type: 'TEXT'}]
```

## review #7

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT NOT NULL)`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Amir')`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Betty')`);
exec(`SELECT * FROM people`);
// Error: UNIQUE constraint failed: people.id
```