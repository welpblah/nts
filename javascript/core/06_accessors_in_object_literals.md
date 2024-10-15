# core: accessors in object literals

JavaScript has always had object literals:

```js
const user = {
  userName: 'Amir'
};
user.userName;
// 'Amir'
```

With the object syntax above, we can only create objects with fixed properties. To change a property's value, we have to modify the object.

But a property doesn't have to hold a fixed value, like `'Amir'`. It can also hold a function. To access it, we add a function call to the property, like `user.getAge()`.

```js
const user = {
  getAge: function() { return 'I am ' + 22; }
};
user.getAge();
// 'I am 22'
```

Modern JavaScript streamlines this syntax with the `get` keyword. To use it, we declare the object property as a function starting with `get`. When we access the property with the usual `user.age` syntax, the function is automatically called and we get its return value. This kind of property is called a "getter".

```js
const user = {
  get age() { return 'Amir'; }
};
user.age;
// 'Amir'
```

Because the function above returns a fixed string, it behaves like a regular property would. But the getter function doesn't have to return a constant value. It can return anything we want.

```js
const user = {
  get age() { return 'I am ' + 38; }
};
user.age;
// 'I am 38'
```

Here's a code problem:

Add a `userName` getter to this object. It should return the value of the `name` variable.

```js
let name = 'Amir';

const user = {
  get userName() { return name; }
};

const userName1 = user.userName;
name = 'Betty';
const userName2 = user.userName;
[userName1, userName2];

// ['Amir', 'Betty']
```

The `get` properties above are called getters. As you might suspect, there's a complementary feature for writing to objects called setters.

Normally, writing to an object's key replaces the value at that key:

```js
const user = {userName: 'Amir'};
user.userName = 'Betty';
user.userName;
// 'Betty'
```

A setter property has an associated function, like a getter does. This time the function takes one argument: the value being written to the property.

Like with getter functions, we don't need to explicitly call the setter function or manually pass in the argument. When the value of a property is modified, JavaScript calls the setter function with the new value as the argument. The setter function can do anything it wants with that argument.

```js
const user = {
  realName: 'Amir',
  set userName(newName) { this.realName = newName; },
};

user.userName = 'Betty';
user.realName;
// 'Betty'
```

```js
const user = {
  realName: 'Amir',
  set userName(newName) {
    this.realName = `New name: ${newName}`;
  },
};

user.userName = 'Betty';
user.realName;
// 'New name: Betty'
```

If we try to read the value of a setter, we'll get `undefined`. The object has no value at that key, even though there is a setter for that key.

```js
const user = {
  realName: 'Amir',
  set userName(newName) { this.realName = newName; }
};
user.userName;
// undefined
```

We can include both a getter and a setter in one object. You can think of this as defining two parts of a single property. One part handles reading from the property value and the other part handles writing to it.

```js
const user = {
  realName: 'Amir',
  get userName() { return this.realName; },
  set userName(newName) { this.realName = newName; },
};
user.userName = 'Betty';
[user.realName, user.userName];
// ['Betty', 'Betty']
```

We can use setters to track changes over time. Other code can modify our object properties as usual, using `user.userName = someNewName`. But behind the scenes, our `user.userName` setter can store a history of what happened.

Here's a code problem:

Add a `userName` setter to this object. It should push `userName` to the `names` list. Inside of the setter function, you can push a value onto `names` with `this.names.push(newName)`.

```js
const user = {
  names: ['Amir'],
  set userName(newName) {
    this.names.push(newName);
  }
};

user.userName = 'Betty';
user.names;

// ['Amir', 'Betty']
```

Here's a more complete example. Once again, we use an array to store the history of the user's names. The user's current `userName` is the last `userName` in that array. We also wrap the object in a `createUser` function so we don't have to hard-code the initial `userName`.

```js
function createUser(userName) {
  return {
    names: [userName],
    get userName() { return this.names[this.names.length 1]; },
    set userName(userName) { this.names.push(userName); }
  };
}

const user = createUser('Amir');
user.userName = 'Betty';
user.names;
// ['Amir', 'Betty']
```

One more small detail. The `get` and `set` keywords are required when creating getters and setters. If an object's keys are regular functions, then they won't be automatically called when we read or write to a property. We'll get the old JavaScript behavior from before getters and setters even existed.

```js
const user = {
  userName: function () { return 'Amir'; }
};
typeof user.userName;
// 'function'
```

---

## review

Add a `userName` getter to this object. It should return the value of the `name` variable.

```js
let name = 'Amir';

// start
const user = {
  get userName() { return name; },
};
// end

const userName1 = user.userName;
name = 'Betty';
const userName2 = user.userName;
[userName1, userName2];
// ['Amir', 'Betty']
```

Add a `userName` setter to this object. It should push `userName` to the `names` list. Inside of the setter function, you can push a value onto `names` with `this.names.push(newName)`.

```js
const user = {
  names: ['Amir'],
  set userName(newName) {
    this.names.push(newName);
  }
}

user.userName = 'Betty';
user.names;
// ['Amir', 'Betty']
```