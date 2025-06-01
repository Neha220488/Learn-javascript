## Function Declaration
A function declaration defines a named function. It is hoisted, meaning it can be called before it is defined.

**Q: What is a function declaration and what are its key characteristics?**

A: A **function declaration** defines a named function using the `function` keyword. Key characteristics:

- **Hoisted**: Can be called before declaration
- **Function-scoped**: Creates a variable in current scope
- **Global properties**: Global declarations become properties on global object
- **Syntax**: `function name(parameters) { body }`

**Q: How does function hoisting work in practice?**

A: Function declarations are **fully hoisted** - both declaration and implementation are moved to the top:

**Q: What are the different ways to define parameters in function declarations?**

A: Function declarations support various parameter patterns:

- **Required parameters**: Standard position-based parameters
- **Default parameters**: ES6+ syntax for default values
- **Rest parameters**: Collect remaining arguments into array
- **Arguments object**: Legacy way to access all arguments

```javascript
// Basic function declaration
function greet(name) {
    return "Hello, " + name + "!";
}

console.log(greet("John"));  // "Hello, John!"

// Function declaration with multiple parameters
function add(a, b) {
    return a + b;
}

console.log(add(5, 3));  // 8

// Function declaration with no parameters
function getCurrentTime() {
    return new Date().toLocaleTimeString();
}

console.log(getCurrentTime());  // Current time, e.g., "10:30:45 AM"

// Function declaration with no return statement
// (returns undefined by default)
function logMessage(message) {
    console.log("Message: " + message);
    // No return statement
}

let result = logMessage("Hello");
console.log(result);  // undefined

// Demonstrating hoisting - calling a function before it's defined
console.log(multiply(4, 5));  // 20

function multiply(a, b) {
    return a * b;
}

// Function with default parameters
function greetUser(name = "Guest") {
    return "Hello, " + name + "!";
}

console.log(greetUser());       // "Hello, Guest!"
console.log(greetUser("John")); // "Hello, John!"

// Function declaration as a method in an object
const calculator = {
    add: function(a, b) {
        return a + b;
    }
};

console.log(calculator.add(5, 3));  // 8

// Recursive function declaration
function factorial(n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);
}

console.log(factorial(5));  // 120

// Function with dynamic number of arguments using the arguments object
function sum() {
    let total = 0;
    for (let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
}

console.log(sum(1, 2, 3, 4));  // 10

// Function declaration with nested function
function createMultiplier(factor) {
    function multiply(number) {
        return number * factor;
    }
    return multiply;
}

const double = createMultiplier(2);
console.log(double(5));  // 10

// Function declaration in conditionals (not recommended)
// This behavior can be inconsistent across browsers
if (true) {
    function conditionalFunc() {
        return "I'm defined in a block";
    }
}

// Using function declarations as event handlers
// document.getElementById("myButton").addEventListener("click", function handleClick() {
//     console.log("Button clicked!");
// });
```
## Function Expressions

**Q: What is a function expression and how does it differ from a function declaration?**

A: A **function expression** creates a function as part of an expression. Key differences from declarations:

- **Not hoisted**: Cannot be called before definition
- **Can be anonymous**: No required function name
- **Can be immediately invoked**: IIFE pattern
- **Conditional creation**: Can be created conditionally

**Q: What are the different types of function expressions?**

A: Function expressions come in several forms:

```javascript
// Named function expression
const namedExpr = function greet(name) {
    return "Hello, " + name + "!";
    // Can call greet() recursively within the function
};

// Anonymous function expression
const anonymousExpr = function(name) {
    return "Hi, " + name + "!";
};

// Immediately Invoked Function Expression (IIFE)
const result = (function(name) {
    return "Hey, " + name + "!";
})("John");

console.log(result); // "Hey, John!"

// Function expression with arrow function (ES6)
const arrowExpr = (name) => "Hello, " + name + "!";

// Function expression as object method
const obj = {
    greet: function(name) {
        return "Welcome, " + name + "!";
    }
};

// Function expression in callback
setTimeout(function() {
    console.log("Timer executed");
}, 1000);

// Conditional function creation
let condition = true;
let myFunc;

if (condition) {
    myFunc = function() { return "Condition was true"; };
} else {
    myFunc = function() { return "Condition was false"; };
}

console.log(myFunc()); // "Condition was true"
```

**Q: What is an IIFE and when would you use it?**

A: **Immediately Invoked Function Expression** - creates and executes a function immediately:

```javascript
// Basic IIFE syntax
(function() {
    console.log("I run immediately!");
})();

// IIFE with parameters
(function(name, age) {
    console.log(`Name: ${name}, Age: ${age}`);
})("Alice", 25);

// IIFE with return value
const calculated = (function(x, y) {
    return x * y + 10;
})(5, 3);

console.log(calculated); // 25

// Module pattern with IIFE
const myModule = (function() {
    let privateVar = 0;
    
    return {
        increment: function() {
            privateVar++;
            return privateVar;
        },
        decrement: function() {
            privateVar--;
            return privateVar;
        },
        getValue: function() {
            return privateVar;
        }
    };
})();

console.log(myModule.increment()); // 1
console.log(myModule.getValue());  // 1
// privateVar is not accessible from outside

// Use cases for IIFE:
// 1. Avoid global scope pollution
// 2. Create private variables
// 3. Initialize code that runs once
// 4. Module pattern implementation
```

## Arrow Functions (ES6)

**Q: What are arrow functions and what are their key features?**

A: **Arrow functions** provide concise syntax and lexical `this` binding:

- **Concise syntax**: Shorter than function expressions
- **Implicit return**: Single expressions return automatically
- **Lexical this**: Inherit `this` from enclosing scope
- **No arguments object**: Use rest parameters instead

**Q: What are the different arrow function syntax variations?**

A: Arrow functions have multiple syntax forms based on parameters and body:

```javascript
// No parameters - parentheses required
const getTimestamp = () => Date.now();

// Single parameter - parentheses optional
const square = x => x * x;
const squareExplicit = (x) => x * x;

// Multiple parameters - parentheses required
const add = (a, b) => a + b;

// Single expression - implicit return
const multiply = (a, b) => a * b;

// Multiple statements - explicit return required
const complexCalc = (a, b) => {
    const temp = a + b;
    const result = temp * 2;
    return result;
};

// Returning object literal - wrap in parentheses
const createUser = (name, age) => ({
    name: name,
    age: age,
    created: new Date()
});

// Array methods with arrow functions
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((acc, n) => acc + n, 0);

console.log(doubled); // [2, 4, 6, 8, 10]
console.log(evens);   // [2, 4]
console.log(sum);     // 15
```

**Q: How does 'this' binding work differently in arrow functions?**

A: Arrow functions **inherit `this`** from their enclosing scope - they don't create their own:

```javascript
// Traditional function vs arrow function 'this' binding
const person = {
    name: "Alice",
    
    // Traditional method - 'this' is person object
    regularGreet: function() {
        console.log("Regular:", this.name); // "Alice"
        
        // Problem: 'this' is lost in callback
        setTimeout(function() {
            console.log("Callback:", this.name); // undefined
        }, 100);
    },
    
    // Arrow function method - 'this' from global scope
    arrowGreet: () => {
        console.log("Arrow method:", this.name); // undefined
    },
    
    // Solution: Arrow function in callback
    fixedGreet: function() {
        console.log("Fixed:", this.name); // "Alice"
        
        // Arrow function inherits 'this' from fixedGreet
        setTimeout(() => {
            console.log("Arrow callback:", this.name); // "Alice"
        }, 100);
    }
};

// Class example
class Counter {
    constructor() {
        this.count = 0;
    }
    
    // Traditional method - 'this' can be lost
    incrementTraditional() {
        setTimeout(function() {
            this.count++; // 'this' is undefined or global
        }, 100);
    }
    
    // Arrow function preserves 'this'
    incrementArrow() {
        setTimeout(() => {
            this.count++; // 'this' is Counter instance
            console.log(this.count);
        }, 100);
    }
}
```

## Function Parameters and Arguments

**Q: What are the different ways to handle function parameters in JavaScript?**

A: JavaScript provides multiple parameter patterns for flexible function signatures:

**Q: How do default parameters work and what are best practices?**

A: **Default parameters** (ES6+) provide fallback values when arguments are undefined:

```javascript
// Basic default parameters
function greet(name = "Guest", greeting = "Hello") {
    return `${greeting}, ${name}!`;
}

console.log(greet());                    // "Hello, Guest!"
console.log(greet("Alice"));             // "Hello, Alice!"
console.log(greet("Bob", "Hi"));         // "Hi, Bob!"

// Default parameters with expressions
function createId(prefix = "user", timestamp = Date.now()) {
    return `${prefix}_${timestamp}`;
}

// Default parameters calling other functions
function getDefaultColor() {
    return Math.random() > 0.5 ? "blue" : "red";
}

function setTheme(color = getDefaultColor(), size = "medium") {
    return { color, size };
}

// Object destructuring with defaults
function createUser({ 
    name = "Anonymous", 
    age = 0, 
    role = "User",
    active = true 
} = {}) {
    return { name, age, role, active, created: new Date() };
}

console.log(createUser()); // All defaults
console.log(createUser({ name: "John", age: 25 })); // Partial defaults

// Array destructuring with defaults
function processCoordinates([x = 0, y = 0, z = 0] = []) {
    return { x, y, z };
}

console.log(processCoordinates([1, 2])); // { x: 1, y: 2, z: 0 }
console.log(processCoordinates());       // { x: 0, y: 0, z: 0 }

// Important: Only undefined triggers defaults
function testDefaults(value = "default") {
    return value;
}

console.log(testDefaults(undefined)); // "default"
console.log(testDefaults(null));      // null (not default!)
console.log(testDefaults(0));         // 0 (not default!)
console.log(testDefaults(""));        // "" (not default!)
```

**Q: How do rest parameters work and when should you use them?**

A: **Rest parameters** collect remaining arguments into an array:

```javascript
// Basic rest parameters
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4)); // 10
console.log(sum(5, 10));      // 15

// Rest with regular parameters
function introduce(greeting, ...names) {
    return `${greeting} ${names.join(", ")}!`;
}

console.log(introduce("Hello", "Alice", "Bob", "Charlie"));
// "Hello Alice, Bob, Charlie!"

// Rest in arrow functions
const multiply = (...args) => args.reduce((product, num) => product * num, 1);

// Rest with destructuring
function processData(first, second, ...rest) {
    console.log("First:", first);
    console.log("Second:", second);
    console.log("Rest:", rest);
}

processData(1, 2, 3, 4, 5);
// First: 1
// Second: 2
// Rest: [3, 4, 5]

// Real-world example: Flexible API function
function apiCall(endpoint, method = "GET", ...middlewares) {
    console.log(`${method} ${endpoint}`);
    middlewares.forEach(middleware => middleware());
}

apiCall("/users", "POST", 
    () => console.log("Auth middleware"),
    () => console.log("Logging middleware")
);
```

**Q: What is the arguments object and how does it compare to rest parameters?**

A: The **arguments object** is a legacy way to access all function arguments (not available in arrow functions):

```javascript
// Arguments object (legacy - avoid in modern code)
function oldStyleSum() {
    let total = 0;
    for (let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
}

// arguments is array-like but not a real array
function showArguments() {
    console.log(arguments);           // [Arguments] object
    console.log(Array.isArray(arguments)); // false
    
    // Convert to real array
    const argsArray = Array.from(arguments);
    // or
    const argsSpread = [...arguments];
}

// Modern alternative: rest parameters
function modernSum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

// Key differences:
// arguments object:
// - Available in all functions (except arrow functions)
// - Array-like object, not real array
// - Includes all arguments regardless of parameters
// - Legacy approach

// rest parameters:
// - Real array with all array methods
// - Only collects "rest" of arguments
// - Works with arrow functions
// - Modern, preferred approach

// Arrow functions DON'T have arguments object
const arrowFunc = () => {
    // console.log(arguments); // ReferenceError
    // Use rest parameters instead
};

const arrowFuncWithRest = (...args) => {
    console.log(args); // Real array
};
```

## Function Return Values

**Q: How do return statements work in JavaScript functions?**

A: **Return statements** determine what value a function produces:

- **Explicit return**: Use `return` keyword with value
- **Implicit return**: Functions return `undefined` by default
- **Early return**: Exit function before reaching end
- **Multiple returns**: Different values based on conditions

**Q: What are the different return patterns and best practices?**

A: Functions can return various types and use different return strategies:

```javascript
// Explicit return with value
function add(a, b) {
    return a + b;
}

// Function without return statement (returns undefined)
function logMessage(message) {
    console.log(message);
    // implicit: return undefined;
}

console.log(logMessage("Hello")); // undefined

// Early return pattern (guard clauses)
function divide(a, b) {
    if (b === 0) {
        return "Cannot divide by zero";
    }
    
    if (typeof a !== "number" || typeof b !== "number") {
        return "Both arguments must be numbers";
    }
    
    return a / b;
}

// Multiple return points based on conditions
function getGrade(score) {
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
}

// Returning different data types
function processUser(user) {
    if (!user) {
        return null; // Return null for invalid input
    }
    
    if (user.active) {
        return {
            id: user.id,
            name: user.name,
            status: "active"
        }; // Return object
    }
    
    return false; // Return boolean
}

// Returning functions (higher-order functions)
function createMultiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = createMultiplier(2);
console.log(double(5)); // 10

// Returning promises for async operations
function fetchData(url) {
    return fetch(url)
        .then(response => response.json())
        .catch(error => {
            console.error("Error:", error);
            return null;
        });
}

// Arrow function implicit return
const square = x => x * x; // Implicit return
const getUser = id => ({ id, name: `User ${id}` }); // Return object

// Complex return with validation
function createUser(name, email) {
    // Validation
    if (!name || !email) {
        return {
            success: false,
            error: "Name and email are required"
        };
    }
    
    if (!email.includes("@")) {
        return {
            success: false,
            error: "Invalid email format"
        };
    }
    
    // Success case
    return {
        success: true,
        user: {
            id: Date.now(),
            name: name.trim(),
            email: email.toLowerCase(),
            created: new Date()
        }
    };
}

const result = createUser("John", "john@example.com");
if (result.success) {
    console.log("User created:", result.user);
} else {
    console.error("Error:", result.error);
}
```

## Function Scope and Closures

**Q: How do functions create scope and what variables can they access?**

A: Functions create their own **execution context** with access to:

- **Local variables**: Declared within the function
- **Parameters**: Function arguments
- **Outer scope**: Variables from containing functions/global scope
- **Global variables**: Accessible from anywhere

**Q: What are closures and how do functions use them?**

A: **Closures** allow functions to access variables from their outer scope even after the outer function has finished:

```javascript
// Basic closure example
function outerFunction(x) {
    // This variable is in the outer scope
    let outerVariable = x;
    
    // Inner function has access to outer variable
    function innerFunction(y) {
        return outerVariable + y; // Closure over outerVariable
    }
    
    return innerFunction;
}

const addFive = outerFunction(5);
console.log(addFive(3)); // 8 (5 + 3)

// Counter example - private variables
function createCounter() {
    let count = 0; // Private variable
    
    return {
        increment: function() {
            count++;
            return count;
        },
        decrement: function() {
            count--;
            return count;
        },
        getValue: function() {
            return count;
        }
    };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getValue());  // 2
// count is not directly accessible

// Function factory with closures
function createGreeter(greeting) {
    return function(name) {
        return `${greeting}, ${name}!`;
    };
}

const sayHello = createGreeter("Hello");
const sayHi = createGreeter("Hi");

console.log(sayHello("Alice")); // "Hello, Alice!"
console.log(sayHi("Bob"));      // "Hi, Bob!"

// Practical closure example: Configuration
function createAPIClient(baseURL, apiKey) {
    return {
        get: function(endpoint) {
            return `GET ${baseURL}${endpoint} with key ${apiKey}`;
        },
        post: function(endpoint, data) {
            return `POST ${baseURL}${endpoint} with ${JSON.stringify(data)} and key ${apiKey}`;
        }
    };
}

const client = createAPIClient("https://api.example.com", "secret123");
console.log(client.get("/users")); // "GET https://api.example.com/users with key secret123"

// Loop closure problem and solution
function createButtonHandlers() {
    const buttons = [];
    
    // Problem with var
    for (var i = 0; i < 3; i++) {
        buttons.push(function() {
            console.log("Button", i); // i will be 3 for all
        });
    }
    
    // Solution with let (creates new binding each iteration)
    for (let j = 0; j < 3; j++) {
        buttons.push(function() {
            console.log("Button", j); // Each j is separate
        });
    }
    
    return buttons;
}
````markdown
## Higher-Order Functions

**Q: What are higher-order functions and why are they important?**

A: **Higher-order functions** are functions that either:

- **Take functions as arguments** (callbacks, event handlers)
- **Return functions as results** (function factories, currying)
- **Both** (function composition, decorators)

They enable **functional programming patterns** and code reusability.

**Q: How do you use functions as arguments (callbacks)?**

A: Pass functions to other functions to customize behavior:

```javascript
// Basic callback example
function processArray(arr, callback) {
    const result = [];
    for (let i = 0; i < arr.length; i++) {
        result.push(callback(arr[i], i));
    }
    return result;
}

const numbers = [1, 2, 3, 4, 5];

// Different callbacks for different behavior
const doubled = processArray(numbers, x => x * 2);
const indexed = processArray(numbers, (x, i) => `${i}: ${x}`);
const squared = processArray(numbers, x => x * x);

console.log(doubled); // [2, 4, 6, 8, 10]
console.log(indexed); // ["0: 1", "1: 2", "2: 3", "3: 4", "4: 5"]
console.log(squared); // [1, 4, 9, 16, 25]

// Built-in higher-order functions
const evenNumbers = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((acc, n) => acc + n, 0);
const strings = numbers.map(n => `Number: ${n}`);

// Event handling with callbacks
function setupButton(selector, callback) {
    const button = document.querySelector(selector);
    if (button) {
        button.addEventListener('click', callback);
    }
}

setupButton('#myButton', function() {
    console.log('Button clicked!');
});

// Async callbacks
function fetchData(url, onSuccess, onError) {
    fetch(url)
        .then(response => response.json())
        .then(onSuccess)
        .catch(onError);
}

fetchData('/api/users', 
    users => console.log('Users:', users),
    error => console.error('Error:', error)
);
```

**Q: How do you create function factories that return customized functions?**

A: Use closures to create specialized functions with preset configurations:

```javascript
// Function factory for validators
function createValidator(type) {
    switch (type) {
        case 'email':
            return function(value) {
                return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
            };
        case 'phone':
            return function(value) {
                return /^\d{10}$/.test(value.replace(/\D/g, ''));
            };
        case 'required':
            return function(value) {
                return value != null && value.toString().trim() !== '';
            };
        default:
            return function() { return true; };
    }
}

const emailValidator = createValidator('email');
const phoneValidator = createValidator('phone');

console.log(emailValidator('user@example.com')); // true
console.log(phoneValidator('555-123-4567'));     // true

// Function factory for API endpoints
function createEndpoint(baseURL) {
    return function(path, options = {}) {
        const url = `${baseURL}${path}`;
        const config = {
            method: 'GET',
            headers: { 'Content-Type': 'application/json' },
            ...options
        };
        return fetch(url, config);
    };
}

const apiCall = createEndpoint('https://api.example.com');
apiCall('/users').then(response => response.json());
apiCall('/posts', { method: 'POST', body: JSON.stringify({title: 'New Post'}) });

// Function factory for calculations
function createCalculator(operation) {
    const operations = {
        add: (a, b) => a + b,
        subtract: (a, b) => a - b,
        multiply: (a, b) => a * b,
        divide: (a, b) => b !== 0 ? a / b : "Cannot divide by zero"
    };
    
    return operations[operation] || (() => "Invalid operation");
}

const adder = createCalculator('add');
const multiplier = createCalculator('multiply');

console.log(adder(5, 3));      // 8
console.log(multiplier(4, 6)); // 24
```

**Q: What is function composition and how do you implement it?**

A: **Function composition** combines multiple functions to create new functionality:

```javascript
// Basic composition - right to left execution
function compose(f, g) {
    return function(x) {
        return f(g(x));
    };
}

// Example functions
const addOne = x => x + 1;
const double = x => x * 2;
const square = x => x * x;

// Compose functions
const addOneThenDouble = compose(double, addOne);
const doubleThenSquare = compose(square, double);

console.log(addOneThenDouble(3)); // (3 + 1) * 2 = 8
console.log(doubleThenSquare(3)); // (3 * 2)² = 36

// Multi-function composition
function composeMany(...functions) {
    return function(value) {
        return functions.reduceRight((acc, fn) => fn(acc), value);
    };
}

const transform = composeMany(square, double, addOne);
console.log(transform(3)); // ((3 + 1) * 2)² = 64

// Pipe (left to right composition)
function pipe(...functions) {
    return function(value) {
        return functions.reduce((acc, fn) => fn(acc), value);
    };
}

const pipeline = pipe(addOne, double, square);
console.log(pipeline(3)); // Same result as compose but clearer reading

// Real-world example: Data processing pipeline
const users = [
    { name: 'john doe', age: 25, active: true },
    { name: 'jane smith', age: 17, active: false },
    { name: 'bob johnson', age: 30, active: true }
];

const capitalize = str => str.replace(/\b\w/g, l => l.toUpperCase());
const isAdult = user => user.age >= 18;
const isActive = user => user.active;

const processUsers = pipe(
    users => users.filter(isAdult),
    users => users.filter(isActive),
    users => users.map(user => ({
        ...user,
        name: capitalize(user.name)
    }))
);

console.log(processUsers(users));
// [{ name: 'John Doe', age: 25, active: true }, 
//  { name: 'Bob Johnson', age: 30, active: true }]
```

**Q: What is currying and how does it work with higher-order functions?**

A: **Currying** transforms a function with multiple arguments into a series of functions with single arguments:

```javascript
// Basic currying example
function curry(fn) {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn.apply(this, args);
        } else {
            return function(...nextArgs) {
                return curried(...args, ...nextArgs);
            };
        }
    };
}

// Regular function
function add(a, b, c) {
    return a + b + c;
}

// Curried version
const curriedAdd = curry(add);

// Can be called in multiple ways
console.log(curriedAdd(1)(2)(3));    // 6
console.log(curriedAdd(1, 2)(3));    // 6
console.log(curriedAdd(1)(2, 3));    // 6
console.log(curriedAdd(1, 2, 3));    // 6

// Practical currying examples
const multiply = curry((a, b, c) => a * b * c);
const double = multiply(2);      // Partial application
const quadruple = double(2);     // Further partial application

console.log(quadruple(5)); // 2 * 2 * 5 = 20

// Curried validation
const createValidator = curry((minLength, pattern, value) => {
    return value.length >= minLength && pattern.test(value);
});

const emailValidator = createValidator(5)(/^[^\s@]+@[^\s@]+\.[^\s@]+$/);
const strongPasswordValidator = createValidator(8)(/^(?=.*[A-Za-z])(?=.*\d)/);

console.log(emailValidator('user@example.com')); // true
console.log(strongPasswordValidator('password123')); // true

// Curried array operations
const map = curry((fn, arr) => arr.map(fn));
const filter = curry((predicate, arr) => arr.filter(predicate));

const numbers = [1, 2, 3, 4, 5];
const doubleAll = map(x => x * 2);
const getEvens = filter(x => x % 2 === 0);

console.log(doubleAll(numbers)); // [2, 4, 6, 8, 10]
console.log(getEvens(numbers));  // [2, 4]

// Function composition with currying
const processNumbers = pipe(
    doubleAll,
    getEvens,
    map(x => x + 1)
);

console.log(processNumbers([1, 2, 3, 4, 5])); // [5, 9]
```