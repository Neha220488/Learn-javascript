# Modern JavaScript: ES6+ Features

## Destructuring Assignment

**Q: What is destructuring assignment and how does it work with arrays and objects?**

**A:** Destructuring assignment extracts multiple values from arrays/objects into distinct variables in one operation, making code more readable and concise.

**Array Destructuring:**

```javascript
// Array destructuring
const numbers = [1, 2, 3, 4, 5];
const [first, second, ...rest] = numbers;

console.log(first);  // 1
console.log(second);  // 2
console.log(rest);  // [3, 4, 5]

// Skipping elements
const colors = ['red', 'green', 'blue'];
const [primary, , tertiary] = colors;
console.log(primary);  // 'red'
console.log(tertiary);  // 'blue'

// Default values
const [a = 5, b = 7] = [1];
console.log(a);  // 1 (from array)
console.log(b);  // 7 (default value)

// Swapping variables
let x = 10;
let y = 20;
[x, y] = [y, x];
console.log(x);  // 20
console.log(y);  // 10
```

**Q: How does object destructuring work and what are its advanced features?**

**A:** Object destructuring extracts properties from objects into variables with matching names, but supports renaming, nesting, and defaults.

**Object Destructuring:**
```javascript
// Basic object destructuring
const person = {
    name: 'John',
    age: 30,
    location: {
        city: 'New York',
        country: 'USA'
    }
};

const { name, age } = person;
console.log(name);  // 'John'
console.log(age);  // 30

// Renaming variables
const { name: fullName, age: years } = person;
console.log(fullName);  // 'John'
console.log(years);  // 30

// Nested destructuring
const { location: { city, country } } = person;
console.log(city);  // 'New York'
console.log(country);  // 'USA'
```

**Q: How can destructuring be used with function parameters?**

**A:** Function parameter destructuring allows direct extraction of object/array values in the parameter list:

```javascript
// Function parameter destructuring
function displayPerson({ name, age }) {
    console.log(`${name} is ${age} years old`);
}

displayPerson(person);  // "John is 30 years old"

// With default values
function greetUser({ name = 'Guest', greeting = 'Hello' } = {}) {
    return `${greeting}, ${name}!`;
}

console.log(greetUser({ name: 'Alice' }));  // "Hello, Alice!"
console.log(greetUser());  // "Hello, Guest!"
```

**Memory Tip:** 🎁 "Destructuring **unpacks** like opening a gift box - extracting specific items!"

## Spread and Rest Operators

**Q: What are the spread and rest operators and how do they differ?**

**A:** Both use the same `...` syntax but serve opposite purposes - spread "spreads" elements out, while rest "collects" elements together.

**Spread Operator (Expanding):**
```javascript
// Array spreading
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];  // Spreads both arrays
console.log(combined);  // [1, 2, 3, 4, 5, 6]

// Array copying (shallow)
const original = [1, 2, 3];
const copy = [...original];
copy.push(4);
console.log(original);  // [1, 2, 3] - unchanged
console.log(copy);  // [1, 2, 3, 4]

// Function arguments
function sum(a, b, c) {
    return a + b + c;
}
const numbers = [1, 2, 3];
console.log(sum(...numbers));  // 6 - spreads array as arguments
```

**Q: How does the spread operator work with objects and what are common use cases?**

**A:** Object spread creates shallow copies and merges objects, useful for immutable updates:

```javascript
// Object spreading and merging
const basic = { firstName: 'John', lastName: 'Doe' };
const details = { job: 'Developer', country: 'USA' };
const fullProfile = { ...basic, ...details };
console.log(fullProfile);
// { firstName: 'John', lastName: 'Doe', job: 'Developer', country: 'USA' }

// Overriding properties
const user = { name: 'Alice', age: 25, role: 'user' };
const admin = { ...user, role: 'admin' };  // Override role
console.log(admin);  // { name: 'Alice', age: 25, role: 'admin' }

// React-style state updates
const state = { count: 0, theme: 'dark' };
const newState = { ...state, count: state.count + 1 };
```

**Q: How does the rest operator work in function parameters and destructuring?**

**A:** Rest operator collects remaining elements into an array, useful for variable-length parameters:

```javascript
// Rest parameters in functions
function collect(...items) {
    console.log(items);  // Array of all arguments
    return items.length;
}
console.log(collect(1, 2, 3, 4, 5));  // [1, 2, 3, 4, 5], returns 5

// Rest with array destructuring
const [first, second, ...others] = [1, 2, 3, 4, 5];
console.log(first);   // 1
console.log(second);  // 2
console.log(others);  // [3, 4, 5]

// Rest with object destructuring  
const { name, age, ...otherProps } = { 
    name: 'Alice', age: 25, job: 'Engineer', city: 'NYC' 
};
console.log(name);        // 'Alice'
console.log(age);         // 25
console.log(otherProps);  // { job: 'Engineer', city: 'NYC' }
```

**Memory Tip:** 🌟 "Spread **S**preads things out, Rest **R**eunites them together!"

## Template Literals

**Q: What are template literals and how do they improve string handling?**

**A:** Template literals use backticks (`) and provide string interpolation with `${expression}`, multi-line strings, and tagged templates for advanced processing.

**Basic Features:**

```javascript
// Basic string interpolation
const name = 'Alice';
const greeting = `Hello, ${name}!`;
console.log(greeting);  // "Hello, Alice!"

// Expressions in template literals
const a = 5;
const b = 10;
console.log(`The sum of ${a} and ${b} is ${a + b}`);  // "The sum of 5 and 10 is 15"

// Multi-line strings
const multiLine = `This is line one.
This is line two.
This is line three.`;
console.log(multiLine);
// This is line one.
// This is line two.
// This is line three.
```

**Q: What are tagged templates and how do they work?**

**A:** Tagged templates allow custom processing of template literals by passing the string parts and values to a function:

```javascript
// Tagged templates
function highlight(strings, ...values) {
    return strings.reduce((result, str, i) => {
        return result + str + (values[i] ? `<strong>${values[i]}</strong>` : '');
    }, '');
}

const name = 'Alice';
const age = 28;
const highlighted = highlight`My name is ${name} and I am ${age} years old.`;
console.log(highlighted);
// "My name is <strong>Alice</strong> and I am <strong>28</strong> years old."

// Raw strings
console.log(String.raw`Line 1\nLine 2`);  // "Line 1\nLine 2" (backslash is not processed)
```

**Q: How do template literals compare to traditional string concatenation?**

**A:** Template literals offer cleaner syntax and better performance:

| Traditional | Template Literal |
|-------------|------------------|
| `'Hello ' + name + '!'` | `` `Hello ${name}!` `` |
| `'Line 1\nLine 2'` | `` `Line 1\nLine 2` `` |
| Complex concatenation | Clean, readable syntax |

**Memory Tip:** 🎭 "Template literals use backticks like **T**heatre **T**ickets with placeholders!"

## let and const Declarations

**Q: How do let and const differ from var in terms of scope and behavior?**

**A:** `let` and `const` provide block scope instead of function scope, and prevent redeclaration within the same scope.

**Scope Differences:**
```javascript
// var is function-scoped
function varExample() {
    var x = 1;
    if (true) {
        var x = 2;  // Same variable - redeclares outer x
        console.log(x);  // 2
    }
    console.log(x);  // 2 (changed by inner scope)
}

// let is block-scoped
function letExample() {
    let y = 1;
    if (true) {
        let y = 2;  // Different variable - new block scope
        console.log(y);  // 2
    }
    console.log(y);  // 1 (unchanged - outer scope)
}

// const is block-scoped and immutable reference
const z = 1;
if (true) {
    const z = 2;  // Different variable in block scope
    console.log(z);  // 2
}
console.log(z);  // 1 (outer scope unchanged)
```

**Q: What is the difference between const immutability and object/array mutability?**

**A:** `const` prevents reassignment of the variable reference, but object/array contents can still be modified:

```javascript
// const prevents reassignment
const PI = 3.14159;
// PI = 3.14;  // ❌ TypeError: Assignment to constant variable

// const with objects - reference is immutable, content is mutable
const person = { name: 'John', age: 25 };
person.name = 'Jane';     // ✅ Modifying property works
person.age = 30;          // ✅ Adding property works
delete person.age;        // ✅ Deleting property works
// person = {};           // ❌ TypeError: Assignment to constant variable

// const with arrays
const colors = ['red', 'blue'];
colors.push('green');     // ✅ Modifying array works
colors[0] = 'yellow';     // ✅ Changing elements works
// colors = [];           // ❌ TypeError: Assignment to constant variable
```

**Q: What is the Temporal Dead Zone (TDZ) and how does it affect let and const?**

**A:** TDZ is the period between entering scope and variable declaration where variables cannot be accessed:

```javascript
function tdzExample() {
    // Hoisting behavior differences
    console.log(usingVar);    // undefined (var is hoisted and initialized)
    // console.log(usingLet); // ReferenceError: Cannot access before initialization
    // console.log(usingConst); // ReferenceError: Cannot access before initialization
    
    var usingVar = 'I am var';
    let usingLet = 'I am let';
    const usingConst = 'I am const';
}

// Block scope and TDZ
{
    // TDZ starts here for 'temp'
    console.log(typeof temp);  // ReferenceError (not 'undefined')
    let temp = 'value';        // TDZ ends here
}
```

**Best Practices:**
- ✅ Use `const` by default for immutable references
- ✅ Use `let` when reassignment is needed
- ❌ Avoid `var` in modern JavaScript

**Memory Tip:** 🏠 "**let** things change in their block, **const**ant things stay the same reference, **var** roams the whole function!"

## Arrow Functions

**Q: What are arrow functions and how do they differ from traditional functions?**

**A:** Arrow functions provide concise syntax and lexical `this` binding, but have limitations compared to traditional functions.

**Syntax Variations:**
```javascript
// Traditional function expression
const add1 = function(a, b) {
    return a + b;
};

// Arrow function with explicit return
const add2 = (a, b) => {
    return a + b;
};

// Arrow function with implicit return (single expression)
const add3 = (a, b) => a + b;

// Single parameter (parentheses optional)
const square1 = (x) => x * x;
const square2 = x => x * x;

// No parameters (parentheses required)
const getTimestamp = () => Date.now();

console.log(add1(2, 3));  // 5
console.log(add2(2, 3));  // 5  
console.log(add3(2, 3));  // 5
console.log(square1(4));  // 16
console.log(square2(4));  // 16
```

**Q: How does `this` binding work differently in arrow functions?**

**A:** Arrow functions inherit `this` from their enclosing scope (lexical binding), unlike traditional functions that have their own `this` context:

```javascript
// Lexical 'this' binding example
function Counter() {
    this.count = 0;
    
    // ❌ Traditional function - 'this' refers to global object
    setInterval(function() {
        this.count++;        // 'this' is window/global
        console.log(this.count);  // NaN (undefined + 1)
    }, 1000);
}

function BetterCounter() {
    this.count = 0;
    
    // ✅ Arrow function - 'this' inherited from BetterCounter
    setInterval(() => {
        this.count++;        // 'this' is BetterCounter instance
        console.log(this.count);  // 1, 2, 3, ...
    }, 1000);
}

// Object method context
const person = {
    name: 'Alice',
    
    // ✅ Traditional method - 'this' is person
    sayHello1: function() {
        return `Hello, my name is ${this.name}`;
    },
    
    // ✅ Method shorthand - 'this' is person
    sayHello2() {
        return `Hello, my name is ${this.name}`;
    },
    
    // ❌ Arrow function - 'this' is outer scope (not person)
    sayHello3: () => {
        return `Hello, my name is ${this.name}`;  // 'this' is not person!
    }
};

console.log(person.sayHello1());  // "Hello, my name is Alice"
console.log(person.sayHello2());  // "Hello, my name is Alice"
console.log(person.sayHello3());  // "Hello, my name is undefined"
```

**Q: What are the limitations of arrow functions?**

**A:** Arrow functions cannot be used in certain scenarios where traditional functions are required:

```javascript
// ❌ Cannot be used as constructors
const Person = (name) => {
    this.name = name;
};
// new Person('John'); // TypeError: Person is not a constructor

// ❌ No arguments object
const traditionalFunc = function() {
    console.log(arguments);  // Available
};
const arrowFunc = () => {
    // console.log(arguments);  // ReferenceError
    // Use rest parameters instead: (...args) => console.log(args)
};

// ❌ Cannot be used as methods when 'this' binding is needed
const obj = {
    value: 42,
    getValue: () => this.value  // 'this' is not obj
};

// ✅ Use traditional functions for object methods
const objFixed = {
    value: 42,
    getValue() { return this.value; }  // 'this' is obj
};
```

**When to Use Each:**
- **Arrow Functions**: Callbacks, array methods, preserving `this` context
- **Traditional Functions**: Object methods, constructors, when `arguments` object is needed

**Memory Tip:** ➡️ "Arrow functions **point** to parent scope for `this` - no binding of their own!"

## Default Parameters

**Q: What are default parameters and how do they work with different value types?**

**A:** Default parameters provide fallback values when arguments are missing or `undefined`, but not for other falsy values like `null` or `false`.

**Basic Default Parameters:**
```javascript
// Simple default values
function greet(name = 'Guest') {
    return `Hello, ${name}!`;
}

console.log(greet('John'));      // "Hello, John!"
console.log(greet());            // "Hello, Guest!" (undefined)
console.log(greet(undefined));   // "Hello, Guest!" (undefined)
console.log(greet(null));        // "Hello, null!" (null is valid)
console.log(greet(''));          // "Hello, !" (empty string is valid)
console.log(greet(0));           // "Hello, 0!" (0 is valid)

// Multiple default parameters
function calculateTax(price, taxRate = 0.1, currency = '$') {
    const tax = price * taxRate;
    return `${currency}${tax.toFixed(2)}`;
}

console.log(calculateTax(100));           // "$10.00"
console.log(calculateTax(100, 0.2));      // "$20.00"
console.log(calculateTax(100, 0.2, '€')); // "€20.00"
```

**Q: How can default parameters use expressions and reference other parameters?**

**A:** Default parameters can be expressions evaluated at call time and can reference previously declared parameters:

```javascript
// Using expressions as defaults
function createId(prefix = 'user', timestamp = Date.now()) {
    return `${prefix}-${timestamp}`;
}

console.log(createId());              // "user-1640995200000"
console.log(createId('admin'));       // "admin-1640995200001"

// Using previous parameters in defaults
function calculateTotal(price, taxRate = 0.1, total = price * (1 + taxRate)) {
    return total.toFixed(2);
}

console.log(calculateTotal(100));         // "110.00"
console.log(calculateTotal(100, 0.2));    // "120.00"
console.log(calculateTotal(100, 0.2, 150)); // "150.00"

// Function calls as defaults
function getRandomColor() {
    const colors = ['red', 'blue', 'green', 'yellow'];
    return colors[Math.floor(Math.random() * colors.length)];
}

function setTheme(backgroundColor = getRandomColor(), textColor = 'black') {
    return { backgroundColor, textColor };
}
```

**Q: How do default parameters work with destructuring?**

**A:** Default parameters combine with destructuring for flexible function signatures:

```javascript
// Object destructuring with defaults
function createUser({ name = 'Anonymous', role = 'User', active = true } = {}) {
    return {
        name,
        role,
        active,
        createdAt: new Date()
    };
}

console.log(createUser({ name: 'John', role: 'Admin' }));
// { name: 'John', role: 'Admin', active: true, createdAt: [Date] }

console.log(createUser({ name: 'Alice' }));
// { name: 'Alice', role: 'User', active: true, createdAt: [Date] }

console.log(createUser());
// { name: 'Anonymous', role: 'User', active: true, createdAt: [Date] }

// Array destructuring with defaults
function processCoordinates([x = 0, y = 0, z = 0] = []) {
    return { x, y, z };
}

console.log(processCoordinates([1, 2]));     // { x: 1, y: 2, z: 0 }
console.log(processCoordinates());           // { x: 0, y: 0, z: 0 }
```

**Best Practices:**
- ✅ Use defaults for optional parameters
- ✅ Place parameters with defaults at the end
- ✅ Consider using object destructuring for multiple optional parameters
- ❌ Don't use complex computations in defaults (performance impact)

**Memory Tip:** 🎯 "Default parameters are **safety nets** - catch `undefined`, ignore other falsy values!"

## Modules (import/export)

**Q: What are ES6 modules and how do they improve code organization?**

**A:** ES6 modules provide standardized import/export syntax for sharing code between files. Each module has its own scope, preventing global namespace pollution.

**Named Exports & Imports:**

```javascript
// math.js - exporting
// Named exports
export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

// Export after declaration
function square(x) {
    return x * x;
}

function cube(x) {
    return x * x * x;
}

export { square, cube as power3 };  // Renaming export
```

**Q: How do default exports work differently from named exports?**

**A:** Default exports allow one primary export per module without curly braces in import:

```javascript
// math.js - default export (only one per module)
export default function multiply(a, b) {
    return a * b;
}

// app.js - importing
import multiply from './math.js';           // Default import (no braces)
import { PI, add, subtract } from './math.js';  // Named imports (with braces)
import { square, power3 as cubeFunction } from './math.js';  // Import with alias

console.log(multiply(2, 3));  // 6
console.log(add(2, 3));  // 5
console.log(PI);  // 3.14159
console.log(cubeFunction(3));  // 27
```

**Q: What are the different ways to import modules?**

**A:** ES6 provides multiple import patterns for different needs:

```javascript
// Import everything as namespace
import * as math from './math.js';
console.log(math.subtract(5, 2));  // 3

// Dynamic imports (ES2020) - for code splitting
async function loadMathModule() {
    try {
        const math = await import('./math.js');
        console.log(math.add(2, 3));  // 5
    } catch (error) {
        console.error('Error loading module:', error);
    }
}
```

**Module Benefits:**
- ✅ **Encapsulation**: Private scope per module
- ✅ **Reusability**: Share code across files  
- ✅ **Tree Shaking**: Bundle only used exports
- ✅ **Static Analysis**: Compile-time dependency checking

**Memory Tip:** 📦 "Modules are like **M**ail packages - **export** to send, **import** to receive!"

## Classes

**Q: What are ES6 classes and how do they relate to JavaScript's prototype system?**

**A:** ES6 classes are syntactical sugar over JavaScript's prototype-based inheritance, providing cleaner syntax for creating objects and handling inheritance.

**Basic Class Features:**

```javascript
// Basic class with constructor, methods, getters, and setters
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    // Instance method
    greet() {
        return `Hello, my name is ${this.name}`;
    }
    
    // Getter
    get profile() {
        return `${this.name}, ${this.age} years old`;
    }
    
    // Setter
    set profile(value) {
        [this.name, this.age] = value.split(',');
    }
    
    // Static method (called on class, not instances)
    static isAdult(age) {
        return age >= 18;
    }
}

// Usage
const alice = new Person('Alice', 28);
console.log(alice.greet());  // "Hello, my name is Alice"
console.log(alice.profile);  // "Alice, 28 years old"

alice.profile = 'Bob,30';   // Uses setter
console.log(alice.name);    // "Bob"

console.log(Person.isAdult(20));  // true (static method)
```

**Q: How does class inheritance work with extends and super?**

**A:** Class inheritance uses `extends` for inheritance and `super` to call parent constructors and methods:

```javascript
// Class inheritance with extends
class Employee extends Person {
    constructor(name, age, jobTitle) {
        super(name, age);           // Call parent constructor
        this.jobTitle = jobTitle;
    }
    
    // Override parent method
    greet() {
        return `${super.greet()} and I work as a ${this.jobTitle}`;
    }
    
    // New method
    work() {
        return `${this.name} is working`;
    }
}

const john = new Employee('John', 35, 'Developer');
console.log(john.greet());  // "Hello, my name is John and I work as a Developer"
console.log(john.work());   // "John is working"
console.log(john.profile);  // "John, 35 years old" (inherited getter)
```

**Q: How do private fields and methods work in modern JavaScript classes?**

**A:** Private fields (`#field`) and methods (`#method`) are truly private and cannot be accessed outside the class:

```javascript
// Private class features (ES2020+)
class BankAccount {
    // Private field
    #balance = 0;
    
    constructor(owner, initialBalance) {
        this.owner = owner;
        if (initialBalance > 0) {
            this.#deposit(initialBalance);
        }
    }
    
    // Private method
    #deposit(amount) {
        this.#balance += amount;
    }
    
    // Public methods accessing private features
    deposit(amount) {
        if (amount <= 0) {
            throw new Error('Deposit amount must be positive');
        }
        this.#deposit(amount);
        return this.#balance;
    }
    
    getBalance() {
        return this.#balance;
    }
}

const account = new BankAccount('Alice', 1000);
console.log(account.getBalance());  // 1000
account.deposit(500);               // 1500
// console.log(account.#balance);   // SyntaxError: Private field
// account.#deposit(100);           // SyntaxError: Private method
```

**Memory Tip:** 🏫 "Classes are like **school blueprints** - define structure once, create many instances!"

## Promises

**Q: What are Promises and how do they improve asynchronous programming?**

**A:** Promises represent eventual completion/failure of asynchronous operations, providing cleaner alternatives to callback patterns and enabling better error handling.

**Promise States and Basic Usage:**
```javascript
// Promise has three states: pending, fulfilled, rejected
const promise = new Promise((resolve, reject) => {
    const success = Math.random() > 0.5;
    
    setTimeout(() => {
        if (success) {
            resolve('Operation successful');  // Fulfilled state
        } else {
            reject(new Error('Operation failed'));  // Rejected state
        }
    }, 1000);
});

// Consuming promises with then/catch/finally
promise
    .then(result => {
        console.log('Success:', result);
        return result.toUpperCase();  // Chain transformations
    })
    .then(upperResult => {
        console.log('Transformed:', upperResult);
    })
    .catch(error => {
        console.error('Error:', error.message);
    })
    .finally(() => {
        console.log('Promise settled (always runs)');
    });
```

**Q: How do Promise utility methods help with multiple asynchronous operations?**

**A:** Promise utility methods handle multiple promises with different strategies:

```javascript
// Promise.all - wait for ALL to resolve (fail-fast)
const promise1 = Promise.resolve('First');
const promise2 = new Promise(resolve => setTimeout(() => resolve('Second'), 100));
const promise3 = fetch('/api/data').then(r => r.json());

Promise.all([promise1, promise2, promise3])
    .then(values => {
        console.log(values);  // ['First', 'Second', {...}]
        // All resolved successfully
    })
    .catch(error => {
        console.error('At least one failed:', error);
        // If ANY promise rejects, catch runs
    });

// Promise.race - first to settle wins
const fast = new Promise(resolve => setTimeout(() => resolve('Fast'), 100));
const slow = new Promise(resolve => setTimeout(() => resolve('Slow'), 500));

Promise.race([fast, slow])
    .then(result => {
        console.log(result);  // 'Fast' (first to resolve)
    });

// Promise.allSettled - wait for all to settle (never rejects)
const resolved = Promise.resolve(42);
const rejected = Promise.reject(new Error('Failed'));

Promise.allSettled([resolved, rejected])
    .then(results => {
        console.log(results);
        // [
        //   { status: 'fulfilled', value: 42 },
        //   { status: 'rejected', reason: Error: Failed }
        // ]
        results.forEach((result, index) => {
            if (result.status === 'fulfilled') {
                console.log(`Promise ${index} succeeded:`, result.value);
            } else {
                console.log(`Promise ${index} failed:`, result.reason);
            }
        });
    });
```

**Q: What are Promise.any and how do you create resolved/rejected promises?**

**A:** Promise.any resolves when the first promise resolves, and static methods create immediate promises:

```javascript
// Promise.any - first successful resolution (ES2021)
const promises = [
    Promise.reject(new Error('First error')),
    Promise.resolve('Success!'),
    Promise.reject(new Error('Second error'))
];

Promise.any(promises)
    .then(result => {
        console.log(result);  // 'Success!' (first to resolve)
    })
    .catch(error => {
        console.error('All promises rejected:', error);
        // Only runs if ALL promises reject
    });

// Static methods for immediate promises
const immediate = Promise.resolve('Already resolved');
const failed = Promise.reject(new Error('Already rejected'));

immediate.then(console.log);  // 'Already resolved'
failed.catch(console.error);  // Error: Already rejected

// Converting values to promises
const values = [1, 2, 3];
const promisedValues = values.map(value => Promise.resolve(value * 2));
Promise.all(promisedValues).then(console.log);  // [2, 4, 6]
```

**Promise vs Callback Comparison:**
```javascript
// ❌ Callback hell
getData(function(a) {
    getMoreData(a, function(b) {
        getEvenMoreData(b, function(c) {
            getFinalData(c, function(d) {
                console.log(d);
            });
        });
    });
});

// ✅ Promise chain
getData()
    .then(a => getMoreData(a))
    .then(b => getEvenMoreData(b))
    .then(c => getFinalData(c))
    .then(d => console.log(d))
    .catch(error => console.error('Error:', error));
```

**Memory Tip:** 🤝 "Promises are **handshake agreements** - they'll let you know when the job is done (or failed)!"

## Optional Chaining

**Q: What is optional chaining and how does it solve property access issues?**

**A:** Optional chaining (`?.`) safely accesses nested object properties without throwing errors if a reference is null/undefined. It short-circuits and returns undefined instead.

**Basic Usage:**

```javascript
// Without optional chaining - verbose and error-prone
function getStreetName(user) {
    if (user && user.address && user.address.street) {
        return user.address.street.name;
    }
    return undefined;
}

// With optional chaining - clean and safe
function getStreetName(user) {
    return user?.address?.street?.name;
}
```

**Q: How does optional chaining work with different data structures?**

**A:** Optional chaining works with object properties, method calls, and array elements:

```javascript
const user1 = {
    name: 'Alice',
    address: {
        street: { name: 'Maple Street', number: 123 },
        city: 'Wonderland'
    }
};

const user2 = {
    name: 'Bob',
    address: { city: 'Example City' }  // No street information
};

const user3 = null;

console.log(getStreetName(user1));  // 'Maple Street'
console.log(getStreetName(user2));  // undefined (no error)
console.log(getStreetName(user3));  // undefined (no error)
```

**Q: How does optional chaining work with method calls and arrays?**

**A:** Use `?.()` for optional method calls and `?.[index]` for optional array access:

```javascript
// Optional chaining with method calls
const data = {
    process() {
        return 'Processed data';
    }
};

console.log(data.process?.());  // 'Processed data'
console.log(data.nonExistentMethod?.());  // undefined (no error)

// Optional chaining with array elements
const items = [1, 2, 3];
console.log(items?.[0]);  // 1
console.log(items?.[10]);  // undefined

// Combining with nullish coalescing
console.log(user2?.address?.street?.name ?? 'No street found');  // 'No street found'
```

**Memory Tip:** ⛓️ "Optional chaining `?.` = **safe chain** that won't break if links are missing!"

## Nullish Coalescing

**Q: What is nullish coalescing and how does it differ from the logical OR operator?**

**A:** Nullish coalescing (`??`) provides default values only for `null` or `undefined`, while logical OR (`||`) provides defaults for any falsy value.

**Key Differences:**
```javascript
// Logical OR vs. Nullish Coalescing comparison
function demonstrateDifference(value) {
    const withOR = value || 'default';
    const withNC = value ?? 'default';
    
    console.log(`Value: ${value}`);
    console.log(`OR (||): ${withOR}`);
    console.log(`NC (??): ${withNC}`);
    console.log('---');
}

// Testing different values
demonstrateDifference('Alice');
// Value: Alice
// OR (||): Alice
// NC (??): Alice

demonstrateDifference('');
// Value: 
// OR (||): default (empty string is falsy)
// NC (??):  (empty string is not null/undefined)

demonstrateDifference(0);
// Value: 0
// OR (||): default (0 is falsy)
// NC (??): 0 (0 is not null/undefined)

demonstrateDifference(false);
// Value: false
// OR (||): default (false is falsy)
// NC (??): false (false is not null/undefined)

demonstrateDifference(null);
// Value: null
// OR (||): default
// NC (??): default

demonstrateDifference(undefined);
// Value: undefined
// OR (||): default
// NC (??): default
```

**Q: What are practical use cases for nullish coalescing?**

**A:** Nullish coalescing is ideal for setting defaults while preserving legitimate falsy values like 0, false, or empty strings:

```javascript
// Configuration with meaningful falsy values
function createConfig(options = {}) {
    return {
        // ❌ Bad: OR operator overwrites valid falsy values
        enableFeature: options.enableFeature || true,    // false becomes true!
        maxRetries: options.maxRetries || 3,             // 0 becomes 3!
        username: options.username || 'guest',           // '' becomes 'guest'!
        
        // ✅ Good: Nullish coalescing preserves falsy values
        enableFeature: options.enableFeature ?? true,   // false stays false
        maxRetries: options.maxRetries ?? 3,             // 0 stays 0
        username: options.username ?? 'guest',           // '' stays ''
    };
}

console.log(createConfig({ enableFeature: false, maxRetries: 0, username: '' }));
// OR result: { enableFeature: true, maxRetries: 3, username: 'guest' }
// NC result: { enableFeature: false, maxRetries: 0, username: '' }

// API response handling
function processUser(apiResponse) {
    return {
        name: apiResponse?.name ?? 'Unknown User',
        score: apiResponse?.score ?? 0,        // 0 is valid score
        isActive: apiResponse?.isActive ?? true, // false is valid status
        lastLogin: apiResponse?.lastLogin ?? new Date()
    };
}
```

**Q: How does nullish coalescing work with optional chaining?**

**A:** They work perfectly together for safe property access with fallbacks:

```javascript
// Combining optional chaining and nullish coalescing
const user = {
    name: 'John',
    settings: {
        theme: 'dark',
        notifications: {
            email: false,  // Valid falsy value
            push: null     // Needs default
        }
    }
};

// Safe access with appropriate defaults
const theme = user?.settings?.theme ?? 'light';
const emailNotifs = user?.settings?.notifications?.email ?? true;
const pushNotifs = user?.settings?.notifications?.push ?? true;

console.log({ theme, emailNotifs, pushNotifs });
// { theme: 'dark', emailNotifs: false, pushNotifs: true }

// Deep object access with multiple fallbacks
function getNestedValue(obj, path, defaultValue) {
    return path.reduce((current, key) => current?.[key], obj) ?? defaultValue;
}

const config = {
    database: {
        connection: {
            timeout: 0,  // Valid 0 value
            retries: null  // Needs default
        }
    }
};

const timeout = getNestedValue(config, ['database', 'connection', 'timeout'], 5000);
const retries = getNestedValue(config, ['database', 'connection', 'retries'], 3);

console.log({ timeout, retries }); // { timeout: 0, retries: 3 }
```

**When to Use Each:**
- **Nullish Coalescing (`??`)**: When `0`, `false`, `''` are valid values
- **Logical OR (`||`)**: When any falsy value should trigger the default

**Memory Tip:** 🔍 "Nullish coalescing is **picky** - only `null` and `undefined` trigger defaults!"
