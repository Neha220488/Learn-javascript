# Closures, Scope, and the 'this' Keyword in JavaScript

# Closures, Scope, and the 'this' Keyword in JavaScript

## JavaScript Scope

**Q: What is scope and what are the different types in JavaScript?**

A: **Scope** determines where variables can be accessed. Four main types:

1. 🌍 **Global Scope**: Accessible everywhere
2. 🏠 **Function Scope**: Only inside the function (var)
3. 📦 **Block Scope**: Only inside {} blocks (let/const)
4. 📄 **Module Scope**: Only within the module file

```javascript
// Global scope
const globalVar = "I'm everywhere!";

function exampleFunction() {
    // Function scope
    var functionScoped = "Only in this function";
    
    if (true) {
        // Block scope
        let blockScoped = "Only in this block";
        const alsoBlockScoped = "Me too!";
        var notReallyBlockScoped = "I'm function-scoped!";
        
        console.log(globalVar);        // ✅ Works
        console.log(functionScoped);   // ✅ Works
        console.log(blockScoped);      // ✅ Works
    }
    
    console.log(notReallyBlockScoped); // ✅ Works (var ignores blocks)
    // console.log(blockScoped);       // ❌ ReferenceError
}

// console.log(functionScoped);       // ❌ ReferenceError
```

**Q: What's the difference between var, let, and const regarding scope?**

A: 
- **var**: Function-scoped (ignores blocks) + hoisted
- **let**: Block-scoped + not accessible before declaration
- **const**: Block-scoped + can't be reassigned

```javascript
function scopeExample() {
    console.log(varVariable);    // undefined (hoisted)
    // console.log(letVariable); // ❌ ReferenceError (temporal dead zone)
    
    var varVariable = "I'm hoisted";
    let letVariable = "I'm block-scoped";
    const constVariable = "I'm also block-scoped and immutable";
    
    if (true) {
        var varInBlock = "I escape the block!";
        let letInBlock = "I stay in the block";
        const constInBlock = "Me too!";
    }
    
    console.log(varInBlock);     // ✅ "I escape the block!"
    // console.log(letInBlock);  // ❌ ReferenceError
    // console.log(constInBlock);// ❌ ReferenceError
}
```

**💡 Memory Tip:** 
- **var** = **V**ery loose (function scope) 🏠
- **let** = **L**imited to block 📦
- **const** = **C**an't change + block scope 🔒
    const outerVar = "I'm in the outer function";
    
    function inner() {
        const innerVar = "I'm in the inner function";
        console.log(outerVar);  // Accessible: "I'm in the outer function"
    }
    
    inner();
    // console.log(innerVar);  // ReferenceError: innerVar is not defined
}

// Scope chain
function grandparent() {
    const a = "a";
    
    function parent() {
        const b = "b";
        
        function child() {
            const c = "c";
            console.log(a, b, c);  // Accessible: "a b c"
        }
        
        child();
    }
    
    parent();
}

grandparent();

// Block scope with let and const
{
    var varInBlock = "I'm accessible outside the block";
    let letInBlock = "I'm confined to the block";
    const constInBlock = "I'm also confined to the block";
}

console.log(varInBlock);     // Accessible: "I'm accessible outside the block"
// console.log(letInBlock);    // ReferenceError: letInBlock is not defined
// console.log(constInBlock);  // ReferenceError: constInBlock is not defined

// Loop variable scope
for (var i = 0; i < 3; i++) {
    // i is function-scoped
}
console.log(i);  // Accessible: 3

for (let j = 0; j < 3; j++) {
    // j is block-scoped
}
// console.log(j);  // ReferenceError: j is not defined

// Temporal Dead Zone (TDZ)
// console.log(tdz);  // ReferenceError: Cannot access 'tdz' before initialization
let tdz = "Variables declared with let/const exist in TDZ before declaration";

// Global object property (with var) vs. truly global variable (with let/const)
var globalWithVar = "I'm a property of the global object";
let globalWithLet = "I'm not a property of the global object";

console.log(window.globalWithVar);  // "I'm a property of the global object" (in browsers)
console.log(window.globalWithLet);  // undefined

// Function declarations vs. function expressions
function declarationFunction() {
    return "I'm hoisted";
}

// functionExpression();  // TypeError: functionExpression is not a function
const functionExpression = function() {
    return "I'm not hoisted";
};
```

## Variable Hoisting

**Q: What is hoisting and how does it behave differently for var, let, const, and functions?**

A: **Hoisting** moves declarations to the top during compilation. Different behaviors:

| Type | Hoisted? | Initialized? | Accessible before declaration? |
|------|----------|--------------|-------------------------------|
| **var** | ✅ Yes | ✅ undefined | ✅ Yes (undefined) |
| **let/const** | ✅ Yes | ❌ No | ❌ No (TDZ) |
| **function declaration** | ✅ Yes | ✅ Full function | ✅ Yes (works) |
| **function expression** | Follows var/let rules | ❌ No | ❌ No |

```javascript
// Function declarations - fully hoisted
console.log(myFunction());  // ✅ "I work!" - called before declaration

function myFunction() {
    return "I work!";
}

// var - declaration hoisted, initialization stays
console.log(myVar);  // ✅ undefined (not error)
var myVar = "I'm assigned later";
console.log(myVar);  // ✅ "I'm assigned later"

// let/const - Temporal Dead Zone (TDZ)
// console.log(myLet);     // ❌ ReferenceError - in TDZ
// console.log(myConst);   // ❌ ReferenceError - in TDZ
let myLet = "Now I exist";
const myConst = "Me too";
```

**Q: What is the Temporal Dead Zone (TDZ)?**

A: The **time between when a let/const variable is hoisted and when it's declared**. Variable exists but is inaccessible:

```javascript
console.log("Starting function");

// TDZ starts here for 'temporalVar'
// console.log(temporalVar);  // ❌ ReferenceError

// Still in TDZ
if (true) {
    // console.log(temporalVar);  // ❌ Still ReferenceError
    
    let temporalVar = "Now I'm born!";  // TDZ ends here
    console.log(temporalVar);  // ✅ "Now I'm born!"
}
```

**Q: How does hoisting affect variable shadowing?**

A: **Local declarations are hoisted**, creating unexpected behavior with var:

```javascript
var shadowVar = "I'm global";

function shadowExample() {
    console.log(shadowVar);  // ❌ undefined (not "I'm global"!)
    // Why? Because var shadowVar below is hoisted as undefined
    
    var shadowVar = "I'm local";  // This declaration was hoisted
    console.log(shadowVar);  // ✅ "I'm local"
}

shadowExample();

// What actually happens:
function shadowExample() {
    var shadowVar;  // ← Hoisted declaration
    console.log(shadowVar);  // undefined
    shadowVar = "I'm local";  // Assignment stays here
    console.log(shadowVar);  // "I'm local"
}
```

**💡 Memory Tip:** "Hoisting = Declarations rise to the top like hot air balloons 🎈"
```

## Closures

**Q: What is a closure and why is it useful?**

A: A **closure** is when a function **remembers variables** from where it was created, even after leaving that scope:

```javascript
function createCounter() {
    let count = 0;  // Private variable
    
    return function() {  // This function "closes over" count
        count++;
        return count;
    };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1());  // 1
console.log(counter1());  // 2
console.log(counter2());  // 1 (separate counter!)

// count is not accessible from outside
// console.log(count);  // ❌ ReferenceError
```

**Q: How do closures create "private" variables in JavaScript?**

A: Variables in the outer function are **private** - only accessible through the returned function:

```javascript
function createBankAccount(initialBalance) {
    let balance = initialBalance;  // Private variable
    
    return {
        deposit(amount) {
            balance += amount;
            return balance;
        },
        
        withdraw(amount) {
            if (amount <= balance) {
                balance -= amount;
                return balance;
            }
            return "Insufficient funds";
        },
        
        getBalance() {
            return balance;  // Only way to read balance
        }
        
        // No direct access to balance variable!
    };
}

const account = createBankAccount(100);
console.log(account.deposit(50));    // 150
console.log(account.withdraw(30));   // 120
console.log(account.getBalance());   // 120

// Can't access balance directly
// console.log(account.balance);     // undefined
// balance is truly private!
```

**Q: What's a common mistake with closures in loops?**

A: **All functions sharing the same variable** instead of each having their own:

```javascript
// ❌ Common mistake - all functions log 3
const functions = [];
for (var i = 0; i < 3; i++) {
    functions.push(function() {
        console.log(i);  // All reference the same 'i' (which becomes 3)
    });
}

functions[0]();  // 3 (not 0!)
functions[1]();  // 3 (not 1!)
functions[2]();  // 3 (not 2!)

// ✅ Solution 1: Use let (block-scoped)
const functions2 = [];
for (let i = 0; i < 3; i++) {  // let creates new 'i' each iteration
    functions2.push(function() {
        console.log(i);
    });
}

functions2[0]();  // 0 ✅
functions2[1]();  // 1 ✅
functions2[2]();  // 2 ✅

// ✅ Solution 2: IIFE (Immediately Invoked Function Expression)
const functions3 = [];
for (var i = 0; i < 3; i++) {
    functions3.push((function(index) {  // Create new scope
        return function() {
            console.log(index);  // Each function has its own 'index'
        };
    })(i));  // Pass current 'i' value
}
```

**Q: How do you use closures for function factories?**

A: Create **specialized functions** with preset configurations:

```javascript
function createMultiplier(multiplier) {
    return function(number) {
        return number * multiplier;  // Remembers multiplier
    };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);
const quadruple = createMultiplier(4);

console.log(double(5));     // 10
console.log(triple(5));     // 15
console.log(quadruple(5));  // 20

// Real-world example: API endpoints
function createAPIEndpoint(baseURL) {
    return function(endpoint) {
        return `${baseURL}${endpoint}`;  // Remembers baseURL
    };
}

const userAPI = createAPIEndpoint('https://api.example.com/users/');
const productAPI = createAPIEndpoint('https://api.example.com/products/');

console.log(userAPI('123'));        // 'https://api.example.com/users/123'
console.log(productAPI('widget'));  // 'https://api.example.com/products/widget'
```

**Q: How do you implement partial application and currying with closures?**

A: **Partial application** pre-fills some arguments, **currying** breaks functions into single-argument chains:

```javascript
// Partial Application - preset some arguments
function createLogger(level) {
    return function(message) {
        const timestamp = new Date().toISOString();
        console.log(`[${timestamp}] ${level.toUpperCase()}: ${message}`);
    };
}

const errorLogger = createLogger('error');
const infoLogger = createLogger('info');

errorLogger('Something went wrong!');  // [2025-06-01T...] ERROR: Something went wrong!
infoLogger('Process completed');       // [2025-06-01T...] INFO: Process completed

// Currying - transform f(a,b,c) into f(a)(b)(c)
function curry(fn) {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn.apply(this, args);
        }
        return function(...nextArgs) {
            return curried.apply(this, [...args, ...nextArgs]);
        };
    };
}

// Example: curried math operations
const curriedAdd = curry((a, b, c) => a + b + c);
console.log(curriedAdd(1)(2)(3));     // 6
console.log(curriedAdd(1, 2)(3));     // 6
```

**💡 Memory Tip:** "Closure = Function that CLOSES over variables and remembers them 🧠"

// Closures for data privacy (private variables)
function createPerson(name) {
    // name is private, accessible only through the returned methods
    return {
        getName: function() {
            return name;
        },
        setName: function(newName) {
            name = newName;
        }
    };
}

const person = createPerson("John");
console.log(person.getName());  // "John"
person.setName("Jane");
console.log(person.getName());  // "Jane"
// console.log(person.name);    // undefined (name is not directly accessible)

// The module pattern (using closures for encapsulation)
const calculator = (function() {
    // Private variables and functions
    let result = 0;
    
    function add(a, b) {
        return a + b;
    }
    
    function multiply(a, b) {
        return a * b;
    }
    
    // Public API
    return {
        add: function(value) {
            result = add(result, value);
            return result;
        },
        multiply: function(value) {
            result = multiply(result, value);
            return result;
        },
        getResult: function() {
            return result;
        },
        reset: function() {
            result = 0;
            return result;
        }
    };
})();

console.log(calculator.add(5));      // 5
console.log(calculator.multiply(2));  // 10
console.log(calculator.getResult());   // 10
console.log(calculator.reset());      // 0

// Closures with event handlers
function setupButton(buttonId, message) {
    const button = document.getElementById(buttonId);
    
    button.addEventListener("click", function() {
        // This function forms a closure over the message parameter
        alert(message);
    });
}

// setupButton("button1", "Hello from Button 1");
// setupButton("button2", "Greetings from Button 2");

// The famous loop closure problem
function createButtons() {
    for (var i = 0; i < 3; i++) {
        const button = document.createElement("button");
        button.textContent = "Button " + i;
        
        // Problem: all buttons will show "3" because i is function-scoped
        button.addEventListener("click", function() {
            alert("Button " + i + " clicked");  // i will be 3 for all buttons
        });
        
        document.body.appendChild(button);
    }
}

// Solutions to the loop closure problem:
// 1. Using an IIFE to create a new scope
function createButtonsWithIIFE() {
    for (var i = 0; i < 3; i++) {
        (function(index) {
            const button = document.createElement("button");
            button.textContent = "Button " + index;
            
            button.addEventListener("click", function() {
                alert("Button " + index + " clicked");  // Correct index for each button
            });
            
            document.body.appendChild(button);
        })(i);
    }
}

// 2. Using let for block-scoping (ES6)
function createButtonsWithLet() {
    for (let i = 0; i < 3; i++) {
        // i is block-scoped, each iteration gets its own i
        const button = document.createElement("button");
        button.textContent = "Button " + i;
        
        button.addEventListener("click", function() {
            alert("Button " + i + " clicked");  // Correct index for each button
        });
        
        document.body.appendChild(button);
    }
}

// Closures with setTimeout
function delayedGreetings() {
    for (let i = 1; i <= 3; i++) {
        setTimeout(function() {
            console.log("Greeting " + i);
        }, i * 1000);
    }
}

delayedGreetings();
// After 1 second: "Greeting 1"
// After 2 seconds: "Greeting 2"
// After 3 seconds: "Greeting 3"

// Closures with higher-order functions
function multiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5));  // 10
console.log(triple(5));  // 15

// Closures for memoization (caching function results)
function createMemoizedFunction(fn) {
    const cache = {};
    
    return function(...args) {
        const key = JSON.stringify(args);
        
        if (key in cache) {
            console.log("Returning cached result for", args);
            return cache[key];
        }
        
        const result = fn(...args);
        cache[key] = result;
        return result;
    };
}

const expensiveCalculation = function(n) {
    console.log("Performing expensive calculation for", n);
    let result = 0;
    for (let i = 0; i < n * 1000; i++) {
        result += Math.random();
    }
    return result;
};

const memoizedCalculation = createMemoizedFunction(expensiveCalculation);

console.log(memoizedCalculation(2));  // Performs calculation
console.log(memoizedCalculation(2));  // Returns cached result
console.log(memoizedCalculation(3));  // Performs calculation
console.log(memoizedCalculation(3));  // Returns cached result
````

## The 'this' Keyword

**Q: What determines the value of 'this' in JavaScript?**

A: **How the function is called**, not where it's defined. Five main rules:

1. 🌍 **Global context**: `this` = global object (window/global)
2. 📞 **Method call**: `this` = the object before the dot
3. 🏗️ **Constructor**: `this` = new instance being created
4. 🎯 **Explicit binding**: `this` = what you set with call/apply/bind
5. ➡️ **Arrow functions**: `this` = inherited from parent scope

```javascript
// Rule 1: Global context
console.log(this);  // Window (browser) or global (Node.js)

function globalFunc() {
    console.log(this);  // Window (non-strict) or undefined (strict)
}

// Rule 2: Method call (object before the dot)
const person = {
    name: "John",
    greet() {
        console.log(this.name);  // "John" (this = person)
    }
};
person.greet();  // "John" ← person is before the dot

// Rule 3: Constructor
function Person(name) {
    this.name = name;  // this = new instance
}
const john = new Person("John");

// Rule 4: Explicit binding
const jane = { name: "Jane" };
person.greet.call(jane);  // "Jane" (this explicitly set to jane)
```

**Q: What's the most common 'this' mistake and how do you fix it?**

A: **Losing `this` in callbacks**. Arrow functions are the modern solution:

```javascript
const user = {
    name: "Bob",
    
    // ❌ Problem: this is lost in callback
    greetLater() {
        setTimeout(function() {
            console.log("Hello " + this.name);  // undefined - this is lost!
        }, 1000);
    },
    
    // ✅ Solution 1: Arrow function (inherits this)
    greetLaterFixed() {
        setTimeout(() => {
            console.log("Hello " + this.name);  // "Bob" - this preserved!
        }, 1000);
    },
    
    // ✅ Solution 2: Store this in variable
    greetLaterOldWay() {
        const self = this;
        setTimeout(function() {
            console.log("Hello " + self.name);  // "Bob"
        }, 1000);
    },
    
    // ✅ Solution 3: Bind this
    greetLaterBind() {
        setTimeout(function() {
            console.log("Hello " + this.name);  // "Bob"
        }.bind(this), 1000);
    }
};
```

**Q: How does 'this' behave differently in arrow functions?**

A: Arrow functions **DON'T have their own `this`** - they inherit from parent scope:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**Q: How do call(), apply(), and bind() control 'this'?**

A: They **explicitly set** what `this` should be:

- **call()**: Calls function immediately with specific `this`
- **apply()**: Same as call but takes array of arguments  
- **bind()**: Returns new function with fixed `this`

```javascript
function introduce(greeting, punctuation) {
    console.log(`${greeting}, I'm ${this.name}${punctuation}`);
}

const person1 = { name: "John" };
const person2 = { name: "Jane" };

// call - immediate execution
introduce.call(person1, "Hello", "!");     // "Hello, I'm John!"
introduce.call(person2, "Hi", ".");        // "Hi, I'm Jane."

// apply - immediate execution with array
introduce.apply(person1, ["Greetings", "!!!"]);  // "Greetings, I'm John!!!"

// bind - returns new function
const johnIntroduce = introduce.bind(person1);
johnIntroduce("Hey", "?");  // "Hey, I'm John?"

// Useful for fixing 'this' in event handlers
class Button {
    constructor(element) {
        this.element = element;
        this.clickCount = 0;
        
        // Fix 'this' with bind
        this.element.addEventListener('click', this.handleClick.bind(this));
    }
    
    handleClick() {
        this.clickCount++;  // 'this' refers to Button instance
        console.log(`Clicked ${this.clickCount} times`);
    }
}
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain
        console.log(this);       // Depends on how function was called
    }
    
    innerFunction();
}

// Method call changes 'this' binding
const obj = {
    name: "My Object",
    method: function() {
        // Function Execution Context created
        // - this: obj (the object before the dot)
        console.log("Method context, this.name:", this.name);
    }
};

outerFunction();  // this = Window
obj.method();     // this = obj
```

**Q: How do arrow functions affect execution context?**

A: Arrow functions **don't create their own context** - they inherit `this` from parent:

```javascript
const obj = {
    name: "Alice",
    
    // Regular method has its own 'this'
    regularMethod() {
        console.log("Regular:", this.name);  // ✅ "Alice"
    },
    
    // Arrow method inherits 'this' from global scope
    arrowMethod: () => {
        console.log("Arrow:", this.name);    // ❌ undefined (global this)
    },
    
    // Useful for callbacks
    setupTimer() {
        // this = obj here
        setTimeout(() => {
            console.log("Timer:", this.name);  // ✅ "Alice" (inherited)
        }, 1000);
    }
};

obj.regularMethod();  // "Alice"
obj.arrowMethod();    // undefined
obj.setupTimer();     // "Alice" after 1 second
```

**💡 Memory Tip:** 
- **CALL** = **C**all immediately with **C**ommas for arguments 📞
- **APPLY** = **A**pply immediately with **A**rray of arguments 🎯  
- **BIND** = **B**ind and get new function back 🔗
    }
}

const instance = new MyClass("Instance name");
instance.regularMethod();  // "Regular method: Instance name"
instance.arrowMethod();    // "Arrow method: Instance name"
instance.problematicMethod();  // "Inner function: undefined"

// this in nested objects
const outer = {
    name: "outer",
    inner: {
        name: "inner",
        method: function() {
            console.log("Method:", this.name);  // "inner"
        }
    }
};

outer.inner.method();  // "Method: inner"

// Common pitfall: Losing this in callbacks
function callWithCallback(callback) {
    callback();
}

const obj = {
    name: "My Object",
    method: function() {
        console.log(this.name);
    }
};

// callWithCallback(obj.method);  // undefined, this is lost

// Solution: Use bind
callWithCallback(obj.method.bind(obj));  // "My Object"

// Function borrowing with call/apply
const person1 = {
    name: "Person 1",
    greet: function(greeting, punctuation) {
        return greeting + ", I'm " + this.name + punctuation;
    }
};

const person2 = {
    name: "Person 2"
};

// Borrowing person1's method for person2
console.log(person1.greet.call(person2, "Hi", "!"));  // "Hi, I'm Person 2!"
console.log(person1.greet.apply(person2, ["Hello", "."]));  // "Hello, I'm Person 2."
```

## Lexical Environment

**Q: What is a Lexical Environment and how does it relate to closures?**

A: A **Lexical Environment** is JavaScript's internal mechanism that tracks **where variables can be accessed**. It has two parts:

1. 📋 **Environment Record**: Stores variables and functions in current scope
2. 🔗 **Outer Reference**: Points to parent scope's lexical environment

```javascript
// Each function creates its own lexical environment
function outer() {
    // Outer Lexical Environment
    // - Environment Record: { outerVar, inner }
    // - Outer Reference: → Global Environment
    
    const outerVar = "I'm outer";
    
    function inner() {
        // Inner Lexical Environment  
        // - Environment Record: { innerVar }
        // - Outer Reference: → Outer Environment
        
        const innerVar = "I'm inner";
        console.log(outerVar);  // Found via outer reference chain
        console.log(innerVar);  // Found in current environment
    }
    
    return inner;
}

const closure = outer();  // outer() finishes, but environment persists!
closure();  // Still has access to outerVar through lexical environment
```

**Q: How do lexical environments create the scope chain?**

A: Through the **outer reference chain** - JavaScript searches environments from inner to outer:

```javascript
const global = "global scope";

function level1() {
    const var1 = "level 1";
    
    function level2() {
        const var2 = "level 2";
        
        function level3() {
            const var3 = "level 3";
            
            // Variable lookup follows the scope chain:
            console.log(var3);   // 1. Current environment ✅
            console.log(var2);   // 2. Level2 environment ✅
            console.log(var1);   // 3. Level1 environment ✅
            console.log(global); // 4. Global environment ✅
            // console.log(unknown); // ❌ ReferenceError
        }
        
        return level3;
    }
    
    return level2();
}

const deepClosure = level1();
deepClosure();  // All variables still accessible!
```

**Q: How do closures explain the classic loop closure problem?**

A: **var** creates one shared environment, **let** creates separate environments per iteration:

```javascript
// ❌ Problem: var shares lexical environment
function createFunctionsWithVar() {
    const functions = [];
    
    for (var i = 0; i < 3; i++) {
        // All functions share the SAME lexical environment
        // where i will eventually be 3
        functions.push(function() {
            console.log(i);  // All reference the same 'i'
        });
    }
    
    return functions;
}

const badFunctions = createFunctionsWithVar();
badFunctions[0]();  // 3 (not 0!)
badFunctions[1]();  // 3 (not 1!)

// ✅ Solution: let creates new environment each iteration
function createFunctionsWithLet() {
    const functions = [];
    
    for (let i = 0; i < 3; i++) {
        // Each iteration gets its own lexical environment
        // with its own copy of 'i'
        functions.push(function() {
            console.log(i);  // Each references different 'i'
        });
    }
    
    return functions;
}

const goodFunctions = createFunctionsWithLet();
goodFunctions[0]();  // 0 ✅
goodFunctions[1]();  // 1 ✅
goodFunctions[2]();  // 2 ✅
```

**💡 Memory Tip:** "Lexical = Where you **LEX**ically (physically) write the code determines the scope!" 📝

## Execution Context

**Q: What is an Execution Context and when is it created?**

A: An **Execution Context** is the environment where JavaScript code runs. Created when:

1. 🌍 **Global Context**: When script starts
2. 📞 **Function Context**: When function is called  
3. 🔍 **Eval Context**: When eval() executes

Each context contains:
- 📋 **Variable Environment**: Where variables are stored
- 🔗 **Lexical Environment**: For scope resolution
- 🎯 **this Binding**: What `this` points to

**Q: How does the Call Stack manage execution contexts?**

A: **LIFO (Last In, First Out)** - newest context on top, removed when function finishes:

```javascript
function first() {
    console.log("In first() - Context 1");
    second();
    console.log("Back in first()");
}

function second() {
    console.log("In second() - Context 2"); 
    third();
    console.log("Back in second()");
}

function third() {
    console.log("In third() - Context 3");
}

// Call Stack visualization:
first();
/*
Call Stack progression:
1. [Global Context]
2. [first(), Global Context]  
3. [second(), first(), Global Context]
4. [third(), second(), first(), Global Context]
5. [second(), first(), Global Context]  // third() removed
6. [first(), Global Context]           // second() removed  
7. [Global Context]                    // first() removed
*/
```

**Q: How do execution contexts affect variable access and 'this' binding?**

A: Each context determines **what variables are accessible** and **what `this` means**:

```javascript
const globalVar = "I'm global";

function outerFunction() {
    // Function Execution Context created
    // - Variable Environment: { outerVar, innerFunction }
    // - this: Window (or undefined in strict mode)
    const outerVar = "I'm outer";
    
    function innerFunction() {
        // New Function Execution Context created
        // - Variable Environment: { innerVar }
        // - Lexical Environment: Points to outerFunction's context
        // - this: Window (or undefined in strict mode)
        const innerVar = "I'm inner";
        
        console.log(innerVar);   // Found in current context
        console.log(outerVar);   // Found via scope chain
        console.log(globalVar);  // Found via scope chain