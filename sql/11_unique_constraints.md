# sql: unique constraints

- Most SQL databases enforce column types, so we can't accidentally insert a number where we expect a string. But there are other types of constraints that we'd like to have. For example, sometimes we want a column to have unique values.

- Email addresses are a good example. Most sites don't allow two people to register with the same email address. We can use the `UNIQUE` keyword to tell the database to enforce that rule for us. Inserting a duplicate value into a `UNIQUE` column is an error. (Remember that you can type `error` if the expression will result in an error!)

```js
exec(`CREATE TABLE users (email TEXT UNIQUE, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`SELECT email FROM users`);
/* [{email: 'amir@example.com'}] */
```

```js
exec(`CREATE TABLE users (email TEXT UNIQUE, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`SELECT * FROM users`);
/* Error: UNIQUE constraint failed: users.email */
```

- Unique constraints only affect the column they're defined on. The next example has a `UNIQUE` constraint on the email address, but not the name. It will reject users with the same email address, even if their names are different.

```js
exec(`CREATE TABLE users (email TEXT UNIQUE, name TEXT)`);
exec(`
  INSERT INTO users (email, name) VALUES ('alex@example.com', 'Alexandra')
`);
exec(`
  INSERT INTO users (email, name) VALUES ('alex@example.com', 'Alexander')
`);
exec(`SELECT * FROM users`);
/* Error: UNIQUE constraint failed: users.email */
```

- Now imagine that we store the email address in a different way. When a user registers, we split it in half at the `@`, storing it as `username` and `domain`. Now, `alex@example.com` is stored as two columns: `username` is "alex", and `domain` is "example.com". How do we make sure that there are no duplicate email addresses?

- We could try putting unique constraints on both columns.

```js
exec(`CREATE TABLE emails (username TEXT UNIQUE, domain TEXT UNIQUE)`);
/* [] */
```

- However, those are separate unique constraints! The first one means "the username must be unique" and the second means "the domain must be unique". Neither uniqueness constraint cares about the other column. Once we have a user with an "@example.com" email address, this schema will reject any other user with an "@example.com" address. Likewise, once we have a user with the username "amir", it will reject any other username of "amir", even if the domain is different.

```js
exec(`CREATE TABLE emails (username TEXT UNIQUE, domain TEXT UNIQUE)`);
exec(`INSERT INTO emails (username, domain) VALUES ('amir', 'example.com')`);
/* [] */
```

```js
exec(`CREATE TABLE emails (username TEXT UNIQUE, domain TEXT UNIQUE)`);
exec(`INSERT INTO emails (username, domain) VALUES ('amir', 'example.com')`);
exec(`INSERT INTO emails (username, domain) VALUES ('betty', 'example.com')`);
/* Error: UNIQUE constraint failed: emails.domain */
```

```js
exec(`CREATE TABLE emails (username TEXT UNIQUE, domain TEXT UNIQUE)`);
exec(`INSERT INTO emails (username, domain) VALUES ('amir', 'example.com')`);
exec(`INSERT INTO emails (username, domain) VALUES ('amir', 'example.net')`);
/* Error: UNIQUE constraint failed: emails.username */
```

- To fix this, we need to tell the database to reject any user where the username and the domain are both identical. We can't put it in the column list, because it doesn't apply to just one column. Instead, we put it in the table definition, after the column list. Then we can insert two users with the same domain.

```js
exec(`
  CREATE TABLE emails (
    username TEXT,
    domain TEXT,
    UNIQUE (username, domain)
  )
`);
exec(`INSERT INTO emails (username, domain) VALUES ('amir', 'example.com')`);
exec(`INSERT INTO emails (username, domain) VALUES ('betty', 'example.com')`);
/* [] */
```

- The constraint will correctly stop us from inserting two users where the username and domain are both identical.

```js
exec(`
  CREATE TABLE emails (
    username TEXT,
    domain TEXT,
    UNIQUE (username, domain)
  )
`);
exec(`INSERT INTO emails (username, domain) VALUES ('amir', 'example.com')`);
exec(`INSERT INTO emails (username, domain) VALUES ('amir', 'example.com')`);
/* Error: UNIQUE constraint failed: emails.username, emails.domain */
```

- Now we can store email addresses as two parts without risking any duplicates!

- Uniqueness constraints are very important. They can save us from bugs that would insert invalid data into the database. This is similar to how `NOT NULL` saves us from accidental `NULL`s. `UNIQUE` and `NOT NULL` are both examples of a more general idea in databases: constraints.

- Enforcing constraints is one of the main jobs of a SQL database. The database will always check the constraints before allowing any change to the data.

- So far we've seen constraints enforced during `INSERT`. But they're also enforced during `UPDATE`. We can `UPDATE` our new emails table as much as we like. But if we try to update it in a way that results in two rows with the same username and the same domain, that's an error.

```js
exec(`
  CREATE TABLE emails (
    username TEXT,
    domain TEXT,
    UNIQUE (username, domain)
  )
`);
exec(`INSERT INTO emails (username, domain) VALUES ('amir', 'example.com')`);
exec(`INSERT INTO emails (username, domain) VALUES ('betty', 'example.com')`);
/* [] */
```

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(`UPDATE emails SET username = 'amir' WHERE username = 'betty'`);
/* Error: UNIQUE constraint failed: emails.username, emails.domain */
```

Here's a code problem:

Create a cats table with two columns, `name` (the name of the cat) and `owner_name`. Add a uniqueness constraint to ensure that an owner can't have two cats with the same name.

```js
exec(`CREATE TABLE cats (
    name TEXT NOT NULL,
    owner_name TEXT NOT NULL,
    UNIQUE (name, owner_name)
  )
`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Ms. Fluff', 'Amir')`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Ms. Fluff', 'Amir')`);
```

---

## review #1

```js
exec(`CREATE TABLE users (email TEXT UNIQUE, name TEXT)`);
exec(`
  INSERT INTO users (email, name) VALUES ('alex@example.com', 'Alexandra')
`);
exec(`
  INSERT INTO users (email, name) VALUES ('alex@example.com', 'Alexander')
`);
exec(`SELECT * FROM users`);
// Error: UNIQUE constraint failed: users.email
```

## review #2

Here's a code problem:

Create a cats table with two columns, `name` (the name of the cat) and `owner_name`. Add a uniqueness constraint to ensure that an owner can't have two cats with the same name.

```js
exec(`CREATE TABLE cats (
    name TEXT NOT NULL,
    owner_name TEXT NOT NULL,
    UNIQUE (name, owner_name)
  )
`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Ms. Fluff', 'Amir')`);
exec(`INSERT INTO cats (name, owner_name) VALUES ('Ms. Fluff', 'Amir')`);
// Error: UNIQUE constraint failed: cats.name, cats.owner_name
```

## review #3

```js
exec(`CREATE TABLE users (email TEXT UNIQUE, name TEXT)`);
exec(`
  INSERT INTO users (email, name) VALUES ('alex@example.com', 'Alexandra')
`);
exec(`
  INSERT INTO users (email, name) VALUES ('alex@example.com', 'Alexander')
`);
exec(`SELECT * FROM users`);
// Error: UNIQUE constraint failed: users.email
```