# sql: no booleans in sqlite

- Most database systems support booleans natively. In the name of simplicity, SQLite doesn't support them. Instead, we use integer columns to represent boolean values: 0 means false and 1 means true.

```js
exec(`CREATE TABLE cats (name TEXT, vaccinated INTEGER)`);
exec(`INSERT INTO cats (name, vaccinated) VALUES ('Ms. Fluff', 1)`);
exec(`INSERT INTO cats (name, vaccinated) VALUES ('Keanu', 0)`);
/* [] */
```

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(`SELECT name FROM cats WHERE vaccinated = 1`);
/* [{name: 'Ms. Fluff'}] */
```

Note: this code example reuses elements (variables, etc.) defined in earlier examples.

```js
exec(`SELECT name FROM cats WHERE vaccinated = 0`);
/* [{name: 'Keanu'}] */
```

---

## review #1

```js
exec(`CREATE TABLE employees (name TEXT, employee_number INTEGER)`);
exec(`INSERT INTO employees (name, employee_number) VALUES ('Amir', 1)`);
exec(`SELECT name FROM employees`);
/* [{name: 'Amir'}] */
```

## review #2

```js
exec(`CREATE TABLE cats (name TEXT, vaccinated INTEGER)`);
exec(`INSERT INTO cats (name, vaccinated) VALUES ('Ms. Fluff', 1)`);
exec(`INSERT INTO cats (name, vaccinated) VALUES ('Keanu', 0)`);

exec(`SELECT name FROM cats WHERE vaccinated = 1`);
// [{name: 'Ms. Fluff'}]
```

## review #3

```js
exec(`CREATE TABLE cats (name TEXT, vaccinated INTEGER)`);
exec(`INSERT INTO cats (name, vaccinated) VALUES ('Ms. Fluff', 1)`);
exec(`INSERT INTO cats (name, vaccinated) VALUES ('Keanu', 0)`);

exec(`SELECT name FROM cats WHERE vaccinated = 0`);
// [{name: 'Keanu'}]
```

## review #4

```js
exec(`CREATE TABLE cats (name TEXT, vaccinated INTEGER)`);
exec(`INSERT INTO cats (name, vaccinated) VALUES ('Ms. Fluff', 1)`);
exec(`INSERT INTO cats (name, vaccinated) VALUES ('Keanu', 0)`);

exec(`SELECT name FROM cats WHERE vaccinated = 1`);
// [{name: 'Ms. Fluff'}]
```

## review #5

```js
exec(`CREATE TABLE people (id INTEGER PRIMARY KEY, name TEXT NOT NULL)`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Amir')`);
exec(`INSERT INTO people (id, name) VALUES (1, 'Betty')`);
exec(`SELECT * FROM people`);
// 
```