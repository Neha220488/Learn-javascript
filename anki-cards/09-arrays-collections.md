# Arrays and Collections in JavaScript

## Arrays Basics
Arrays are ordered collections of values that can be of any type.

**Q&A Format:**

**Q: What are JavaScript arrays and how are they different from arrays in other languages?**
A: JavaScript arrays are versatile data structures that can store elements of any type (numbers, strings, objects, other arrays). Unlike strongly-typed languages, they're zero-indexed, dynamically sized, and can contain mixed data types. They're actually objects with special behaviors for ordered collections.

**Q: What's the difference between Array constructor and Array.of()?**
A: 
- `new Array(5)` creates an array with 5 empty slots (special case for single number)
- `new Array(1, 2, 3)` creates `[1, 2, 3]` (multiple arguments)
- `Array.of(5)` always creates `[5]` regardless of arguments
- `Array.of()` fixes the confusing single-number behavior of the constructor

**Q: What are the main methods for adding and removing array elements?**
A: 
- **Add to end**: `push(element)` - returns new length
- **Add to beginning**: `unshift(element)` - returns new length  
- **Remove from end**: `pop()` - returns removed element
- **Remove from beginning**: `shift()` - returns removed element
These methods mutate the original array.

```javascript
// Creating arrays
const numbers = [1, 2, 3, 4, 5];
const mixed = [1, 'two', true, null, { name: 'John' }, [6, 7, 8]];
const empty = [];

// Array constructor
const numberArray = new Array(1, 2, 3, 4);
console.log(numberArray);  // [1, 2, 3, 4]

// Special case: single number creates array of that length
const zeros = new Array(5);  // Creates array with 5 empty slots
console.log(zeros);  // [ <5 empty items> ]

// Array.of() - fixes the special case behavior
const fiveArray = Array.of(5);
console.log(fiveArray);  // [5]

// Accessing elements
const fruits = ['apple', 'banana', 'cherry'];
console.log(fruits[0]);  // 'apple'
console.log(fruits[1]);  // 'banana'
console.log(fruits[2]);  // 'cherry'
console.log(fruits[3]);  // undefined (no error)

// Modifying elements
fruits[1] = 'blueberry';
console.log(fruits);  // ['apple', 'blueberry', 'cherry']

// Array length
console.log(fruits.length);  // 3

// Adding elements
fruits.push('date');  // Add to the end
console.log(fruits);  // ['apple', 'blueberry', 'cherry', 'date']

fruits.unshift('avocado');  // Add to the beginning
console.log(fruits);  // ['avocado', 'apple', 'blueberry', 'cherry', 'date']

// Removing elements
const lastFruit = fruits.pop();  // Remove from the end
console.log(lastFruit);  // 'date'
console.log(fruits);  // ['avocado', 'apple', 'blueberry', 'cherry']

const firstFruit = fruits.shift();  // Remove from the beginning
console.log(firstFruit);  // 'avocado'
console.log(fruits);  // ['apple', 'blueberry', 'cherry']

// Finding elements
const colors = ['red', 'green', 'blue', 'yellow', 'green'];
console.log(colors.indexOf('green'));  // 1 (first occurrence)
console.log(colors.lastIndexOf('green'));  // 4 (last occurrence)
console.log(colors.indexOf('purple'));  // -1 (not found)

// Checking if an element exists
console.log(colors.includes('blue'));  // true
console.log(colors.includes('purple'));  // false

// Removing elements by index
const removed = colors.splice(1, 2);  // Remove 2 elements starting at index 1
console.log(removed);  // ['green', 'blue']
console.log(colors);  // ['red', 'yellow', 'green']

// Adding elements at a specific position
colors.splice(1, 0, 'purple', 'orange');  // Insert at index 1, remove 0 elements
console.log(colors);  // ['red', 'purple', 'orange', 'yellow', 'green']
```

## Array Methods for Iteration
JavaScript provides various methods for iterating over arrays, allowing you to transform, filter, and aggregate data efficiently.

**Q&A Format:**

**Q: What are the key differences between forEach, map, and filter array methods?**
A: 
- **forEach**: Executes a function for each element, returns `undefined`, used for side effects
- **map**: Transforms each element, returns new array of same length
- **filter**: Returns new array with elements that pass a test condition
- Only `map` and `filter` return new arrays; `forEach` is for iteration only

**Q: How do find() and findIndex() differ from includes() and indexOf()?**
A: 
- **find()**: Returns first element that passes test function
- **findIndex()**: Returns index of first element that passes test function  
- **includes()**: Returns boolean if value exists (uses strict equality)
- **indexOf()**: Returns index of first occurrence (uses strict equality)
Find methods work with complex conditions; includes/indexOf work with exact matches

**Q: What's the difference between some() and every() methods?**
A: 
- **some()**: Returns `true` if at least one element passes the test (logical OR)
- **every()**: Returns `true` only if all elements pass the test (logical AND)
- Both short-circuit (stop early when result is determined)
- Useful for validation and condition checking

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach - execute a function for each element
numbers.forEach((number, index, array) => {
    console.log(`Element at index ${index} is ${number}`);
});

// map - create a new array by transforming each element
const doubled = numbers.map(num => num * 2);
console.log(doubled);  // [2, 4, 6, 8, 10]

// filter - create a new array with elements that pass a test
const evens = numbers.filter(num => num % 2 === 0);
console.log(evens);  // [2, 4]

// find - return the first element that passes a test
const firstEven = numbers.find(num => num % 2 === 0);
console.log(firstEven);  // 2

// findIndex - return the index of the first element that passes a test
const firstEvenIndex = numbers.findIndex(num => num % 2 === 0);
console.log(firstEvenIndex);  // 1

// some - check if at least one element passes a test
const hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven);  // true

// every - check if all elements pass a test
const allEven = numbers.every(num => num % 2 === 0);
console.log(allEven);  // false

// reduce - reduce the array to a single value
const sum = numbers.reduce((total, num) => total + num, 0);
console.log(sum);  // 15

// reduceRight - like reduce, but starts from the right
const numbersStr = ['1', '2', '3', '4', '5'];
const combined = numbersStr.reduceRight((result, num) => result + num, '');
console.log(combined);  // '54321'

// Chaining array methods
const result = numbers
    .filter(num => num % 2 === 0)  // Keep even numbers
    .map(num => num * 10)  // Multiply by 10
    .reduce((sum, num) => sum + num, 0);  // Sum them up

console.log(result);  // 60 (2*10 + 4*10)

// Real-world example: Processing user data
const users = [
    { id: 1, name: 'Alice', age: 25, active: true },
    { id: 2, name: 'Bob', age: 30, active: false },
    { id: 3, name: 'Charlie', age: 35, active: true },
    { id: 4, name: 'Dave', age: 40, active: true }
];

// Get names of active users over 30
const activeNames = users
    .filter(user => user.active && user.age > 30)
    .map(user => user.name);

console.log(activeNames);  // ['Charlie', 'Dave']

// Calculate average age of active users
const activeUserCount = users.filter(user => user.active).length;
const totalActiveAge = users
    .filter(user => user.active)
    .reduce((sum, user) => sum + user.age, 0);
const averageActiveAge = totalActiveAge / activeUserCount;

console.log(averageActiveAge);  // 33.33...
```

## Array Methods for Manipulation
JavaScript provides various methods for manipulating arrays, allowing you to sort, combine, extract, and transform array data.

**Q&A Format:**

**Q: Which array methods mutate the original array vs. return a new array?**
A: 
**Mutating methods** (modify original): `sort()`, `reverse()`, `push()`, `pop()`, `shift()`, `unshift()`, `splice()`
**Non-mutating methods** (return new): `concat()`, `slice()`, `map()`, `filter()`, `find()`, spread operator `[...arr]`
Understanding this distinction prevents unexpected side effects in your code.

**Q: Why does sorting numbers with sort() give unexpected results, and how do you fix it?**
A: `sort()` converts elements to strings and sorts lexicographically by default. Numbers like `[1, 200, 40, 5]` become `["1", "200", "40", "5"]` and sort as `[1, 200, 40, 5]`. 
**Fix**: Use compare function:
- Ascending: `sort((a, b) => a - b)`
- Descending: `sort((a, b) => b - a)`

**Q: What's the difference between concat() and the spread operator for combining arrays?**
A: Both create new arrays without modifying originals:
- `arr1.concat(arr2)` - method syntax, works in older browsers
- `[...arr1, ...arr2]` - ES6 spread syntax, more flexible positioning
- Performance is similar, spread operator allows inserting elements anywhere: `[0, ...arr1, 'middle', ...arr2, 'end']`

```javascript
// sort - sorts the elements of an array in place
const fruits = ['banana', 'cherry', 'apple', 'date'];
fruits.sort();
console.log(fruits);  // ['apple', 'banana', 'cherry', 'date']

// Sorting numbers requires a compare function
const numbers = [40, 1, 5, 200];
numbers.sort();
console.log(numbers);  // [1, 200, 40, 5] (lexicographic order!)

// Correct numeric sort
numbers.sort((a, b) => a - b);  // Ascending
console.log(numbers);  // [1, 5, 40, 200]

numbers.sort((a, b) => b - a);  // Descending
console.log(numbers);  // [200, 40, 5, 1]

// Custom sort for objects
const people = [
    { name: 'Alice', age: 25 },
    { name: 'Bob', age: 20 },
    { name: 'Charlie', age: 30 }
];

// Sort by age
people.sort((a, b) => a.age - b.age);
console.log(people);  // [{name: 'Bob', age: 20}, {name: 'Alice', age: 25}, {name: 'Charlie', age: 30}]

// Sort by name
people.sort((a, b) => a.name.localeCompare(b.name));
console.log(people);  // [{name: 'Alice', age: 25}, {name: 'Bob', age: 20}, {name: 'Charlie', age: 30}]

// reverse - reverses the order of elements
const letters = ['a', 'b', 'c', 'd'];
letters.reverse();
console.log(letters);  // ['d', 'c', 'b', 'a']

// concat - combine arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = arr1.concat(arr2);
console.log(combined);  // [1, 2, 3, 4, 5, 6]
console.log(arr1);  // [1, 2, 3] (unchanged)

// Using spread operator to combine arrays
const combinedSpread = [...arr1, ...arr2];
console.log(combinedSpread);  // [1, 2, 3, 4, 5, 6]

// slice - extract a section of an array
const colors = ['red', 'green', 'blue', 'yellow', 'purple'];
const sliced = colors.slice(1, 4);
console.log(sliced);  // ['green', 'blue', 'yellow']
console.log(colors);  // ['red', 'green', 'blue', 'yellow', 'purple'] (unchanged)

// Negative indices count from the end
const last = colors.slice(-2);
console.log(last);  // ['yellow', 'purple']

// Clone an array with slice
const clone = colors.slice();
console.log(clone);  // ['red', 'green', 'blue', 'yellow', 'purple']

// join - convert array to string
const parts = ['Hello', 'world', 'of', 'JavaScript'];
const sentence = parts.join(' ');
console.log(sentence);  // 'Hello world of JavaScript'

// split - convert string to array
const csv = 'apple,banana,cherry,date';
const fruitArray = csv.split(',');
console.log(fruitArray);  // ['apple', 'banana', 'cherry', 'date']

// Removing duplicates with Set
const duplicates = [1, 2, 2, 3, 4, 4, 5];
const unique = [...new Set(duplicates)];
console.log(unique);  // [1, 2, 3, 4, 5]

// flat - flatten nested arrays
const nested = [1, [2, 3], [4, [5, 6]]];
console.log(nested.flat());  // [1, 2, 3, 4, [5, 6]]
console.log(nested.flat(2));  // [1, 2, 3, 4, 5, 6]

// flatMap - map and then flatten
const sentences = ['Hello world', 'JavaScript is fun'];
const words = sentences.flatMap(sentence => sentence.split(' '));
console.log(words);  // ['Hello', 'world', 'JavaScript', 'is', 'fun']
```

## Array-Like Objects and Conversion
JavaScript has array-like objects that resemble arrays but don't have array methods. Converting between arrays and other collection types is common.

**Q&A Format:**

**Q: What are array-like objects and why can't you use array methods on them?**
A: Array-like objects have numbered indices and a `length` property but aren't actual arrays. Examples include `arguments` object, DOM NodeLists, and strings. They lack array methods because they don't inherit from `Array.prototype`. You can access elements by index, but methods like `forEach()`, `map()`, etc. aren't available.

**Q: What are the main ways to convert array-like objects to real arrays?**
A: 
- **Array.from()**: `Array.from(arrayLike)` - works with any iterable/array-like object
- **Spread operator**: `[...arrayLike]` - works with iterables only
- **Array.prototype.slice.call()**: `Array.prototype.slice.call(arrayLike)` - older ES5 method
- Array.from() is most versatile and readable for modern JavaScript

**Q: How do you handle function arguments in modern JavaScript?**
A: Modern approaches:
- **Rest parameters**: `function sum(...args) { }` - gives real array
- **Array.from()**: `Array.from(arguments)` - converts arguments object
- **Spread**: `[...arguments]` - converts to array
Rest parameters are preferred for new code as they provide a real array from the start.

```javascript
// Array-like object example (function arguments)
function sumArgs() {
    // arguments is an array-like object
    console.log(arguments);  // { 0: 1, 1: 2, 2: 3, length: 3 }
    
    // It doesn't have array methods
    // arguments.forEach(arg => console.log(arg));  // Error: arguments.forEach is not a function
    
    // Convert to a real array (ES6 way)
    const args = Array.from(arguments);
    args.forEach(arg => console.log(arg));
    
    // Convert using spread operator (ES6)
    const argsSpread = [...arguments];
    return argsSpread.reduce((sum, num) => sum + num, 0);
}

console.log(sumArgs(1, 2, 3));  // 6

// DOM NodeList example
// const paragraphs = document.querySelectorAll('p');  // NodeList
// const paragraphArray = Array.from(paragraphs);
// paragraphArray.forEach(p => p.style.color = 'blue');

// Converting array-like objects in older JavaScript
function oldSchoolConversion() {
    // Using Array.prototype.slice.call
    const args = Array.prototype.slice.call(arguments);
    return args.join(', ');
}

console.log(oldSchoolConversion('a', 'b', 'c'));  // 'a, b, c'

// Creating arrays from iterables
const set = new Set([1, 2, 3, 4, 3, 2, 1]);
const arrFromSet = Array.from(set);
console.log(arrFromSet);  // [1, 2, 3, 4]

// Array.from with mapping function
const numbers = Array.from({ length: 5 }, (_, index) => index * 2);
console.log(numbers);  // [0, 2, 4, 6, 8]

// String to array
const str = 'Hello';
const chars = Array.from(str);
console.log(chars);  // ['H', 'e', 'l', 'l', 'o']

// Convert array to object
const fruits = ['apple', 'banana', 'cherry'];
const fruitsObj = { ...fruits };
console.log(fruitsObj);  // { 0: 'apple', 1: 'banana', 2: 'cherry' }

// Better way to create an object from an array
const users = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' },
    { id: 3, name: 'Charlie' }
];

const userMap = users.reduce((obj, user) => {
    obj[user.id] = user;
    return obj;
}, {});

console.log(userMap);  
// { 
//   '1': { id: 1, name: 'Alice' }, 
//   '2': { id: 2, name: 'Bob' }, 
//   '3': { id: 3, name: 'Charlie' } 
// }
```

## Array Iteration and Loops
JavaScript offers multiple ways to iterate over arrays, each with different use cases and syntax.

**Q&A Format:**

**Q: What are the main types of loops available for iterating over arrays in JavaScript?**
A: JavaScript provides several iteration methods:
- **for loop**: Traditional, gives full control and can break/continue
- **for...of**: ES6, direct access to values, cleaner syntax
- **forEach()**: Functional style, no return value, can't break
- **for...in**: Iterates over indices (not recommended for arrays)

**Q: When should you use each type of array iteration method?**
A: 
- **for loop**: When you need indices, early termination (break/continue), or performance is critical
- **for...of**: When you only need values and want clean, readable code
- **forEach()**: For functional programming style, when you don't need early termination
- **for...in**: Avoid for arrays (designed for objects)

**Q: What are the key differences between for...of and forEach()?**
A:
- **for...of**: Can use break/continue, synchronous, returns undefined
- **forEach()**: Cannot break early, provides index as second parameter, method of array prototype
- **Performance**: for...of is slightly faster, forEach is more functional

```javascript
const colors = ['red', 'green', 'blue'];

// 1. for loop
console.log('for loop:');
for (let i = 0; i < colors.length; i++) {
    console.log(colors[i]);
}

// 2. for...of loop (ES6)
console.log('for...of loop:');
for (const color of colors) {
    console.log(color);
}

// 3. forEach method
console.log('forEach:');
colors.forEach(color => {
    console.log(color);
});

// 4. forEach with index
console.log('forEach with index:');
colors.forEach((color, index) => {
    console.log(`${index}: ${color}`);
});

// 5. for...in loop (not recommended for arrays but works)
console.log('for...in loop:');
for (const index in colors) {
    console.log(colors[index]);
}

// When to use each type:

// Use for loop when you need to:
// - Break out of the loop early
// - Skip iterations (continue)
// - Need more control over iteration behavior
for (let i = 0; i < colors.length; i++) {
    if (colors[i] === 'green') break;  // Stop at green
    console.log(colors[i]);  // Only prints 'red'
}

// Use for...of when you:
// - Need a simple iteration
// - Want to use break/continue
// - Don't need the index
for (const color of colors) {
    if (color === 'green') continue;  // Skip green
    console.log(color);  // Prints 'red' and 'blue'
}

// Use forEach when you:
// - Prefer functional programming style
// - Don't need to break the loop
// - Need the index or the original array
colors.forEach((color, index, array) => {
    console.log(`${color} is at position ${index} in [${array}]`);
});

// Performance considerations:
// - for loop is traditionally fastest but micro-optimization rarely matters
// - for...of and forEach are more readable in most cases
// - Choose based on readability and specific requirements

// Special cases:
// 1. Sparse arrays
const sparse = [1, , 3, 4];
console.log('Sparse array:');
sparse.forEach(item => console.log(item));  // Skips empty item
for (let i = 0; i < sparse.length; i++) {
    console.log(sparse[i]);  // Prints 1, undefined, 3, 4
}

// 2. Modifying the array during iteration
const numbers = [1, 2, 3, 4, 5];
console.log('Original array:', numbers);

// forEach does not see new elements added during iteration
numbers.forEach(num => {
    console.log(num);
    if (num === 2) numbers.push(100);  // Added element won't be iterated
});
console.log('After forEach:', numbers);  // [1, 2, 3, 4, 5, 100]

// Reset array
numbers.length = 5;
console.log('Reset array:', numbers);

// for loop will see new elements if we use the array's live length
for (let i = 0; i < numbers.length; i++) {
    console.log(numbers[i]);
    if (numbers[i] === 2) numbers.push(200);  // This can cause an infinite loop!
    if (i > 10) break;  // Safety exit
}
console.log('After for loop:', numbers);  // [1, 2, 3, 4, 5, 200, ...]
```

## Maps
Map is a collection of keyed data items, similar to an Object, but with several important differences.

**Q&A Format:**

**Q: What is a Map in JavaScript and how does it differ from an Object?**
A: A Map is a collection that holds key-value pairs and remembers insertion order. Key differences from Objects:
- **Key types**: Map can use any type as keys (objects, primitives); Objects only use strings/symbols
- **Size**: Map has `.size` property; Objects need `Object.keys().length`
- **Iteration**: Map is directly iterable; Objects need `Object.keys()` or `Object.entries()`
- **Performance**: Map is optimized for frequent additions/removals

**Q: What are the main methods for working with Maps?**
A: Core Map methods:
- **set(key, value)**: Adds/updates entry, returns the Map (chainable)
- **get(key)**: Retrieves value, returns undefined if not found
- **has(key)**: Checks if key exists, returns boolean
- **delete(key)**: Removes entry, returns boolean success
- **clear()**: Removes all entries
- **size**: Property (not method) returning number of entries

**Q: What are the advantages of using Map over Object for certain use cases?**
A: Use Map when:
- Keys are not strings (objects, numbers, etc.)
- Need to know the collection size frequently
- Need to iterate in insertion order
- Frequently adding/removing key-value pairs
- Need a clean, prototype-free collection

```javascript
// Creating a Map
const userMap = new Map();

// Adding entries
userMap.set('alice', { name: 'Alice', role: 'Admin' });
userMap.set('bob', { name: 'Bob', role: 'User' });
userMap.set('charlie', { name: 'Charlie', role: 'User' });

console.log(userMap.size);  // 3

// Creating a Map from an array of key-value pairs
const colorMap = new Map([
    ['red', '#FF0000'],
    ['green', '#00FF00'],
    ['blue', '#0000FF']
]);

// Getting values
console.log(userMap.get('alice'));  // { name: 'Alice', role: 'Admin' }
console.log(colorMap.get('red'));  // '#FF0000'
console.log(userMap.get('dave'));  // undefined (no error)

// Checking if a key exists
console.log(userMap.has('bob'));  // true
console.log(userMap.has('dave'));  // false

// Deleting entries
userMap.delete('bob');
console.log(userMap.size);  // 2
console.log(userMap.has('bob'));  // false

// Clearing all entries
colorMap.clear();
console.log(colorMap.size);  // 0

// Object vs Map as keys
const obj1 = { id: 1 };
const obj2 = { id: 2 };

const objectAsKey = new Map();
objectAsKey.set(obj1, 'Object 1 data');
objectAsKey.set(obj2, 'Object 2 data');

console.log(objectAsKey.get(obj1));  // 'Object 1 data'
console.log(objectAsKey.size);  // 2

// Iterating over a Map
userMap.set('bob', { name: 'Bob', role: 'User' });  // Add bob back

// 1. Iterating over keys
console.log('Map keys:');
for (const key of userMap.keys()) {
    console.log(key);  // 'alice', 'charlie', 'bob'
}

// 2. Iterating over values
console.log('Map values:');
for (const value of userMap.values()) {
    console.log(value.name);  // 'Alice', 'Charlie', 'Bob'
}

// 3. Iterating over entries
console.log('Map entries:');
for (const [key, value] of userMap.entries()) {
    console.log(`${key}: ${value.name}`);  // 'alice: Alice', etc.
}

// Shorthand for entries
console.log('Map entries (shorthand):');
for (const [key, value] of userMap) {
    console.log(`${key}: ${value.name}`);  // Same result
}

// Using forEach
userMap.forEach((value, key) => {
    console.log(`${key}: ${value.name}`);
});

// Converting Map to Array
const keysArray = [...userMap.keys()];
const valuesArray = [...userMap.values()];
const entriesArray = [...userMap.entries()];

console.log(keysArray);  // ['alice', 'charlie', 'bob']
console.log(valuesArray);  // [{ name: 'Alice', ... }, ...]
console.log(entriesArray);  // [['alice', { ... }], ...]

// Converting Object to Map
const userObj = {
    dave: { name: 'Dave', role: 'User' },
    eve: { name: 'Eve', role: 'Admin' }
};

const userMap2 = new Map(Object.entries(userObj));
console.log(userMap2.get('dave').name);  // 'Dave'

// Converting Map to Object (only works properly with string keys)
const mapAsObject = Object.fromEntries(userMap.entries());
console.log(mapAsObject.alice.name);  // 'Alice'

// Merging Maps
const combinedMap = new Map([...userMap, ...userMap2]);
console.log(combinedMap.size);  // 5
console.log([...combinedMap.keys()]);  // ['alice', 'charlie', 'bob', 'dave', 'eve']
```

## Sets
## Sets
Set is a collection of unique values, where each value may occur only once.

**Q&A Format:**

**Q: What is a Set in JavaScript and what problem does it solve?**
A: A Set is a collection of unique values where each value occurs only once. It solves the problem of:
- Removing duplicates from arrays
- Maintaining unique collections without manual checking
- Fast membership testing (has() method)
- Automatic deduplication when adding values

**Q: How does Set handle object uniqueness compared to primitive values?**
A: Set determines uniqueness by reference, not content:
- **Primitives**: `5` and `5` are considered the same (same value)
- **Objects**: `{id: 1}` and `{id: 1}` are different (different references)
- **Same object**: Adding the same object reference multiple times only stores it once

**Q: What are the main methods and properties of Set?**
A: Core Set operations:
- **add(value)**: Adds value, returns the Set (chainable)
- **has(value)**: Checks if value exists, returns boolean
- **delete(value)**: Removes value, returns boolean success
- **clear()**: Removes all values
- **size**: Property returning number of unique values
- **Iteration**: for...of, forEach, or spread operator

**Q: How can you convert between Set and Array?**
A: Easy conversion between Set and Array:
- **Array to Set**: `new Set([1, 2, 2, 3])` → `Set {1, 2, 3}`
- **Set to Array**: `[...mySet]` or `Array.from(mySet)`
- **Remove duplicates**: `[...new Set(array)]`

```javascript
// Creating a Set
const uniqueNumbers = new Set([1, 2, 3, 4, 5]);
console.log(uniqueNumbers.size);  // 5

// Adding values
uniqueNumbers.add(6);
uniqueNumbers.add(1);  // Already exists, will be ignored
console.log(uniqueNumbers.size);  // 6

// Creating a Set from an iterable
const uniqueFromArray = new Set([1, 2, 2, 3, 4, 4, 5]);
console.log(uniqueFromArray.size);  // 5
console.log([...uniqueFromArray]);  // [1, 2, 3, 4, 5]

// Checking if a value exists
console.log(uniqueNumbers.has(3));  // true
console.log(uniqueNumbers.has(10));  // false

// Deleting values
uniqueNumbers.delete(4);
console.log(uniqueNumbers.has(4));  // false
console.log(uniqueNumbers.size);  // 5

// Clearing all values
uniqueNumbers.clear();
console.log(uniqueNumbers.size);  // 0

// Working with objects in Sets
const objSet = new Set();
const obj1 = { id: 1 };
const obj2 = { id: 2 };
const obj3 = { id: 1 };  // Different object with same content as obj1

objSet.add(obj1);
objSet.add(obj2);
objSet.add(obj3);  // Different object reference, will be added
objSet.add(obj1);  // Same object reference, will be ignored

console.log(objSet.size);  // 3

// Iterating over a Set
const colors = new Set(['red', 'green', 'blue', 'yellow']);

// 1. Using for...of
console.log('Set values:');
for (const color of colors) {
    console.log(color);
}

// 2. Using forEach
console.log('Set forEach:');
colors.forEach(color => {
    console.log(color);
});

// 3. Using values() method
console.log('Set values() method:');
for (const color of colors.values()) {
    console.log(color);
}

// Note: keys() is the same as values() for compatibility with Map
console.log('Set keys() method:');
for (const color of colors.keys()) {
    console.log(color);  // Same output as values()
}

// entries() returns [value, value] pairs for Map compatibility
console.log('Set entries() method:');
for (const [key, value] of colors.entries()) {
    console.log(key, value);  // key and value are the same
}

// Converting Set to Array
const colorsArray = [...colors];
console.log(colorsArray);  // ['red', 'green', 'blue', 'yellow']

// Set operations
const set1 = new Set([1, 2, 3, 4, 5]);
const set2 = new Set([3, 4, 5, 6, 7]);

// Union
const union = new Set([...set1, ...set2]);
console.log([...union]);  // [1, 2, 3, 4, 5, 6, 7]

// Intersection
const intersection = new Set(
    [...set1].filter(num => set2.has(num))
);
console.log([...intersection]);  // [3, 4, 5]

// Difference
const difference = new Set(
    [...set1].filter(num => !set2.has(num))
);
console.log([...difference]);  // [1, 2]

// Symmetric difference
const symmetricDifference = new Set(
    [...set1].filter(num => !set2.has(num))
    .concat([...set2].filter(num => !set1.has(num)))
);
console.log([...symmetricDifference]);  // [1, 2, 6, 7]

// Practical example: Filtering unique values from an array
const numbers = [1, 2, 2, 3, 4, 4, 5, 5, 5];
const uniqueValues = [...new Set(numbers)];
console.log(uniqueValues);  // [1, 2, 3, 4, 5]

// Counting unique words in a text
const text = "The quick brown fox jumps over the lazy dog. The dog was not amused.";
const words = text.toLowerCase().match(/\b\w+\b/g);
const uniqueWords = new Set(words);
console.log(`There are ${uniqueWords.size} unique words in the text.`);  // 11
```

## WeakMap and WeakSet
WeakMap and WeakSet are collections that don't prevent their keys/values from being garbage collected.

**Q&A Format:**

**Q: What makes WeakMap and WeakSet "weak" compared to Map and Set?**
A: They're "weak" because they don't create strong references to their keys/values:
- **Weak references**: If an object is only referenced by WeakMap/WeakSet, it can be garbage collected
- **Memory management**: Helps prevent memory leaks by allowing automatic cleanup
- **No enumeration**: Cannot iterate over entries (no size, no iteration methods)

**Q: What are the key restrictions of WeakMap compared to Map?**
A: WeakMap limitations:
- **Keys must be objects**: Cannot use primitive values (strings, numbers) as keys
- **Not iterable**: No for...of, forEach, or other iteration methods
- **No size property**: Cannot determine number of entries
- **Limited methods**: Only get(), set(), has(), delete()

**Q: When should you use WeakMap instead of Map?**
A: Use WeakMap when:
- Storing metadata for objects that might be removed
- Associating data with DOM elements that may be deleted
- Preventing memory leaks in long-running applications
- Need automatic cleanup when objects are no longer used

**Q: What's a practical use case for WeakMap?**
A: **DOM element metadata**: Store extra data for DOM elements without preventing them from being garbage collected when removed from the DOM. The associated data automatically disappears when the element is cleaned up.

```javascript
// WeakMap example
// Create a WeakMap to store extra data for DOM elements
// const elementData = new WeakMap();

// function addData(element, key, value) {
//     // Get or create data object for this element
//     if (!elementData.has(element)) {
//         elementData.set(element, {});
//     }
    
//     // Add the data
//     const data = elementData.get(element);
//     data[key] = value;
// }

// function getData(element, key) {
//     const data = elementData.get(element);
//     return data ? data[key] : undefined;
// }

// // Usage
// const div = document.createElement('div');
// addData(div, 'color', 'red');
// console.log(getData(div, 'color'));  // 'red'

// // If div is removed and no longer referenced, the data will be garbage collected

// Basic WeakMap operations
let obj1 = { name: 'Object 1' };
let obj2 = { name: 'Object 2' };

const weakMap = new WeakMap();
weakMap.set(obj1, 'Data for object 1');
weakMap.set(obj2, 'Data for object 2');

console.log(weakMap.get(obj1));  // 'Data for object 1'
console.log(weakMap.has(obj2));  // true

obj1 = null;  // Object becomes eligible for garbage collection
// The entry in weakMap will also be garbage collected eventually

// WeakMap cannot use primitive values as keys
// weakMap.set('string', 'value');  // TypeError: Invalid value used as weak map key

// WeakMap is not iterable
// for (const [key, value] of weakMap) {}  // TypeError: weakMap is not iterable

// WeakMap doesn't have size property
// console.log(weakMap.size);  // undefined

// WeakSet example
let obj3 = { name: 'Object 3' };
let obj4 = { name: 'Object 4' };

const weakSet = new WeakSet();
weakSet.add(obj3);
weakSet.add(obj4);

console.log(weakSet.has(obj3));  // true

obj3 = null;  // Object becomes eligible for garbage collection
// The entry in weakSet will also be garbage collected eventually

// WeakSet use cases: marking objects as "visited" or "processed"
// function process(objects) {
//     const processed = new WeakSet();
    
//     function doProcess(obj) {
//         if (processed.has(obj)) {
//             console.log('Already processed:', obj);
//             return;
//         }
        
//         // Process the object
//         console.log('Processing:', obj);
//         processed.add(obj);
        
//         // Process nested objects
//         if (obj.children) {
//             obj.children.forEach(doProcess);
//         }
//     }
    
//     objects.forEach(doProcess);
// }

// Memory leaks prevention with WeakMap
// Without WeakMap (potential memory leak)
function createCache() {
    const cache = {};
    
    return {
        add: (obj, value) => {
            // Using object as key by converting to string - BAD APPROACH
            cache[obj] = value;
        },
        get: (obj) => cache[obj]
    };
}

// With WeakMap (no memory leak)
function createWeakCache() {
    const cache = new WeakMap();
    
    return {
        add: (obj, value) => {
            cache.set(obj, value);
        },
        get: (obj) => cache.get(obj)
    };
}

// Limitations:
// 1. No way to get a list of all keys/values
// 2. No way to iterate over the entries
// 3. No way to clear all entries at once
// 4. No way to get the size
```

## TypedArrays
TypedArrays are array-like objects that provide a mechanism for accessing raw binary data, crucial for handling binary files, network protocols, and WebGL.

**Q&A Format:**

**Q: What are Typed Arrays and why were they introduced to JavaScript?**
A: Typed Arrays are array-like objects for handling binary data efficiently. They were introduced for:
- **WebGL**: 3D graphics requiring fast numeric computations
- **File processing**: Reading binary files (images, audio, etc.)
- **Network protocols**: Handling binary network data
- **Performance**: Much faster than regular arrays for numeric operations

**Q: What are the main differences between Typed Arrays and regular Arrays?**
A: Key differences:
- **Type constraint**: Can only contain specific numeric types (Int32, Float64, etc.)
- **Fixed size**: Length cannot be changed after creation
- **Memory efficient**: Direct mapping to binary data
- **Performance**: Faster for numeric operations
- **ArrayBuffer**: Backed by raw binary data buffer

**Q: What are the common Typed Array types and their ranges?**
A: Main Typed Array types:
- **Int8Array**: -128 to 127 (1 byte signed)
- **Uint8Array**: 0 to 255 (1 byte unsigned)
- **Int16Array**: -32,768 to 32,767 (2 bytes signed)
- **Uint16Array**: 0 to 65,535 (2 bytes unsigned)
- **Int32Array**: ±2.1 billion (4 bytes signed)
- **Float32Array**: 32-bit floating point
- **Float64Array**: 64-bit floating point (double precision)

**Q: What are the different ways to create Typed Arrays?**
A: Four creation methods:
- **From length**: `new Int32Array(5)` - creates array with 5 zeros
- **From array**: `new Float32Array([1.1, 2.2, 3.3])` - converts values
- **From ArrayBuffer**: `new Int32Array(buffer)` - views existing binary data
- **From another typed array**: `new Int16Array(int32Array)` - converts types

```javascript
// Available TypedArray views
// Int8Array, Uint8Array, Uint8ClampedArray,
// Int16Array, Uint16Array,
// Int32Array, Uint32Array,
// Float32Array, Float64Array

// Creating TypedArrays

// 1. From length
const int32 = new Int32Array(5);  // Array of 5 32-bit integers (initialized to 0)
console.log(int32);  // Int32Array(5) [0, 0, 0, 0, 0]

// 2. From another array or typed array
const float32FromArray = new Float32Array([1.1, 2.2, 3.3, 4.4, 5.5]);
console.log(float32FromArray);  // Float32Array(5) [1.100000023841858, 2.200000047683716, ...]

// 3. From ArrayBuffer
const buffer = new ArrayBuffer(16);  // 16 bytes
const int32View = new Int32Array(buffer);  // 4 elements (16 bytes / 4 bytes per Int32)
int32View[0] = 42;
console.log(int32View);  // Int32Array(4) [42, 0, 0, 0]

// 4. From another typed array
const int16View = new Int16Array(int32View);  // Converts Int32 to Int16
console.log(int16View);  // Int16Array(4) [42, 0, 0, 0]

// Basic operations

// Getting length
console.log(int32View.length);  // 4
console.log(int32View.byteLength);  // 16

// Accessing elements
int32View[1] = 123;
console.log(int32View[1]);  // 123

// Iterating
for (const value of int32View) {
    console.log(value);  // 42, 123, 0, 0
}

// Using array methods
const uint8 = new Uint8Array([1, 2, 3, 4, 5]);
const mapped = uint8.map(x => x * 2);
console.log(mapped);  // Uint8Array(5) [2, 4, 6, 8, 10]

const filtered = uint8.filter(x => x > 2);
console.log(filtered);  // Uint8Array(3) [3, 4, 5]

// TypedArray-specific methods
const uint8Copy = uint8.subarray(1, 4);
console.log(uint8Copy);  // Uint8Array(3) [2, 3, 4]

// Set values from another array
const target = new Uint8Array(5);
target.set([10, 20, 30], 1);  // Start at index 1
console.log(target);  // Uint8Array(5) [0, 10, 20, 30, 0]

// Shared buffers between different views
const sharedBuffer = new ArrayBuffer(8);
const uint8View = new Uint8Array(sharedBuffer);
const uint16View = new Uint16Array(sharedBuffer);

// Setting a 16-bit value
uint16View[0] = 0x1234;
console.log(uint16View);  // Uint16Array(4) [4660, 0, 0, 0]

// See how it's represented in the 8-bit view (depends on endianness)
console.log(uint8View);  // Typically Uint8Array(8) [52, 18, 0, 0, 0, 0, 0, 0] on little-endian systems

// Using TypedArrays for binary file operations (in browsers)
async function readImageAsGrayscale(url) {
    // Fetch the image
    const response = await fetch(url);
    const arrayBuffer = await response.arrayBuffer();
    
    // Create a view of the data
    const uint8View = new Uint8Array(arrayBuffer);
    
    // Process the image data (simple example)
    const grayscale = new Uint8Array(uint8View.length);
    
    // For simplicity, assume RGB format (3 bytes per pixel)
    for (let i = 0; i < uint8View.length; i += 3) {
        // Simple grayscale conversion: average the RGB values
        const r = uint8View[i];
        const g = uint8View[i + 1];
        const b = uint8View[i + 2];
        
        const gray = Math.round((r + g + b) / 3);
        
        grayscale[i] = gray;
        grayscale[i + 1] = gray;
        grayscale[i + 2] = gray;
    }
    
    return grayscale;
}

// Example of using TypedArrays for numerical computations
function computeAverage(data) {
    const float64Data = new Float64Array(data);
    let sum = 0;
    
    for (const value of float64Data) {
        sum += value;
    }
    
    return sum / float64Data.length;
}

console.log(computeAverage([1, 2, 3, 4, 5]));  // 3
```
