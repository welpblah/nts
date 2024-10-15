# sql: bind parameters

- SQL isn't very useful in isolation. In real systems, it will always be combined with a general-purpose language like JavaScript.

- Throughout this course, we've seen query results like `[{name: 'Amir'}, {name: 'Betty'}]`. Now we'll write JavaScript code to work with those query results.

- The simplest case is: we `INSERT` some rows, then access parts of them.

```js
exec(`CREATE TABLE users (name TEXT)`);
exec(`INSERT INTO users (name) VALUES ('Amir')`);
exec(`INSERT INTO users (name) VALUES ('Betty')`);
// []
```

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(`SELECT * FROM users`);
// [{name: 'Amir'}, {name: 'Betty'}]
```

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
const users = exec(`SELECT * FROM users`);
const amir = users[0];
amir.name;
// 'Amir'
```

- The most common type of query is "find me the record with this ID". We want to say `SELECT * FROM users WHERE id = ?`. But what goes in place of the "?"?

- The answer is that `?` is actually the correct thing to put there! A `?`, called a "bind parameter", tells SQLite "I (the programmer) will provide a value for you to insert here." We provide the bind parameter values as an array, which is passed as the second argument to our `exec` function.

```js
exec(`CREATE TABLE users (id INTEGER NOT NULL, name TEXT)`);
exec(`INSERT INTO users (id, name) VALUES (1, 'Amir')`);
exec(`INSERT INTO users (id, name) VALUES (2, 'Betty')`);
exec(`INSERT INTO users (id, name) VALUES (3, 'Cindy')`);

const users = exec(`SELECT name FROM users WHERE id = ?`, [2]);
const user = users[0];
user.name;
// 'Betty'
```

Here's a code problem:

Write a `findUser` function that finds a user by their ID, returning the result (an array of objects) that comes back from the database. Use a bind parameter (`?`) to provide the ID to the query. Remember that the ID needs to be passed in an array!

```js
exec(`CREATE TABLE users (id INTEGER NOT NULL, name TEXT)`);
exec(`INSERT INTO users (id, name) VALUES (1, 'Amir')`);
exec(`INSERT INTO users (id, name) VALUES (2, 'Betty')`);
exec(`INSERT INTO users (id, name) VALUES (3, 'Betty')`);

function findUser(id) {
  return exec(`SELECT * FROM users WHERE id = ?`, [id]);
}

[findUser(1), findUser(2), findUser(100)];
// [[{id: 1, name: 'Amir'}], [{id: 2, name: 'Betty'}], []]
```

- We can use more than one bind parameter if needed. Each one is a different parameter. If we have two `?` parameters in our query, we have to provide two values to fill those parameters.

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(
  `SELECT * FROM users WHERE id = ? AND name = ?`,
  [1, 'Amir']
);
// [{id: 1, name: 'Amir'}]
```

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(
  `SELECT * FROM users WHERE id = ? AND name = ?`,
  [2, 'Amir']
);
// []
```

- The `findUser` function we wrote earlier works, but it's annoying to use. By directly returning what it gets from the database, it always returns the user wrapped in an array. It also returns `[]` when the user isn't found. A better version would return just the user object, and `null` when the user doesn't exist.

Here's a code problem:

Write a `findUser` function that finds a user by their ID. Return the user object itself, not the full query result array. If no user is found, return JavaScript's `null`. (You can check for whether the query returned no results with `if (queryResult.length === 0) { ... }`.)

```js
exec(`CREATE TABLE users (id INTEGER NOT NULL, name TEXT)`);
exec(`INSERT INTO users (id, name) VALUES (1, 'Amir')`);
exec(`INSERT INTO users (id, name) VALUES (2, 'Betty')`);
exec(`INSERT INTO users (id, name) VALUES (3, 'Betty')`);

function findUser(id) {
  const queryResult = exec(`SELECT * FROM users WHERE id = ?`, [id]);
  if (queryResult.length === 0) {
    return null;
  } else {
    return queryResult[0];
  }
}

[findUser(1), findUser(2), findUser(100)];
// [{id: 1, name: 'Amir'}, {id: 2, name: 'Betty'}, null]
```

- When we do ``exec(`SELECT ... ?`, [1, 2])``, the `1` and `2` are called "bind parameters". The query contains some holes marked with `?`, and the parameters get bound to those holes.

- What if we want to reference a bind parameter multiple times in the query? Instead of `?`, we can reference `?1`, `?2`, etc. Parameter numbers start at 1, so `?1` refers to the bind parameter at index 0. Here's an example of `?1` in action:

To find all cats whose name matches their owner's name, we can compare them in a `WHERE`.

```js
exec(`CREATE TABLE cats (name TEXT, owner_name TEXT)`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Ms. Fluff', 'Amir')`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Wilford', 'Wilford')`);
exec(`SELECT name FROM cats WHERE name = owner_name`);
// [{name: 'Wilford'}]
```

- If we only care about one particular matching cat and owner (like Wilford and Wilford), then we can also find them using a bind parameter. We'll pass `'Wilford'` in as the bind parameter, then reference it twice with `?1`.

```js
exec(`CREATE TABLE cats (name TEXT, owner_name TEXT)`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Ms. Fluff', 'Amir')`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Wilford', 'Wilford')`);
exec(`SELECT name FROM cats WHERE name = ?1 AND owner_name = ?1`, ['Wilford']);
// [{name: 'Wilford'}]
```

- The syntax for these parameters varies by database. For example, PostgreSQL's bind parameters are referenced with `$1`, `$2`, etc.

- The query execution function also varies between different databases. In this course, we execute queries with `exec(query, bindParameters)`, which is a function that we (the course authors) defined while writing the course. In Node's PostgreSQL API, the equivalent function is called `query`. In Python's SQLite API, it's called `execute`.

- No matter the syntax, all SQL databases support bind parameters in some form. They wouldn't be very useful without it!

---

## review #1

Here's a code problem:

Write a `findUser` function that finds a user by their ID. Return the user object itself, not the full query result array. If no user is found, return JavaScript's `null`. (You can check for whether the query returned no results with `if (queryResult.length === 0) { ... }`.)

```js
exec(`CREATE TABLE users (id INTEGER NOT NULL, name TEXT)`);
exec(`INSERT INTO users (id, name) VALUES (1, 'Amir')`);
exec(`INSERT INTO users (id, name) VALUES (2, 'Betty')`);
exec(`INSERT INTO users (id, name) VALUES (3, 'Betty')`);

function findUser(id) {
  const queryResult = exec(`SELECT * FROM users WHERE id = ?`, [id]);
  if (queryResult.length === 0) { return null; }
  else { return queryResult[0]; }
}
[findUser(1), findUser(2), findUser(100)];


// [{id: 1, name: 'Amir'}, {id: 2, name: 'Betty'}, null]
```

## review #2

```js
exec(`CREATE TABLE cats (name TEXT, owner_name TEXT)`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Ms. Fluff', 'Amir')`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Wilford', 'Wilford')`);
exec(`SELECT name FROM cats WHERE name = ?1 AND owner_name = ?1`, ['Wilford']);
// [{name: 'Wilford'}]
```