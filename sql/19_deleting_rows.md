# sql: deleting rows

- Deleting data is as important as inserting it. Privacy laws like the [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation) even make it a legal requirement.

- We can delete data with the `DELETE` statement. Like `UPDATE`, it will delete every row by default. You'll want to be very careful with `DELETE`!

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.j@example.com', 'Betty')`);
exec(`DELETE FROM users`);
exec(`SELECT name FROM users`);
// []
```

- We can use a `WHERE` clause to limit the deletion. As with `SELECT` and `UPDATE`, the `WHERE` can include function calls, mathematical expressions, etc.

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.j@example.com', 'Betty')`);
exec(`DELETE FROM users WHERE name = 'Betty'`);
exec(`SELECT name FROM users`);
// [{name: 'Amir'}]
```

Here's a code problem:

If multiple rows match a `WHERE` clause, they'll all be deleted. Delete all of the people named "Amir" or "Betty".

```js
exec(`
  CREATE TABLE users (email TEXT, name TEXT);
  INSERT INTO users (email, name)
    VALUES ('amir@example.com', 'Amir');
  INSERT INTO users (email, name)
    VALUES ('betty.j@example.com', 'Betty');
  INSERT INTO users (email, name)
    VALUES ('betty.k@example.com', 'Betty');
  INSERT INTO users (email, name)
    VALUES ('cindy@example.com', 'Cindy');
`);
exec(`DELETE FROM users WHERE name = 'Amir' OR name = 'Betty'`);
exec(`SELECT name FROM users`);
// [{name: 'Cindy'}]
```

---

## review #1

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`INSERT INTO users (email, name) VALUES ('betty.j@example.com', 'Betty')`);
exec(`DELETE FROM users`);
exec(`SELECT name FROM users`);
// []
```

## review #2

Here's a code problem:

If multiple rows match a `WHERE` clause, they'll all be deleted. Delete all of the people named "Amir" or "Betty".

```js
exec(`
  CREATE TABLE users (email TEXT, name TEXT);
  INSERT INTO users (email, name)
    VALUES ('amir@example.com', 'Amir');
  INSERT INTO users (email, name)
    VALUES ('betty.j@example.com', 'Betty');
  INSERT INTO users (email, name)
    VALUES ('betty.k@example.com', 'Betty');
  INSERT INTO users (email, name)
    VALUES ('cindy@example.com', 'Cindy');
`);
exec(`DELETE FROM users WHERE name = 'Amir' OR name = 'Betty'`);
exec(`SELECT name FROM users`);
// [{name: 'Cindy'}]
```