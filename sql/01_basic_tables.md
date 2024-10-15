# sql: basic tables

- This course teaches the SQL language from the ground up. It assumes no prior SQL experience. We'll use basic JavaScript syntax in some places. You should know the JavaScript syntax for `{}` objects and `[]` arrays, and be able to declare `function`s. You'll also have to write `if () { ... } else { ... }` statements later in the course.

- SQL (pronounced as either S-Q-L or "sequel") isn't a general-purpose programming language. You won't use it to write your backend server or your client application or a command line tool. It does one thing very well: it manages data.

- In SQL, data is stored in tables made up of columns. You can think of each table like a spreadsheet. A spreadsheet of users might have an "email" column and a "name" column. Each row represents one user with its own email address and name.

- The example below creates that "users" table as a SQL database. We `CREATE` a table, then `INSERT` a user into it, then `SELECT` the user back out. We'll go into more detail on each of these pieces, but it's nice to start by seeing it all working together.

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
exec(`SELECT * FROM users`);
/* [{email: 'amir@example.com', name: 'Amir'}] */
```

- `exec` is a JavaScript function that we've provided. It accepts a string containing SQL code, then runs that SQL in our database, SQLite. In later lessons, we'll write code that mixes SQL and JavaScript, like real applications do.

- We can specify which columns we want when `SELECT`ing data via SQL. In this introductory lesson we'll always do `SELECT *`, which means "give me all the columns".

Here's a code problem:

Select `*` to retrieve the user from the database.

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
/* answer below */
exec(`SELECT * FROM users`);

/* [{email: 'amir@example.com', name: 'Amir'}] */
```

- Executing a `SELECT` always returns an array of objects. In the example above, there was one user in the database, so the array contained that one user. Queries always return arrays, even when the query returns only one row

Here's a code problem:

Modify this code to insert a second user into the database. Then the `SELECT` will return both of them. The second user is Betty, whose email address is betty@example.com.

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
/* answer below */
exec(`INSERT INTO users (email, name) VALUE ('betty@example.com', 'Betty')`);
/* answer above */
exec(`SELECT * FROM users`);

/* 
[{email: 'amir@example.com', name: 'Amir'}, {email: 'betty@example.com', name: 'Betty'}]
*/
```

- At this next prompt, write the expected return value of the `SELECT` query. The query will return an array of objects, like above. This time, there's more than one record.

```js
exec(`CREATE TABLE users (name TEXT)`);
exec(`INSERT INTO users (name) VALUES ('Amir')`);
exec(`INSERT INTO users (name) VALUES ('Betty')`);
exec(`SELECT * FROM users`);

/*
[{name: 'Amir'}, {name: 'Betty'}]
*/
```

Here's a code problem:

When there's nothing in the table, `SELECT`ing will return `[]`. Select all the users so we can see that empty result.

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`SELECT * FROM users`);

/* [] */
```

- A table can have as many columns as we like. (Within reason: SQLite allows [up to 2,000 columns](https://www.sqlite.org/limits.html) by default, but can be configured to allow as many as 32,767.)

Here's a code problem:

Use the "CREATE TABLE" syntax to create a table called `cats` with one `TEXT` column, `name`.

```js
exec(`CREATE TABLE cats (name TEXT)`);

exec(`INSERT INTO cats (name) VALUES ('Ms. Fluff')`);
exec(`SELECT * FROM cats`);

/* [{name: 'Ms. Fluff'}] */
```

- `SELECT`'s only job is to retrieve data from the database. That returns an array of objects. But `CREATE` and `INSERT` don't retrieve data. Neither do `DELETE`, `ALTER`, and `BEGIN`, which we'll learn about in later lessons. However, executing these SQL statements has to return something.

- For consistency, all SQL statements return arrays. Statements like `CREATE`, `INSERT` that don't retrieve data will return `[]`, an empty array of rows. (That's what the next example returns.)

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
/* [] */
```

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
exec(`INSERT INTO users (email, name) VALUES ('amir@example.com', 'Amir')`);
/* [] */
```

- In this introductory lesson, we've only used string columns. (We'll see other column types later.) We use SQLite's `TEXT` type to store strings. Most databases provide `TEXT` as well as other specialized types of strings, but the details vary from database to database.

- SQL keywords like `INSERT` and `SELECT` ignore case, so `INSERT` and `InSeRt` mean the same thing. Table and column names also ignore case, so `users` and `USErs` refer to the same table.

- In this course, we'll SHOUT SQL keywords in UPPERCASE, like `CREATE` and `INSERT`. Our tables and columns will be lower_snake_case, like `user_name`. This is a common convention that makes it easier to see what's a SQL keyword and what isn't. When defining JavaScript functions and variables, we'll use `lowerCamelCase`, which is that community's convention.

- Fortunately, SQL does respect case within strings. `'a'` and `'A'` are different strings that are not equal. (Pay attention to the case of the strings in this next example.)

```js
exec(`CREATE TABLE users (email text, name text)`);
exec(`insert into users (email, name) values ('AmiR@example.com', 'Amir')`);
exec(`SeLeCt * FrOm users`);
/* [{email: 'AmiR@example.com', name: 'Amir'}] */
```

- If we try to insert into a column that doesn't exist, the database system will error. (When you think that code in this course will cause an error, you can type `error` as its output.)

```js
exec(`CREATE TABLE users (name TEXT)`);
exec(`INSERT INTO users (name, age) VALUES ('Amir', 36)`);
/* Error: table users has no column named age */
```

- Any operation on a column that doesn't exist will cause an error. It doesn't matter whether it's a `SELECT`, an `INSERT`, or one of the other kinds of operations that we'll see later. If you ever think that a code example will error, you can type `error`.

```js
exec(`<BAD>{BAD}[BAD] This is invalid SQL syntax!`);
/* Error: near "<": syntax error */
```

Here's a summary of this lesson.

Here's a code problem:

`CREATE` a cats table with a `TEXT` name column, then `INSERT` the cat "Keanu" into the table, then `SELECT` Keanu back out. Because you're writing multiple statements, you'll have to type out three `exec()`s.

```js
exec(`CREATE TABLE cats (name TEXT)`);
exec(`INSERT INTO cats (name) VALUES ('Keanu')`);
exec(`SELECT * FROM cats`);
```

- You now know the basics of SQL! All of SQL's power builds on the simple ideas that we just saw: create a table, insert data, select it back out. This course builds on those simple ideas, step by step, eventually building up to advanced SQL features like joining, constraints, and views.

---

## review #1

```js
exec(`CREATE TABLE countries (name TEXT, abbreviation TEXT)`);
/* [] */
```

## review #2

```js
exec(`CREATE TABLE countries (name TEXT)`);
exec(`INSERT INTO countries (name) VALUES ('Croatia')`);
exec(`INSERT INTO countries (name) VALUES ('Senegal')`);
exec(`SELECT * FROM countries`);
/* [{name: 'Croatia'}, {name: 'Senegal'}] */
```

## review #3

```js
exec(`CREATE TABLE users (name TEXT)`);
exec(`INSERT INTO users (name, age) VALUES ('Amir', 36)`);
// Error: table users has no column named age
```

## review #4

Here's a code problem:

`CREATE` a cats table with a `TEXT` name column, then `INSERT` the cat "Keanu" into the table, then `SELECT` Keanu back out. Because you're writing multiple statements, you'll have to type out three `exec()`s.

```js
exec(`CREATE TABLE cats (name TEXT)`);
exec(`INSERT INTO cats (name) VALUES ('Keanu')`);
exec(`SELECT * FROM cats`);
// [{name: 'Keanu'}]
```

## review #5

```js
exec(`CREATE TABLE users (email TEXT, name TEXT)`);
// []
```

## review #6

```js
exec(`CREATE TABLE users (name TEXT)`);
exec(`INSERT INTO users (name) VALUES ('Amir')`);
exec(`INSERT INTO users (name) VALUES ('Betty')`);
exec(`SELECT * FROM users`);
// [{name: 'Amir'}, {name: 'Betty'}]
```