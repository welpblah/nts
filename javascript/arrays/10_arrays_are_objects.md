## arrays are object

- Most programming languages have an array data type that's separate from other data types. JavaScript is unusual here: in JavaScript, arrays are a special type of object.

- We can see this by using `typeof`. When we have a regular object, `typeof someObject` is `'object'`.

```js
typeof {name: 'Amir'};
// 'object'
```

- But `typeof someArray` is also `'object'`!

```js
typeof [];
// 'object'
```

```js
typeof ['a', 'b', 'c'];
// 'object'
```

- This doesn't impact most common array operations. However, there are some surprising effects, and they can cause problems in real-world systems. Most notably, we can assign arbitrary properties to an array.

```js
const arr = ['a', 'b', 'c'];
arr['five'] = 5;
arr.five;
// 5
```

```js
const arr = ['a', 'b', 'c'];
arr.six = 6;
arr['six'];
// 6
```

- These new properties don't change the array elements. As a result, they don't affect its length.

```js
const arr = [1, 2];
arr['name'] = 'some numbers';
arr;
// [1, 2]
```

```js
const arr = ['a', 'b', 'c'];
arr['five'] = 5;
arr.length;
// 3
```

- In most cases, extra array properties are created by mistake. When other programmers read the code and see an array, they won't expect it to have these extra properties.

Here's an example. Imagine that we're building a service where one account can have multiple team members. The more team members a customer has, the more money we charge them. We need to keep track of both the members and the limit (how many team members are allowed in this account).

Somewhere in our application, we have a `teamMembers` array. We could store a `limit` number directly on the array.

```js
const teamMembers = [
  'ebony@example.com',
  'fang@example.com',
];
teamMembers.limit = 3;
teamMembers.length <= teamMembers.limit;
// true
```

This works, but it's not a good idea because it's surprising. Most arrays don't have metadata stored on object properties. When an array does have extra properties, other programmers won't expect it, which can lead them to make mistakes. For example, they might build a new array and pass it to a function, not realizing that the function expects the array to have this unusual `limit` property.

It's better to use an object in this case.

```js
const team = {
  members: [
    'ebony@example.com',
    'fang@example.com',
    'gabriel@example.com',
  ],
  memberLimit: 2,
};
team.members.length <= team.memberLimit;
// false
```

Now we have two nicely separate concepts: there's a list of team members, and there's a numerical limit on the number of members. Together, the members and the member limit form a team object. If a function needs the full team, we can pass that object; but if it only needs the members, we can pass only the array. Nothing about this is surprising or unusual.

A couple more details about extra properties on arrays. First, looping with `.forEach` will ignore extra array properties.

```js
const arr1 = ['a', 'b'];
const arr2 = [];
arr1.five = 5;
arr1.forEach(i => arr2.push(i));
arr2;
// ['a', 'b']
```

Second, extra properties will show up if we treat the array like an object. For example, we can use `Object.keys` to list the properties in an object. We'll get all of the array indexes (not the values), plus any extra properties that we added.

```js
const person = {name: 'Amir', likesOlives: true};
Object.keys(person);
// ['name', 'likesOlives']
```

```js
const arr1 = ['a', 'b'];
arr1.five = 5;
Object.keys(arr1);
// ['0', '1', 'five']
```

## review #1

```js
typeof [1, 2, 3];
// 'object'
```

## review #2

```js
const arr = [1, 2, 3, 4];
arr.someExtraProperty = 'some value';
arr.length;
// 4
```

## review #3

```js
typeof [true, false];
// 'object'
```

## review #4

```js
const arr = [true, false];
arr['two'] = true;
arr.length;
// 2
```

## review #5

```js
const arr = ['a', 'b', 'c'];
arr['five'] = 5;
arr.length;
// 3
```
