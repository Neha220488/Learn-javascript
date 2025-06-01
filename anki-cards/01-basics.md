## What is JavaScript?
JavaScript is a lightweight, interpreted, or just-in-time compiled programming language with first-class functions. While it is most well-known as the scripting language for Web pages, many non-browser environments also use it.

JavaScript is prototype-based, multi-paradigm, single-threaded, dynamic language, supporting object-oriented, imperative, and declarative styles.

**Q&A Format:**

**Q: What are the key characteristics that define JavaScript as a programming language?**
A: JavaScript has several defining characteristics:
- **Lightweight**: Simple syntax, minimal computing requirements
- **Interpreted/JIT compiled**: Code executed line-by-line or compiled just before execution
- **First-class functions**: Functions can be stored in variables, passed as arguments, and returned
- **Dynamic**: Types associated with values, not variables; can change during execution

**Q: What programming paradigms does JavaScript support?**
A: JavaScript is **multi-paradigm**, supporting:
- **Object-oriented**: Using objects and inheritance
- **Functional**: Using functions as first-class citizens
- **Imperative**: Step-by-step instructions
- **Declarative**: Describing what should happen rather than how

**Q: What does "prototype-based" mean in JavaScript?**
A: In prototype-based inheritance, objects inherit directly from other objects rather than from classes. Every object has a prototype object, and properties/methods are looked up through the prototype chain. This differs from class-based languages like Java or C++.

**Q: Why is JavaScript called "single-threaded" and how does it handle multiple operations?**
A: JavaScript executes one operation at a time in a single sequence (single-threaded). However, it handles multiple operations through:
- **Event loop**: Manages asynchronous operations
- **Callbacks**: Functions executed after operations complete
- **Promises/async-await**: Modern asynchronous patterns
- **Web APIs**: Browser APIs run operations outside the main thread

```javascript
// Hello world example
console.log("Hello, World!");

// Multiple ways to use JavaScript
// 1. In the browser
document.getElementById("demo").innerHTML = "Hello, World!";

// 2. In Node.js environment
const http = require('http');
http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World!');
}).listen(8080);

// 3. As a scripting language
const sum = (a, b) => a + b;
console.log(sum(5, 3));  // 8
```

## Variable Declarations (var, let, const)
JavaScript provides three ways to declare variables, each with different scoping rules and behaviors.

**Q&A Format:**

**Q: What are the key differences between var, let, and const in terms of scope?**
A: 
- **var**: Function-scoped or globally-scoped, can be redeclared
- **let**: Block-scoped, cannot be redeclared in same scope
- **const**: Block-scoped, cannot be redeclared or reassigned
- Block scope means variables are only accessible within the nearest enclosing braces `{}`

**Q: What is hoisting and how does it affect var, let, and const?**
A: Hoisting moves declarations to the top of their scope:
- **var**: Hoisted and initialized with `undefined`
- **let/const**: Hoisted but not initialized (Temporal Dead Zone)
- **Best practice**: Always declare variables before use

**Q: When should you use const vs let vs var?**
A: Modern JavaScript best practices:
- **const**: Default choice for values that won't be reassigned
- **let**: When you need to reassign the variable
- **var**: Avoid in modern code (legacy only)
- **Rule**: Use const by default, let when needed, avoid var

```javascript
// var examples - function scoped
function varExample() {
    console.log(x); // undefined (hoisted but not assigned)
    var x = 5;
    console.log(x); // 5
    
    if (true) {
        var x = 10; // Same variable due to function scope
        console.log(x); // 10
    }
    console.log(x); // 10 (var ignores block scope)
}

// let examples - block scoped
function letExample() {
    // console.log(y); // ReferenceError: Cannot access 'y' before initialization
    let y = 5;
    console.log(y); // 5
    
    if (true) {
        let y = 10; // Different variable due to block scope
        console.log(y); // 10
    }
    console.log(y); // 5 (original y unchanged)
}

// const examples - block scoped, cannot reassign
function constExample() {
    const z = 5;
    console.log(z); // 5
    
    // z = 10; // TypeError: Assignment to constant variable
    
    // But objects/arrays can be mutated
    const obj = { name: 'John' };
    obj.name = 'Jane'; // This is allowed
    obj.age = 25; // This is allowed
    console.log(obj); // { name: 'Jane', age: 25 }
    
    const arr = [1, 2, 3];
    arr.push(4); // This is allowed
    console.log(arr); // [1, 2, 3, 4]
    
    // arr = [5, 6, 7]; // TypeError: Assignment to constant variable
}

// Temporal Dead Zone example
function temporalDeadZone() {
    console.log(typeof a); // undefined (var)
    // console.log(typeof b); // ReferenceError (let)
    // console.log(typeof c); // ReferenceError (const)
    
    var a = 1;
    let b = 2;
    const c = 3;
}

// Loop examples showing scope differences
console.log('=== Loop Examples ===');

// var in loops - problematic
console.log('var in setTimeout:');
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log('var:', i), 100); // Prints: 3, 3, 3
}

// let in loops - works as expected
console.log('let in setTimeout:');
for (let j = 0; j < 3; j++) {
    setTimeout(() => console.log('let:', j), 200); // Prints: 0, 1, 2
}

// Block scope demonstration
{
    let blockScoped = 'I am block scoped';
    const alsoBlockScoped = 'Me too';
    var notBlockScoped = 'I escape the block';
}

// console.log(blockScoped); // ReferenceError
// console.log(alsoBlockScoped); // ReferenceError
console.log(notBlockScoped); // 'I escape the block'

// Redeclaration examples
var name = 'John';
var name = 'Jane'; // OK with var
console.log(name); // 'Jane'

let age = 25;
// let age = 30; // SyntaxError: Identifier 'age' has already been declared

const PI = 3.14159;
// const PI = 3.14; // SyntaxError: Identifier 'PI' has already been declared
```

## Data Types and Primitives
JavaScript has several built-in data types that can be categorized as primitive and non-primitive types.

**Q&A Format:**

**Q: What are the primitive data types in JavaScript?**
A: JavaScript has 7 primitive types:
- **Number**: Integers and floating-point numbers
- **String**: Text data in quotes
- **Boolean**: true or false values
- **Undefined**: Variable declared but not assigned
- **Null**: Intentional absence of value
- **Symbol**: Unique identifier (ES6)
- **BigInt**: Large integers beyond Number.MAX_SAFE_INTEGER (ES2020)

**Q: What's the difference between undefined and null?**
A: 
- **undefined**: Variable exists but has no value assigned
- **null**: Explicitly assigned to represent "no value"
- **typeof undefined**: "undefined"
- **typeof null**: "object" (historical bug in JavaScript)

**Q: How do you check the type of a variable in JavaScript?**
A: Use `typeof` operator for most types, with special cases:
- `typeof variable` returns string representation
- Arrays: Use `Array.isArray()`
- null: Check with `variable === null`
- Objects: Use `instanceof` or `Object.prototype.toString.call()`

```javascript
// Number examples
let integer = 42;
let float = 3.14159;
let negative = -100;
let infinity = Infinity;
let notANumber = NaN;

console.log(typeof integer); // "number"
console.log(typeof float); // "number"
console.log(typeof infinity); // "number"
console.log(typeof notANumber); // "number"

// Number methods and properties
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
console.log(Number.MIN_SAFE_INTEGER); // -9007199254740991
console.log(Number.isInteger(42)); // true
console.log(Number.isInteger(3.14)); // false
console.log(Number.isNaN(NaN)); // true
console.log(Number.isFinite(Infinity)); // false

// String examples
let singleQuotes = 'Hello, World!';
let doubleQuotes = "JavaScript is awesome";
let templateLiteral = `The answer is ${42}`;
let multiLine = `This is a
multi-line
string`;

console.log(typeof singleQuotes); // "string"
console.log(templateLiteral); // "The answer is 42"

// String methods
console.log(singleQuotes.length); // 13
console.log(singleQuotes.toUpperCase()); // "HELLO, WORLD!"
console.log(singleQuotes.includes('World')); // true
console.log(singleQuotes.slice(0, 5)); // "Hello"

// Boolean examples
let isTrue = true;
let isFalse = false;
let implicitTrue = Boolean("hello"); // true
let implicitFalse = Boolean(""); // false

console.log(typeof isTrue); // "boolean"

// Falsy values in JavaScript
console.log(Boolean(false)); // false
console.log(Boolean(0)); // false
console.log(Boolean(-0)); // false
console.log(Boolean(0n)); // false (BigInt zero)
console.log(Boolean("")); // false
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false
console.log(Boolean(NaN)); // false

// Truthy values (everything else)
console.log(Boolean("0")); // true (string "0")
console.log(Boolean([])); // true (empty array)
console.log(Boolean({})); // true (empty object)

// Undefined examples
let declared;
console.log(declared); // undefined
console.log(typeof declared); // "undefined"

function noReturn() {
    // No return statement
}
console.log(noReturn()); // undefined

// Null examples
let intentionallyEmpty = null;
console.log(intentionallyEmpty); // null
console.log(typeof intentionallyEmpty); // "object" (JavaScript quirk)
console.log(intentionallyEmpty === null); // true

// Symbol examples (ES6)
let sym1 = Symbol();
let sym2 = Symbol('description');
let sym3 = Symbol('description');

console.log(typeof sym1); // "symbol"
console.log(sym2 === sym3); // false (each symbol is unique)
console.log(sym2.description); // "description"

// BigInt examples (ES2020)
let bigNumber = 123456789012345678901234567890n;
let anotherBig = BigInt("123456789012345678901234567890");

console.log(typeof bigNumber); // "bigint"
console.log(bigNumber === anotherBig); // true

// Cannot mix BigInt with regular numbers
// console.log(bigNumber + 1); // TypeError
console.log(bigNumber + 1n); // 123456789012345678901234567891n

// Non-primitive types
let array = [1, 2, 3, 4, 5];
let object = { name: "John", age: 30 };
let func = function() { return "Hello"; };

console.log(typeof array); // "object"
console.log(typeof object); // "object"
console.log(typeof func); // "function"

// Better type checking
console.log(Array.isArray(array)); // true
console.log(Array.isArray(object)); // false

// Object.prototype.toString for precise type checking
console.log(Object.prototype.toString.call(array)); // "[object Array]"
console.log(Object.prototype.toString.call(object)); // "[object Object]"
console.log(Object.prototype.toString.call(func)); // "[object Function]"
console.log(Object.prototype.toString.call(null)); // "[object Null]"
console.log(Object.prototype.toString.call(undefined)); // "[object Undefined]"

// Type conversion examples
console.log('=== Type Conversion ===');

// String conversion
console.log(String(123)); // "123"
console.log(String(true)); // "true"
console.log(String(null)); // "null"
console.log(String(undefined)); // "undefined"

// Number conversion
console.log(Number("123")); // 123
console.log(Number("123.45")); // 123.45
console.log(Number("hello")); // NaN
console.log(Number(true)); // 1
console.log(Number(false)); // 0
console.log(Number(null)); // 0
console.log(Number(undefined)); // NaN

// Boolean conversion
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean("hello")); // true
console.log(Boolean("")); // false
```

## Type Conversion and Coercion
JavaScript automatically converts between types in certain situations (coercion) and provides methods for explicit conversion.

**Q&A Format:**

**Q: What's the difference between explicit and implicit type conversion?**
A: 
- **Explicit conversion**: Manually converting types using functions like `Number()`, `String()`, `Boolean()`
- **Implicit conversion (coercion)**: JavaScript automatically converts types during operations
- **Example**: `"5" + 3` results in `"53"` (implicit), while `Number("5") + 3` results in `8` (explicit)

**Q: How does JavaScript handle type coercion in different operations?**
A: 
- **Addition (+)**: If either operand is a string, converts to string concatenation
- **Other math operations**: Converts to numbers
- **Logical contexts**: Converts to boolean using truthy/falsy rules
- **Comparison**: `==` performs coercion, `===` doesn't

**Q: What are the rules for converting values to strings, numbers, and booleans?**
A: 
- **To String**: `null` → `"null"`, `undefined` → `"undefined"`, `true` → `"true"`, numbers → their string representation
- **To Number**: `""` → `0`, `"123"` → `123`, `true` → `1`, `false` → `0`, `null` → `0`, `undefined` → `NaN`
- **To Boolean**: Only 8 falsy values (false, 0, -0, 0n, "", null, undefined, NaN), everything else is truthy

```javascript
// Explicit Type Conversion
console.log('=== Explicit Type Conversion ===');

// Converting to String
console.log(String(123)); // "123"
console.log(String(true)); // "true"
console.log(String(false)); // "false"
console.log(String(null)); // "null"
console.log(String(undefined)); // "undefined"
console.log(String([1, 2, 3])); // "1,2,3"
console.log(String({name: "John"})); // "[object Object]"

// Using toString() method
console.log((123).toString()); // "123"
console.log(true.toString()); // "true"
console.log([1, 2, 3].toString()); // "1,2,3"

// Converting to Number
console.log(Number("123")); // 123
console.log(Number("123.45")); // 123.45
console.log(Number("123abc")); // NaN
console.log(Number("")); // 0
console.log(Number("   ")); // 0 (whitespace)
console.log(Number(true)); // 1
console.log(Number(false)); // 0
console.log(Number(null)); // 0
console.log(Number(undefined)); // NaN
console.log(Number([1])); // 1
console.log(Number([1, 2])); // NaN
console.log(Number({})); // NaN

// Using parseInt() and parseFloat()
console.log(parseInt("123")); // 123
console.log(parseInt("123.45")); // 123 (ignores decimal)
console.log(parseInt("123abc")); // 123 (stops at first non-digit)
console.log(parseInt("abc123")); // NaN (must start with digit)
console.log(parseInt("1010", 2)); // 10 (binary to decimal)
console.log(parseInt("FF", 16)); // 255 (hexadecimal to decimal)

console.log(parseFloat("123.45")); // 123.45
console.log(parseFloat("123.45abc")); // 123.45

// Unary + operator (shorthand for Number())
console.log(+"123"); // 123
console.log(+true); // 1
console.log(+false); // 0
console.log(+null); // 0
console.log(+undefined); // NaN

// Converting to Boolean
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean("hello")); // true
console.log(Boolean("")); // false
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false
console.log(Boolean([])); // true (empty array is truthy!)
console.log(Boolean({})); // true (empty object is truthy!)

// Double negation (shorthand for Boolean())
console.log(!!"hello"); // true
console.log(!!0); // false
console.log(!![1, 2, 3]); // true

// Implicit Type Conversion (Coercion)
console.log('=== Implicit Type Conversion ===');

// String concatenation with +
console.log("5" + 3); // "53" (number to string)
console.log(5 + "3"); // "53" (number to string)
console.log("5" + 3 + 2); // "532" (left to right)
console.log(5 + 3 + "2"); // "82" (5+3=8, then "8"+"2")
console.log("Hello" + true); // "Hellotrue"
console.log("Value: " + null); // "Value: null"

// Arithmetic operations (except +)
console.log("10" - 5); // 5 (string to number)
console.log("10" * "2"); // 20 (both strings to numbers)
console.log("10" / "2"); // 5
console.log("10" % 3); // 1
console.log("hello" - 5); // NaN (can't convert "hello" to number)

// Comparison coercion
console.log("5" == 5); // true (string "5" converted to number 5)
console.log(true == 1); // true (true converted to 1)
console.log(false == 0); // true (false converted to 0)
console.log(null == undefined); // true (special case)
console.log("" == 0); // true (empty string converted to 0)
console.log([1] == 1); // true (array converted to primitive)

// Strict comparison (no coercion)
console.log("5" === 5); // false (different types)
console.log(true === 1); // false
console.log(null === undefined); // false

// Boolean context coercion
if ("hello") {
    console.log("Strings are truthy (except empty string)");
}

if (0) {
    console.log("This won't print");
} else {
    console.log("0 is falsy");
}

// Logical operators and coercion
console.log("hello" && "world"); // "world" (both truthy, returns last)
console.log(0 && "hello"); // 0 (0 is falsy, returns first falsy)
console.log("" || "default"); // "default" (empty string is falsy)
console.log("value" || "default"); // "value" (first truthy)

// Array to primitive conversion
console.log([1, 2, 3] + ""); // "1,2,3" (array to string)
console.log(+[1]); // 1 (array to number)
console.log(+[1, 2]); // NaN (can't convert multi-element array to number)
console.log(+[]); // 0 (empty array to number)

// Object to primitive conversion
console.log({} + ""); // "[object Object]"
console.log(+{}); // NaN

// Special cases and gotchas
console.log('=== Special Cases ===');

console.log([] + []); // "" (both arrays become empty strings)
console.log([] + {}); // "[object Object]"
console.log({} + []); // "[object Object]" or 0 (depends on context)
console.log(true + true); // 2 (both convert to 1)
console.log(true + false); // 1 (true=1, false=0)

// Date object coercion
let date = new Date();
console.log(date + ""); // String representation of date
console.log(+date); // Timestamp (number of milliseconds since epoch)

// Function to check actual vs expected behavior
function demonstrateCoercion(expression, expected) {
    try {
        let result = eval(expression);
        console.log(`${expression} = ${result} (${typeof result})`);
        if (result == expected) console.log("✓ Matches expected");
        else console.log(`✗ Expected: ${expected}`);
    } catch (e) {
        console.log(`${expression} = Error: ${e.message}`);
    }
    console.log('---');
}

// Test tricky coercion cases
console.log('=== Tricky Cases ===');
demonstrateCoercion('"" + 1 + 0', "10");
demonstrateCoercion('"" - 1 + 0', -1);
demonstrateCoercion('true + false', 1);
demonstrateCoercion('6 / "3"', 2);
demonstrateCoercion('"2" * "3"', 6);
demonstrateCoercion('4 + 5 + "px"', "9px");
demonstrateCoercion('"$" + 4 + 5', "$45");
demonstrateCoercion('"4" - 2', 2);
demonstrateCoercion('"4px" - 2', NaN);
demonstrateCoercion('7 / 0', Infinity);
demonstrateCoercion('null + 1', 1);
demonstrateCoercion('undefined + 1', NaN);
```

## Comments and Basic Syntax
JavaScript supports single-line and multi-line comments for documenting code.

**Q&A Format:**

**Q: What are the different types of comments in JavaScript?**
A: JavaScript supports two types of comments:
- **Single-line comments**: Use `//` - everything after `//` on that line is ignored
- **Multi-line comments**: Use `/* */` - everything between these markers is ignored, can span multiple lines
- **JSDoc comments**: Use `/** */` - special format for documentation generation

**Q: What are the basic syntax rules for JavaScript?**
A: Key JavaScript syntax rules:
- **Case-sensitive**: `variable` and `Variable` are different
- **Statements end with semicolons**: Optional but recommended for clarity
- **Code blocks use curly braces**: `{ }` for functions, loops, conditionals
- **Whitespace is generally ignored**: Except within strings and certain contexts

**Q: What are JavaScript's naming conventions and reserved words?**
A: 
- **Variables/functions**: camelCase (`userName`, `getUserData`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_SIZE`, `API_URL`)
- **Classes**: PascalCase (`UserAccount`, `DataProcessor`)
- **Reserved words**: Cannot use keywords like `function`, `return`, `if`, `class`, etc. as variable names

```javascript
// Single-line comment
// This explains what the next line does
let userName = "Alice";

// You can also add comments at the end of a line
let age = 25; // User's age

/* 
   Multi-line comment
   This is useful for longer explanations
   that span multiple lines
*/
let userProfile = {
    name: userName,
    age: age,
    isActive: true
};

/**
 * JSDoc comment - used for documentation generation
 * @param {string} name - The user's name
 * @param {number} age - The user's age
 * @returns {Object} User profile object
 */
function createUserProfile(name, age) {
    return {
        name: name,
        age: age,
        createdAt: new Date()
    };
}

// Case sensitivity examples
let myVariable = "lowercase";
let MyVariable = "uppercase first letter";
let MYVARIABLE = "all uppercase";
// These are three different variables!

console.log(myVariable); // "lowercase"
console.log(MyVariable); // "uppercase first letter"
console.log(MYVARIABLE); // "all uppercase"

// Semicolons - optional but recommended
let x = 5; // With semicolon (recommended)
let y = 10 // Without semicolon (works but not recommended)

// Automatic Semicolon Insertion (ASI) can cause issues:
function problematicFunction() {
    return // ASI inserts semicolon here
        "This string is unreachable";
}
console.log(problematicFunction()); // undefined (not the string!)

// Correct version:
function correctFunction() {
    return "This string is reachable";
}
console.log(correctFunction()); // "This string is reachable"

// Code blocks and indentation
if (true) {
    console.log("This is inside a block");
    let blockScoped = "I'm only available in this block";
    
    if (true) {
        console.log("Nested blocks work too");
    }
}
// console.log(blockScoped); // ReferenceError: blockScoped is not defined

// Naming conventions examples
// Variables and functions: camelCase
let firstName = "John";
let lastName = "Doe";
let isLoggedIn = true;

function getUserName() {
    return firstName + " " + lastName;
}

function calculateTotal() {
    // Function names should describe what they do
    return 100;
}

// Constants: UPPER_SNAKE_CASE
const MAX_RETRY_ATTEMPTS = 3;
const API_BASE_URL = "https://api.example.com";
const DEFAULT_TIMEOUT = 5000;

// Classes: PascalCase
class UserAccount {
    constructor(username, email) {
        this.username = username;
        this.email = email;
    }
    
    getDisplayName() {
        return this.username;
    }
}

class DataProcessor {
    static processData(data) {
        return data.map(item => item.toString());
    }
}

// Reserved words - these cannot be used as variable names
/*
Reserved words include:
break, case, catch, class, const, continue, debugger, default, delete, do,
else, export, extends, finally, for, function, if, import, in, instanceof,
let, new, return, super, switch, this, throw, try, typeof, var, void,
while, with, yield
*/

// Examples of what NOT to do:
// let if = 5; // SyntaxError: Unexpected token 'if'
// let function = "test"; // SyntaxError
// let class = "MyClass"; // SyntaxError

// But you can use them as object properties:
let myObject = {
    if: "this works",
    function: "this also works",
    class: "and this too"
};

console.log(myObject.if); // "this works"
console.log(myObject.function); // "this also works"

// Whitespace rules
let spaced = 1 + 2 + 3; // Spaces around operators (recommended)
let notSpaced = 1+2+3; // Works but less readable

// Whitespace in strings is preserved
let message1 = "Hello World";
let message2 = "Hello    World"; // Multiple spaces preserved
console.log(message1); // "Hello World"
console.log(message2); // "Hello    World"

// Line breaks and formatting
let longExpression = 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10;

// Better formatting for readability:
let betterFormatted = 1 + 2 + 3 + 4 + 5 +
                     6 + 7 + 8 + 9 + 10;

// Or with parentheses:
let withParentheses = (
    1 + 2 + 3 + 4 + 5 +
    6 + 7 + 8 + 9 + 10
);

// String literals can span multiple lines with template literals
let multiLineString = `This is a 
multi-line 
string`;
console.log(multiLineString);

// Object and array formatting
let person = {
    name: "Alice",
    age: 30,
    city: "New York"
};

let numbers = [
    1, 2, 3,
    4, 5, 6,
    7, 8, 9
];

// Commenting best practices
// TODO: Implement user authentication
// FIXME: This function doesn't handle edge cases
// NOTE: This is a temporary workaround
// HACK: Quick fix for production issue

// Good comment - explains WHY, not WHAT
// Calculate tax using the 2024 tax brackets
let tax = income * 0.25;

// Bad comment - states the obvious
// Increment i by 1
// i++;

// Good comment - explains complex logic
// Use binary search to find insertion point
// Time complexity: O(log n)
function binarySearch(arr, target) {
    // Implementation here...
}

// Disable linting for specific lines (when necessary)
// eslint-disable-next-line no-console
console.log("This bypasses the no-console rule");

/* eslint-disable no-unused-vars */
let temporaryVariable = "This won't trigger unused variable warning";
/* eslint-enable no-unused-vars */
````