# sql: null

- Like most programming languages, SQL supports "null", which indicates the absence of a value. For example, a user's `login_count` might be null if they've never logged in.

- When a column is allowed to be null, we say that it's "nullable". In SQLite and most other database systems, all columns are nullable unless we specify otherwise. Every column that we've seen up to this point has been nullable!

- Our queries return JavaScript data, so null is represented as the standard JavaScript `null`.

```js
exec(`CREATE TABLE users (name TEXT, login_count INTEGER)`);
exec(`INSERT INTO users (name, login_count) VALUES (NULL, NULL)`);
exec(`SELECT * FROM users`);
/* [{name: null, login_count: null}] */
```

- This isn't a quirk specific to SQLite. Most database systems make columns nullable by default, including PostgreSQL and MySQL.

- For the rest of this lesson, we'll explicitly mark columns as either `NULL` or `NOT NULL`. A column like `login_count INTEGER NULL` means that the column is nullable. That's the default, so the `NULL` isn't strictly required, but it's nice to make it explicit.

- We can tell the database to disallow null values by adding `NOT NULL` to a column declaration. Trying to insert a null value into a `NOT NULL` column is an error. You can type `error` at these prompts if you think that a query will produce an error.

```js
exec(`CREATE TABLE users (name TEXT NOT NULL, login_count INTEGER NULL)`);
exec(`INSERT INTO users (name, login_count) VALUES ('Amir', NULL)`);
exec(`SELECT * FROM users`);
/* [{name: 'Amir', login_count: null}] */
```

```js
exec(`CREATE TABLE users (name TEXT NOT NULL, login_count INTEGER NULL)`);
exec(`INSERT INTO users (name, login_count) VALUES (NULL, NULL)`);
exec(`SELECT * FROM users`);
/* Error: NOT NULL constraint failed: users.name */
```

- Like many things in SQL, `NOT NULL` is more important than it might seem at first. Let's imagine a production bug that shows why. (This story is fictional, but stories like it have happened in many real systems.)

- We have an `orders` table with a `phone_number` column. We accidentally introduce a bug where we sometimes insert a `NULL` `phone_number`.

Here's a code problem:

Write code that inserts a `NULL` `phone_number`.

```js
exec(`CREATE TABLE orders (phone_number TEXT NULL)`);
exec(`INSERT INTO orders (phone_number) VALUES (NULL)`);
exec(`SELECT * FROM orders`);

/* [{phone_number: null}] */
```

- We fix the bug after 12 hours. The system stops inserting nulls. Everything is fine again, right?

- Unfortunately, everything is not fine! We'd like to change the column to be `NOT NULL` to prevent this bug from recurring in the future. But the production database now contains some rows where `phone_number` is `NULL`. SQL databases won't let us change a column to be `NOT NULL` if it already contains some `NULL`s.

- We have a couple of relatively simple options for trying to clean this up once it's happened:

- First, we can leave our `phone_number` column nullable forever. But that means that we can make this mistake again, so it's not a good solution.

- Second, we can make up fake phone numbers and put those in the `phone_number` columns for the affected orders. Then there are no nulls left, so we can change the column to be `NOT NULL`. But now some of our customer data is intentionally made-up and therefore incorrect, so it's not a good solution.

- There are other, more creative solutions that we could use, but they involve permanently complicating the database schema. Once we have unexpected `NULL`s in the database, there's no good way to fix them.

- The best solution is to make the bug impossible in the first place. If we'd made `phone_number` column `NOT NULL` then none of this would've happened. Instead, the database would've errored at runtime when we tried to insert the `NULL`. We may have lost a few sales due to those errors, but we wouldn't be saddled with invalid database data!

- The takeaway here is: it's best to make columns `NOT NULL` unless you have a very good reason not to. When you do make a column nullable, it's best to explicitly mark it as `NULL` by defining it as `phone_number TEXT NULL`. That way, anyone who reads your schema later can clearly see which columns are intentionally nullable and non-nullable.

Here's a code problem:

Create a table "cats" with a `TEXT NOT NULL` name and an `INTEGER NULL` age.

```js
exec(`CREATE TABLE cats (name TEXT NOT NULL, age INTEGER NULL)`);
exec(`INSERT INTO cats (name, age) VALUES (NULL, 3)`);

/* Error: NOT NULL constraint failed: cats.name */
```

---

## review #1

```js
exec(`
  CREATE TABLE employees (
    name TEXT NOT NULL,
    employee_number INTEGER NULL
  )
`);
exec(`INSERT INTO employees (name, employee_number) VALUES ('Betty', NULL)`);
exec(`SELECT * FROM employees`);
/* [{name: 'Betty', employee_number: null}] */
```

## review #2

```js
exec(`CREATE TABLE users (name TEXT NOT NULL, login_count INTEGER NULL)`);
exec(`INSERT INTO users (name, login_count) VALUES (NULL, NULL)`);
exec(`SELECT * FROM users`);
// Error: NOT NULL constraint failed: users.name
```

## review #3

```js
exec(`CREATE TABLE cats (name TEXT NOT NULL, age INTEGER NULL)`);
exec(`INSERT INTO cats (name, age) VALUES (NULL, 3)`);
// Error: NOT NULL constraint failed: cats.name
```

## review #4

```js
exec(`CREATE TABLE users (name TEXT NOT NULL, login_count INTEGER NULL)`);
exec(`INSERT INTO users (name, login_count) VALUES ('Amir', NULL)`);
exec(`SELECT * FROM users`);
// [{name: 'Amir', login_count: null}]
```



