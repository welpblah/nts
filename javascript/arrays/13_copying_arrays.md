
- Copying data seems like it would be a simple, straightforward topic in programming. It's not: it's surprisingly complex and error-prone! This lesson will show some of those complexities and how they affect JavaScript arrays.

We'll begin with a seemingly-simple example: an array that contains arrays.

```js
const arrayOfArrays = [[1]];
arrayOfArrays.push([2]);
arrayOfArrays;
// [[1], [2]]
```

We can create other variables that reference this array, but creating those variables doesn't copy the array. There's still only one array, accessible and modifiable via multiple variables.

```js
const arrayOfArrays = [[1]];
const copy = arrayOfArrays;
// Modify via one variable. The change is then visible via either variable.
arrayOfArrays.push([2]);
copy;
// [[1], [2]]
arrayOfArrays;
// [[1], [2]]
```

We can make a copy of the array by calling the `.slice` method with no arguments. Modifying the copy doesn't modify the original, and modifying the original doesn't modify the copy.

```js
const arrayOfArrays = [[1]];
const copy = arrayOfArrays.slice();
arrayOfArrays.push([2]);
copy;
// [[1]]
```

This seems straightforward, but there's a subtle danger here. When we call `.slice()` with no arguments, it does something like this:

```js
function slice(array) {
  const newArray = [];
  array.forEach(element => {
    newArray.push(element);
  });
  return newArray;
}
```

This is a "shallow" copy. It creates a new array, but that array contains exactly the same values that were in the original array. If the array contains other arrays, those arrays are not copied! Even if we `.slice` the array, other arrays nested inside it are still shared.

```js
const arrayOfArrays = [[1]];
const copy = arrayOfArrays.slice();
arrayOfArrays[0][0] = 2;
copy;
// [[2]]
```

If this seems surprising, step through the `slice` function that we wrote above. Nothing in that function copies the array's elements! It only creates a new array containing exactly the same elements as the original array.

The same principle applies to objects. When we `.slice` an array of objects, we don't copy the objects. We only build a new array referencing the same objects in memory.

```js
const users = [{name: 'Amir'}];
const copy = users.slice();
users[0].name = 'Betty';
copy;
// [{name: 'Betty'}]
```

Here's what happened in the example above:

1. `copy` is a shallow copy of the `users` array. They're different arrays, but `copy[0]` and `users[0]` both hold references to the same object in memory.
2. We update an object property by changing `users[0].name`.
3. Because `users[0]` and `copy[0]` reference the same object, `copy[0].name` also changes.

How do we copy an array, and also ensure that changing deeply nested values in one array doesn't affect the copy? One solution is to put an entirely new object in the array, for example via `copy[0] = {name: 'Cindy'}`. The original object is still intact, and the original array still contains it. But the copied array now contains a totally different object.

```js
const users = [{name: 'Amir'}];
const copy = users.slice();
copy[0] = {name: 'Cindy'};
/* Now the `users` array is unchanged, but `copy` contains a new,
 * different user object. */
copy;
// [{name: 'Cindy'}]
users;
// [{name: 'Amir'}]
```

To summarize:

- When we `.slice` an array, we get a shallow copy. It's a new array, referencing all of the same elements in the original array.
- Two arrays can reference the same mutable value, like an object or another array. Changes to the object are visible no matter how we look at them: via the original array or the copied array. There's only one object, even though it's referenced by two arrays.
- If we store an entirely new object in one array, that doesn't affect any objects in the other array.

Here's a code problem:

Betty used to live at 12 Maple St. She moved to 14 Oak St, but her record was updated incorrectly. Now it looks like her original address is 14 Oak St too!

Update Betty's address without affecting her original address. You can do this by putting a new address object at `newRecord[0]`, rather than changing the existing object at that index.

```js
const address = {street: '12 Maple St'};
const oldRecord = [address];
const newRecord = oldRecord.slice();

// my answer
newRecord[0] = {street: '14 Oak St'};
// author's answer
newRecord[0] = {street: '14 Oak St'};

; /* This extra semicolon prevents a potential problem when your answer doesn't
   * end in a semicolon. */
[oldRecord[0].street, newRecord[0].street];

// ['12 Maple St', '14 Oak St']
```

If `.slice` is a shallow copy, what's a deep copy? A deep array copy creates a new array, but each element value is a copy of the original array's element value. For example, to deep copy an array of arrays, we have to call `.slice` on each individual array in the array.

```js
const array = [[1], [2]];
// Make a shallow copy with `.slice`.
const copy = array.slice();
copy[0][0] = 10;
array;
// [[10], [2]]
```

```js
const array = [[1], [2]];
// Make a deep copy by calling `.slice` on each sub-array.
const copy = array.map(subArray => subArray.slice());
copy[0][0] = 10;
array;
// [[1], [2]]
```

Unfortunately, using `.map` with `.slice` isn't a general solution. What if the array contains user objects? We need to copy those users.

What if the users contain arrays of address strings? As part of copying each user, we need to `.slice` the array of addresses.

What if the users contain arrays of address objects? As part of copying each user, we need to build a new array of addresses by copying each of the original addresses.

Now we can start to see the problems with deep copies! First, they can be slow. For complex objects, it's a lot of work to fully copy every nested object and array, all the way down.

Second, and somewhat paradoxically, deep copies can be confusing! They change the normal behavior of code, where updating an object in one place updates it everywhere. When copying arrays or objects, it's easy to forget that they were copied. Now some objects in the system don't follow the normal rules, which can lead to bugs.

When you do need to make a deep copy, you have some options. You can use the [lodash](https://lodash.com/docs#cloneDeep) `cloneDeep` method, or the [clone-deep](https://www.npmjs.com/package/clone-deep) npm library. JavaScript also has a `structuredClone` function for exactly this purpose.

```js
const array = [[1], [2]];
// Make a deep copy with structuredClone
const copy = structuredClone(array);
copy[0][0] = 10;
array;
// [[1], [2]]
```

However, note that deep copying is rarely a true necessity, and there's usually a cleaner solution. When this lesson was written, Execute Program's own source code included 14 calls to `.slice`, but only a single call to lodash's `cloneDeep`.

## review #1

```js
const users = [{name: 'Amir'}];
const copy = users.slice();
users[0].name = 'Betty';
copy;
// [{name: 'Betty'}]
```

```js
const users = [{name: 'Amir'}];
const copy = users.slice();
users[0].name = 'Betty';
copy;
// [{name: 'Betty'}]
```

## review #2

Here's a code problem:

Betty used to live at 12 Maple St. She moved to 14 Oak St, but her record was updated incorrectly. Now it looks like her original address is 14 Oak St too!

Update Betty's address without affecting her original address. You can do this by putting a new address object at `newRecord[0]`, rather than changing the existing object at that index.

```js
const address = {street: '12 Maple St'};
const oldRecord = [address];
const newRecord = oldRecord.slice();

// code here
newRecord[0] = {street: '14 Oak St'};

[oldRecord[0].street, newRecord[0].street];
// ['12 Maple St', '14 Oak St']
```