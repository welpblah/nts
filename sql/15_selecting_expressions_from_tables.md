# sql: selecting expressions from tables

- When `SELECT`ing from a table, we can `SELECT` expressions computed from the table's columns.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);

// Note that this query will result in two object keys, not just one!
exec(`SELECT age, age + 1 AS age_next_year FROM cats`);
// [{age: 3, age_next_year: 4}]
```

- In JavaScript, we can concatenate strings like `"a" + "b"`. In SQLite, we concatenate with `"a" || "b"`. This syntax is unfortunate because `||` means "or" in most other languages. (In SQL, "or" is simply `OR`.)

```js
exec(`CREATE TABLE cats (name TEXT)`);
exec(`INSERT INTO cats (name) VALUES ('Ms. Fluff')`);
exec(`SELECT name || ' the cat' AS name FROM cats`);
// [{name: 'Ms. Fluff the cat'}]
```

- This doesn't change Ms. Fluff because `SELECT` never changes tables. That makes it safer to hack away at `SELECT`s when exploring the data.

- SQL dialects tend to have more built-in operators than other programming languages. For example, most programming languages have `>`, `<`, `>=`, and `<=`. SQL has those too.

- But most SQL dialects also have a special `BETWEEN` operator. In SQL, instead of `x >= y and x <= z`, we can say `x BETWEEN y AND z`. (Remember that SQLite represents `true` as `1` and `false` as `0`.)

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Katy Purry', 5)`);
// Note that there are two inserts above, so this query returns two rows!
exec(`SELECT name, age BETWEEN 2 AND 3 AS is_2_or_3 FROM cats`);
// [{name: 'Ms. Fluff', is_2_or_3: 1}, {name: 'Katy Purry', is_2_or_3: 0}]
```

- The `AND` in `x BETWEEN y AND z` is an interesting example of SQL syntax. Normally, `AND` in SQL is a "logical and", like `&&` in most programming languages. `SELECT 1 AND 1` returns 1, `SELECT 1 AND 0` returns 0, etc.

- SQL often reuses keywords, which is happening here. When we select `x BETWEEN y AND z`, the `AND` is part of `BETWEEN`. It has nothing to do with the logical `AND` of `SELECT x AND y`.

- We can think of this by analogy to JavaScript's syntax. In JavaScript objects like `{a: 1}`, `:` separates the property from its value. In JavaScript switch statements, we say `case x:`, with the `:` marking the case clause. Both of these use the same `:` character, but it means something different in each context. A similar thing is happening with `AND`. In SQL, `AND` can be either a "logical and" or it can be part of `BETWEEN`.

- SQL is making a trade-off here that doesn't exist in any popular language created since the 90s: it's using huge numbers of language keywords instead of providing functions.

- In JavaScript, most functions are required or imported from third-party NPM modules. In Python or Java, which have larger standard libraries, a lot of functions come with the language. In SQL, many "functions" aren't functions at all; they're syntax of the language itself, like `x BETWEEN y AND z`. This is why JavaScript has 64 keywords, but PostgreSQL's SQL dialect has [760](https://www.postgresql.org/docs/11/sql-keywords-appendix.html).

Here's a code problem:

Select two values from the cats table. First: the cats' names, with " the cat" appended to each. Second: `is_3_years_old`, a boolean (represented as 0 or 1 in SQLite). Remember that equality comparison in SQL is `=`, not `==` or `===`.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`SELECT name || ' the cat' AS name, age = 3 AS is_3_years_old FROM cats`);
// [{name: 'Ms. Fluff the cat', is_3_years_old: 1}, {name: 'Keanu the cat', is_3_years_old: 0}]
```

---

## review #1

Here's a code problem:

Select the number 17.

```js
exec(`SELECT 17`);
// [{'17': 17}]
```

## review #2

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);

// Note that this query will result in two object keys, not just one!
exec(`SELECT age, age + 1 AS age_next_year FROM cats`);
// [{age: 3, age_next_year: 4}]
```

## review #3

Here's a code problem:

Select two values from the cats table. First: the cats' names, with " the cat" appended to each. Second: `is_3_years_old`, a boolean (represented as 0 or 1 in SQLite). Remember that equality comparison in SQL is `=`, not `==` or `===`.

```js
exec(`CREATE TABLE cats (name TEXT, age INTEGER)`);
exec(`INSERT INTO cats (name, age) VALUES ('Ms. Fluff', 3)`);
exec(`INSERT INTO cats (name, age) VALUES ('Keanu', 2)`);
exec(`SELECT name || ' the cat' AS name, age = 3 AS is_3_years_old FROM cats`);
// [{name: 'Ms. Fluff the cat', is_3_years_old: 1}, {name: 'Keanu the cat', is_3_years_old: 0}]
```