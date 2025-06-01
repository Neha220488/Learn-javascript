# Error Handling and Debugging in JavaScript

## Types of Errors

**Q: What are the main types of errors in JavaScript and when do they occur?**

**A:** JavaScript has several built-in error types that help identify specific problems in your code:

```javascript
// SyntaxError: Occurs when there's a mistake in the code syntax
// console.log('Hello world"; // Missing closing quote

// ReferenceError: Occurs when you try to reference a variable that doesn't exist
try {
    console.log(undefinedVariable);
} catch (error) {
    console.log(error.name);  // "ReferenceError"
    console.log(error.message);  // "undefinedVariable is not defined"
}

// TypeError: Occurs when a value is not of the expected type
try {
    const obj = null;
    console.log(obj.property);
} catch (error) {
    console.log(error.name);  // "TypeError"
    console.log(error.message);  // "Cannot read properties of null (reading 'property')"
}

// RangeError: Occurs when a numeric value is outside the allowed range
try {
    const arr = new Array(-1);  // Cannot create array with negative length
} catch (error) {
    console.log(error.name);  // "RangeError"
    console.log(error.message);  // "Invalid array length"
}

// URIError: Occurs when an incorrect URI function is used
try {
    decodeURIComponent('%');  // Invalid URI format
} catch (error) {
    console.log(error.name);  // "URIError"
    console.log(error.message);  // "URI malformed"
}

// EvalError: Historically for errors with the eval() function (rarely used now)
// eval('alert("Hello")');

// AggregateError: Group of errors (added in ES2020)
try {
    Promise.any([
        Promise.reject(new Error("Error 1")),
        Promise.reject(new Error("Error 2"))
    ]);
} catch (error) {
    console.log(error.name);  // "AggregateError"
    console.log(error.errors);  // [Error: "Error 1", Error: "Error 2"]
}

// Custom Error class
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}

try {
    throw new ValidationError("Invalid input");
} catch (error) {
    console.log(error.name);  // "ValidationError"
    console.log(error.message);  // "Invalid input"
}
```

## Try...Catch Statement

**Q: How does the try...catch statement work and when should you use it?**

**A:** Try...catch allows you to handle errors gracefully without stopping your program execution:

```javascript
// Basic try...catch
try {
    // Code that might throw an error
    const result = 10 / 0;  // Not actually an error in JavaScript
    console.log(result);  // Infinity
    
    // This will cause an error
    const obj = null;
    console.log(obj.property);  // TypeError
    
    // This code never runs
    console.log("This won't execute");
} catch (error) {
    // Handle the error
    console.log("An error occurred:");
    console.log("Name:", error.name);
    console.log("Message:", error.message);
}

// Code after try...catch continues to execute
console.log("Program continues running");

// Try...catch...finally
try {
    console.log("Starting operation");
    // Simulating an error
    throw new Error("Something went wrong");
} catch (error) {
    console.log("Error caught:", error.message);
} finally {
    // Always executes, whether an error occurred or not
    console.log("Cleanup code here");
}

// Error objects contain useful properties
try {
    throw new Error("Custom error message");
} catch (error) {
    console.log(error.name);  // "Error"
    console.log(error.message);  // "Custom error message"
    console.log(error.stack);  // Stack trace information
}

// Selective catch handling
function processData(data) {
    try {
        if (data === null) {
            throw new TypeError("Data cannot be null");
        }
        if (typeof data !== "object") {
            throw new TypeError("Data must be an object");
        }
        if (!data.hasOwnProperty("id")) {
            throw new ValidationError("Data must have an id");
        }
        
        // Process the data
        return `Processed data with id: ${data.id}`;
    } catch (error) {
        if (error instanceof ValidationError) {
            console.log("Validation issue:", error.message);
            return null;
        } else if (error instanceof TypeError) {
            console.log("Type issue:", error.message);
            return null;
        } else {
            // Re-throw errors we don't handle specifically
            throw error;
        }
    }
}

console.log(processData({ id: 123 }));  // "Processed data with id: 123"
console.log(processData(null));  // null (with "Type issue: Data cannot be null" logged)
console.log(processData({}));  // null (with "Validation issue: Data must have an id" logged)
```

**Q: How do you use try...catch...finally for comprehensive error handling?**

**A:** The finally block ensures cleanup code runs regardless of success or failure:

```javascript
// Basic try...catch...finally structure
function processFile(filename) {
    let file = null;
    
    try {
        console.log(`Opening file: ${filename}`);
        file = openFile(filename);  // Simulated file operation
        
        console.log('Processing file content...');
        const data = processFileData(file);
        
        console.log('File processed successfully');
        return data;
        
    } catch (error) {
        console.error(`Error processing file: ${error.message}`);
        
        // Handle specific error types
        if (error.name === 'FileNotFoundError') {
            console.log('Creating default file...');
            return createDefaultFile(filename);
        } else if (error.name === 'PermissionError') {
            console.log('Insufficient permissions');
            return null;
        } else {
            // Re-throw unexpected errors
            throw error;
        }
        
    } finally {
        // Cleanup code - always executes
        if (file) {
            console.log('Closing file...');
            closeFile(file);
        }
        console.log('Cleanup completed');
    }
}

// Database connection example
async function databaseOperation() {
    let connection = null;
    
    try {
        connection = await connectToDatabase();
        await connection.startTransaction();
        
        const result = await connection.query('SELECT * FROM users');
        await connection.commit();
        
        return result;
        
    } catch (error) {
        // Rollback on error
        if (connection) {
            await connection.rollback();
        }
        
        console.error('Database operation failed:', error.message);
        throw error;  // Re-throw for higher-level handling
        
    } finally {
        // Always close connection
        if (connection) {
            await connection.close();
        }
    }
}

// Resource management pattern
function withResource(resourceFactory, operation) {
    let resource = null;
    
    try {
        resource = resourceFactory();
        return operation(resource);
    } catch (error) {
        console.error('Operation failed:', error.message);
        throw error;
    } finally {
        if (resource && typeof resource.cleanup === 'function') {
            resource.cleanup();
        }
    }
}
```

**Q: How do you handle different error types with selective catch blocks?**

**A:** Use instanceof and error properties to handle specific error types differently:

```javascript
// Custom error classes for different scenarios
class ValidationError extends Error {
    constructor(message, field) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
    }
}

class NetworkError extends Error {
    constructor(message, statusCode) {
        super(message);
        this.name = 'NetworkError';
        this.statusCode = statusCode;
    }
}

class AuthenticationError extends Error {
    constructor(message) {
        super(message);
        this.name = 'AuthenticationError';
    }
}

// Function that can throw different error types
function processUserData(userData) {
    try {
        // Validation errors
        if (!userData.email) {
            throw new ValidationError('Email is required', 'email');
        }
        
        if (!userData.email.includes('@')) {
            throw new ValidationError('Invalid email format', 'email');
        }
        
        // Simulate authentication check
        if (!userData.token) {
            throw new AuthenticationError('Authentication token required');
        }
        
        // Simulate network operation
        if (userData.simulateNetworkError) {
            throw new NetworkError('Server unavailable', 503);
        }
        
        return { success: true, user: userData };
        
    } catch (error) {
        // Handle different error types
        if (error instanceof ValidationError) {
            console.log(`Validation error in field '${error.field}': ${error.message}`);
            return { 
                success: false, 
                error: 'validation',
                field: error.field,
                message: error.message 
            };
            
        } else if (error instanceof AuthenticationError) {
            console.log(`Authentication error: ${error.message}`);
            return { 
                success: false, 
                error: 'authentication',
                message: error.message 
            };
            
        } else if (error instanceof NetworkError) {
            console.log(`Network error (${error.statusCode}): ${error.message}`);
            return { 
                success: false, 
                error: 'network',
                statusCode: error.statusCode,
                message: error.message 
            };
            
        } else {
            // Unexpected errors - re-throw
            console.error('Unexpected error:', error);
            throw error;
        }
    }
}

// Usage examples
console.log(processUserData({}));  // Validation error
console.log(processUserData({ email: 'user@example.com' }));  // Auth error
console.log(processUserData({ 
    email: 'user@example.com', 
    token: 'abc123',
    simulateNetworkError: true 
}));  // Network error
```

**Q: How do you handle async/await errors and nested try...catch blocks?**

**A:** Async functions require try...catch for proper error handling, and nested blocks provide granular control:

```javascript
// Async error handling patterns
async function fetchUserProfile(userId) {
    try {
        // Network request
        const response = await fetch(`/api/users/${userId}`);
        
        if (!response.ok) {
            throw new NetworkError(
                `Failed to fetch user ${userId}`, 
                response.status
            );
        }
        
        const userData = await response.json();
        
        // Validate received data
        if (!userData.email) {
            throw new ValidationError('User data missing email', 'email');
        }
        
        return userData;
        
    } catch (error) {
        if (error instanceof NetworkError) {
            console.error(`Network issue: ${error.message}`);
            // Maybe try from cache or show offline message
            return getCachedUserData(userId);
            
        } else if (error instanceof ValidationError) {
            console.error(`Data validation failed: ${error.message}`);
            return null;
            
        } else {
            console.error('Unexpected error:', error);
            throw error;  // Re-throw for higher-level handling
        }
    }
}

// Nested try...catch for granular error handling
async function complexAsyncOperation() {
    let step = 'initialization';
    
    try {
        step = 'authentication';
        const authToken = await authenticate();
        
        step = 'user data';
        let userData;
        try {
            userData = await fetchUserData(authToken);
        } catch (userError) {
            console.log('User data fetch failed, using guest mode');
            userData = { guest: true };
        }
        
        step = 'preferences';
        let preferences;
        try {
            preferences = await fetchUserPreferences(userData.id);
        } catch (prefError) {
            console.log('Preferences fetch failed, using defaults');
            preferences = getDefaultPreferences();
        }
        
        return { userData, preferences };
        
    } catch (error) {
        console.error(`Operation failed at step '${step}':`, error.message);
        
        // Provide different fallback strategies based on failure point
        switch (step) {
            case 'authentication':
                return redirectToLogin();
            case 'user data':
                return { error: 'user_data_unavailable' };
            default:
                throw error;
        }
    }
}

// Error aggregation for multiple async operations
async function processMultipleUsers(userIds) {
    const results = [];
    const errors = [];
    
    for (const userId of userIds) {
        try {
            const user = await fetchUserProfile(userId);
            results.push({ userId, success: true, data: user });
        } catch (error) {
            errors.push({ userId, success: false, error: error.message });
        }
    }
    
    return { results, errors };
}

// Promise.allSettled alternative for older environments
async function processUsersInParallel(userIds) {
    const promises = userIds.map(async (userId) => {
        try {
            const user = await fetchUserProfile(userId);
            return { userId, success: true, data: user };
        } catch (error) {
            return { userId, success: false, error: error.message };
        }
    });
    
    return await Promise.all(promises);
}
```

**Best Practices:**
- ✅ **Always use try...catch** with async/await
- ✅ **Use finally** for cleanup operations  
- ✅ **Handle specific error types** differently
- ✅ **Re-throw unexpected errors** for higher-level handling
- ✅ **Log errors** with sufficient context
- ✅ **Provide fallback values** when appropriate

**Memory Tip:** 🎯 "**Try** the risky code, **Catch** the problems, **Finally** clean up - even if things go wrong!"

## Throwing Errors

**Q: How do you throw custom errors and when should you use the throw statement?**

**A:** The `throw` statement generates custom errors to signal specific problems in your code:

```javascript
// Throwing a basic error
function divide(a, b) {
    if (b === 0) {
        throw new Error("Division by zero is not allowed");
    }
    return a / b;
}

try {
    console.log(divide(10, 2));  // 5
    console.log(divide(10, 0));  // Throws an error
} catch (error) {
    console.log(error.message);  // "Division by zero is not allowed"
}

// Throwing different types of errors
function checkPositiveNumber(num) {
    if (typeof num !== 'number') {
        throw new TypeError("Parameter must be a number");
    }
    if (num <= 0) {
        throw new RangeError("Parameter must be positive");
    }
    return true;
}

try {
    checkPositiveNumber("10");
} catch (error) {
    console.log(`${error.name}: ${error.message}`);  // "TypeError: Parameter must be a number"
}

try {
    checkPositiveNumber(-5);
} catch (error) {
    console.log(`${error.name}: ${error.message}`);  // "RangeError: Parameter must be positive"
}

// Creating custom error types
class NetworkError extends Error {
    constructor(message, statusCode) {
        super(message);
        this.name = "NetworkError";
        this.statusCode = statusCode;
    }
}

class ValidationError extends Error {
    constructor(message, field) {
        super(message);
        this.name = "ValidationError";
        this.field = field;
    }
}

// Using custom errors
function validateUser(user) {
    if (!user) {
        throw new ValidationError("User object is required", "user");
    }
    
    if (!user.name) {
        throw new ValidationError("Name is required", "name");
    }
    
    if (user.age && (typeof user.age !== 'number' || user.age < 0)) {
        throw new ValidationError("Age must be a positive number", "age");
    }
}

try {
    validateUser({ name: "John", age: -5 });
} catch (error) {
    if (error instanceof ValidationError) {
        console.log(`Validation failed for field '${error.field}': ${error.message}`);
    } else {
        console.log(`Unexpected error: ${error.message}`);
    }
}

// Throwing in async functions
async function fetchUserData(userId) {
    try {
        const response = await fetch(`https://api.example.com/users/${userId}`);
        
        if (response.status === 404) {
            throw new NetworkError("User not found", 404);
        }
        
        if (!response.ok) {
            throw new NetworkError(`HTTP error: ${response.status}`, response.status);
        }
        
        return await response.json();
    } catch (error) {
        if (error instanceof NetworkError) {
            console.log(`Network error (${error.statusCode}): ${error.message}`);
        } else {
            console.log(`Fetch error: ${error.message}`);
        }
        return null;
    }
}

// Throwing vs. returning error values
// Bad practice: returning error codes or special values
function divideImperative(a, b) {
    if (b === 0) {
        return { success: false, error: "Division by zero" };
    }
    return { success: true, result: a / b };
}

const result = divideImperative(10, 0);
if (!result.success) {
    console.log(result.error);
}

// Better practice: throwing errors
function divideFunctional(a, b) {
    if (b === 0) {
        throw new Error("Division by zero");
    }
    return a / b;
}

try {
    const quotient = divideFunctional(10, 0);
    console.log(quotient);
} catch (error) {
    console.log(error.message);
}
```

## Error Propagation
JavaScript errors propagate up the call stack until they are caught or reach the global scope.

**Q: How do errors propagate through the call stack in JavaScript?**

**A:** When an error occurs, JavaScript stops executing the current function and starts looking for an error handler. If none is found in the current function, it moves up to the calling function, and so on until it finds a handler or reaches the global scope. This behavior is called "error propagation" or "bubbling."

**Q: What happens when an error propagates through multiple function calls?**

**A:** The error travels up the call stack until it encounters a try...catch block or reaches the global scope:

```javascript
// Error propagation through the call stack
function level3() {
    // Error occurs here
    console.log(nonExistentVariable);  // ReferenceError
}

function level2() {
    level3();  // Error propagates up from here
}

function level1() {
    try {
        level2();  // Error is caught here
    } catch (error) {
        console.log("Error caught in level1:", error.message);
    }
}

level1();  // "Error caught in level1: nonExistentVariable is not defined"
```

**Q: Does error propagation stop when an error is caught?**

**A:** Yes, propagation stops at the first catch block unless you explicitly rethrow the error:

```javascript
// Propagation stops at the first catch
function innerFunction() {
    throw new Error("Inner error");
}

function middleFunction() {
    try {
        innerFunction();
    } catch (error) {
        console.log("Caught in middleFunction:", error.message);
        // If we rethrow, the error continues to propagate
        // throw error;
    }
}

function outerFunction() {
    try {
        middleFunction();
        console.log("This will execute because error was caught in middleFunction");
    } catch (error) {
        console.log("Caught in outerFunction:", error.message);
    }
}

outerFunction();
// Output:
// "Caught in middleFunction: Inner error"
// "This will execute because error was caught in middleFunction"
```

**Q: How do you rethrow errors to continue propagation after handling them?**

**A:** Use the `throw` statement with the caught error to continue propagation up the call stack:
function processValue(value) {
    try {
        if (typeof value !== 'number') {
            throw new TypeError("Value must be a number");
        }
        if (value < 0) {
            throw new RangeError("Value must be non-negative");
        }
        return Math.sqrt(value);
    } catch (error) {
        console.log("Error in processValue:", error.message);
        
        // Rethrow the error after logging
        throw error;
    }
}

function calculateArea(radius) {
    try {
        const area = Math.PI * processValue(radius) ** 2;
        return area;
    } catch (error) {
        if (error instanceof TypeError) {
            return "Invalid type for radius";
        } else if (error instanceof RangeError) {
            return "Invalid value for radius";
        } else {
            return "Unknown error calculating area";
        }
    }
}

console.log(calculateArea(4));  // ~50.27
console.log(calculateArea(-4));  // "Invalid value for radius"
console.log(calculateArea("4"));  // "Invalid type for radius"
```

**Q: How does error propagation work with async/await functions?**

**A:** Async errors propagate similarly to synchronous errors, but they're wrapped in rejected promises:

```javascript
// Async error handling with try...catch
async function fetchData(url) {
    try {
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
    } catch (error) {
        console.log("Fetch error:", error.message);
        return null;
    }
}
```

**Q: How do nested try...catch blocks work for granular error handling?**

**A:** Nested try...catch blocks allow you to handle errors at different levels of granularity:

```javascript
// Nested try...catch blocks
function complexOperation() {
    try {
        console.log("Outer try block");
        
        try {
            console.log("Inner try block");
            throw new Error("Inner error");
        } catch (innerError) {
            console.log("Caught inner error:", innerError.message);
            throw new Error("New error from inner catch");
        }
        
        console.log("This won't execute");
    } catch (outerError) {
        console.log("Caught outer error:", outerError.message);
    }
}

complexOperation();
// Output:
// "Outer try block"
// "Inner try block"
// "Caught inner error: Inner error"
// "Caught outer error: New error from inner catch"
```

**Best Practices:**
- ✅ **Catch errors** at the appropriate level in your call stack
- ✅ **Re-throw errors** when you can't fully handle them
- ✅ **Add context** to errors before re-throwing
- ✅ **Use error boundaries** to prevent entire application crashes
- ✅ **Handle async errors** with try...catch in async functions

**Memory Tip:** 📈 "Errors **bubble up** the stack like air bubbles in water - catch them where you can **handle** them!"

## JavaScript Debugging Techniques

**Q: What are the essential console methods for debugging JavaScript applications?**

**A:** The console object provides powerful debugging tools beyond simple logging:

```javascript
// Basic logging levels
console.log('General information');
console.info('Informational message');
console.warn('Warning message');
console.error('Error message');

// Structured data inspection
const user = { 
    name: 'John', 
    age: 30, 
    preferences: { theme: 'dark', language: 'en' } 
};

console.table(user);                    // Display as formatted table
console.dir(user, { depth: null });     // Deep object inspection
console.json = obj => console.log(JSON.stringify(obj, null, 2));

// Performance debugging
console.time('operation');
performExpensiveOperation();
console.timeEnd('operation');           // Logs execution time

console.time('loop');
for (let i = 0; i < 1000000; i++) {
    // Some operation
}
console.timeLog('loop', 'Checkpoint at 1M iterations');
console.timeEnd('loop');

// Call stack and trace
function levelOne() {
    levelTwo();
}

function levelTwo() {
    levelThree();
}

function levelThree() {
    console.trace('Call stack trace');   // Shows function call path
}

levelOne(); // Displays the complete call stack

// Grouping related logs
console.group('User Authentication');
console.log('Validating credentials');
console.log('Checking permissions');
console.groupCollapsed('Database queries'); // Collapsed by default
console.log('SELECT * FROM users');
console.log('SELECT * FROM permissions');
console.groupEnd();
console.groupEnd();

// Conditional logging
const DEBUG = true;
console.assert(user.age > 0, 'User age must be positive', { user });
console.assert(false, 'This will always log');

// Custom log styling (browser only)
console.log('%c Success! ', 'background: green; color: white; padding: 2px');
console.log('%c Error! ', 'background: red; color: white; padding: 2px');
console.log('%c Debug %c Info', 'background: blue; color: white', 'background: gray; color: white');

// Memory usage monitoring
if (console.memory) {
    console.log('Memory usage:', {
        used: `${(console.memory.usedJSHeapSize / 1048576).toFixed(2)} MB`,
        total: `${(console.memory.totalJSHeapSize / 1048576).toFixed(2)} MB`,
        limit: `${(console.memory.jsHeapSizeLimit / 1048576).toFixed(2)} MB`
    });
}
```

**Q: How do you use the debugger statement and browser debugging tools effectively?**

**A:** The debugger statement and browser DevTools provide powerful debugging capabilities:

```javascript
// Using debugger statement
function calculateTotal(items) {
    let total = 0;
    
    for (let item of items) {
        debugger; // Execution will pause here when DevTools is open
        
        if (item.price && item.quantity) {
            total += item.price * item.quantity;
        }
    }
    
    return total;
}

// Conditional debugging
function processOrder(order) {
    if (order.total > 1000) {
        debugger; // Only pause for large orders
    }
    
    // Processing logic...
}

// Debugging with breakpoints in complex scenarios
function complexCalculation(data) {
    const step1 = data.map(item => {
        const processed = processItem(item);
        
        // Conditional breakpoint: only pause if processed.error exists
        if (processed.error) {
            debugger; 
        }
        
        return processed;
    });
    
    const step2 = step1.filter(item => item.isValid);
    
    debugger; // Always pause here to inspect step2
    
    return step2.reduce((acc, item) => acc + item.value, 0);
}

// Using try-catch with debugging
function debuggedAsyncOperation() {
    return async function(data) {
        try {
            console.log('Starting operation with:', data);
            
            const result = await riskyOperation(data);
            
            console.log('Operation successful:', result);
            return result;
            
        } catch (error) {
            // Set breakpoint here for error analysis
            debugger;
            
            console.error('Operation failed:', {
                error: error.message,
                stack: error.stack,
                input: data,
                timestamp: new Date().toISOString()
            });
            
            throw error;
        }
    };
}

// Browser DevTools debugging techniques
function demonstrateDebugging() {
    // 1. Sources Panel:
    //    - Set breakpoints by clicking line numbers
    //    - Use conditional breakpoints (right-click)
    //    - Set logpoints for non-intrusive debugging
    
    // 2. Console Panel:
    //    - Access variables in current scope
    //    - Execute code in current context
    //    - Use $0, $1, $2... for recently selected elements
    
    // 3. Network Panel:
    //    - Monitor API calls and responses
    //    - Check request/response headers
    //    - Analyze timing and performance
    
    // 4. Application Panel:
    //    - Inspect localStorage, sessionStorage
    //    - View and modify cookies
    //    - Check service worker status
    
    console.log('Check DevTools panels for debugging features');
}

// Debugging utility functions
const DebugUtils = {
    // Function execution wrapper with logging
    trace(fn, name = fn.name) {
        return function(...args) {
            console.group(`🔍 ${name}`);
            console.log('Arguments:', args);
            console.time(name);
            
            try {
                const result = fn.apply(this, args);
                
                if (result instanceof Promise) {
                    return result
                        .then(res => {
                            console.log('Resolved:', res);
                            console.timeEnd(name);
                            console.groupEnd();
                            return res;
                        })
                        .catch(err => {
                            console.error('Rejected:', err);
                            console.timeEnd(name);
                            console.groupEnd();
                            throw err;
                        });
                } else {
                    console.log('Result:', result);
                    console.timeEnd(name);
                    console.groupEnd();
                    return result;
                }
            } catch (error) {
                console.error('Error:', error);
                console.timeEnd(name);
                console.groupEnd();
                throw error;
            }
        };
    },
    
    // Object change monitoring
    watchProperty(obj, prop, callback) {
        let value = obj[prop];
        
        Object.defineProperty(obj, prop, {
            get() {
                console.log(`📖 Reading ${prop}:`, value);
                return value;
            },
            set(newValue) {
                console.log(`✏️ Writing ${prop}:`, value, '→', newValue);
                value = newValue;
                if (callback) callback(newValue, value);
            }
        });
    },
    
    // Memory leak detection helper
    trackObjectCreation(className) {
        const original = window[className];
        const instances = new Set();
        
        window[className] = function(...args) {
            const instance = new original(...args);
            instances.add(instance);
            
            console.log(`➕ Created ${className} instance. Total: ${instances.size}`);
            
            // Add cleanup tracking
            const originalDestroy = instance.destroy || instance.remove || (() => {});
            instance.destroy = function() {
                instances.delete(instance);
                console.log(`➖ Destroyed ${className} instance. Total: ${instances.size}`);
                return originalDestroy.call(this);
            };
            
            return instance;
        };
        
        // Global function to check instances
        window[`get${className}Count`] = () => instances.size;
    }
};
```

**Q: What are the best practices for debugging asynchronous JavaScript code?**

**A:** Async debugging requires special techniques to handle timing and flow control:

```javascript
// Promise debugging with enhanced logging
function createDebuggedPromise(promiseFactory, name) {
    return function(...args) {
        console.group(`🔄 ${name} - Starting`);
        console.log('Arguments:', args);
        console.time(name);
        
        return promiseFactory(...args)
            .then(result => {
                console.log(`✅ ${name} - Success:`, result);
                console.timeEnd(name);
                console.groupEnd();
                return result;
            })
            .catch(error => {
                console.error(`❌ ${name} - Error:`, error);
                console.timeEnd(name);
                console.groupEnd();
                throw error;
            });
    };
}

// Usage example
const debuggedFetch = createDebuggedPromise(
    (url, options) => fetch(url, options).then(r => r.json()),
    'API Call'
);

// Async/await debugging patterns
async function debuggedAsyncFunction() {
    try {
        console.log('🚀 Starting async operation');
        
        // Step-by-step debugging
        const step1 = await performStep1();
        console.log('Step 1 complete:', step1);
        
        const step2 = await performStep2(step1);
        console.log('Step 2 complete:', step2);
        
        const step3 = await performStep3(step2);
        console.log('Step 3 complete:', step3);
        
        console.log('✅ All steps completed successfully');
        return step3;
        
    } catch (error) {
        console.error('💥 Async operation failed at step:', error.step || 'unknown');
        console.error('Error details:', error);
        
        // Add debugging information to the error
        error.debugInfo = {
            timestamp: new Date().toISOString(),
            userAgent: navigator.userAgent,
            currentUrl: window.location.href
        };
        
        throw error;
    }
}

// Promise chain debugging
function debugPromiseChain() {
    return fetchUserData()
        .then(user => {
            console.log('👤 User fetched:', user);
            return user;
        })
        .then(user => fetchUserPreferences(user.id))
        .then(preferences => {
            console.log('⚙️ Preferences fetched:', preferences);
            return preferences;
        })
        .then(preferences => applyPreferences(preferences))
        .then(result => {
            console.log('✨ Preferences applied:', result);
            return result;
        })
        .catch(error => {
            // Log the exact point of failure
            console.error('🔍 Promise chain failed:', {
                error: error.message,
                stack: error.stack,
                chainStep: error.step || 'unknown'
            });
            throw error;
        });
}

// Concurrent operation debugging
async function debugConcurrentOperations() {
    const operations = [
        { name: 'fetchUsers', fn: () => fetch('/api/users') },
        { name: 'fetchPosts', fn: () => fetch('/api/posts') },
        { name: 'fetchComments', fn: () => fetch('/api/comments') }
    ];
    
    console.group('🔄 Concurrent Operations');
    
    const results = await Promise.allSettled(
        operations.map(async (op, index) => {
            try {
                console.time(op.name);
                const result = await op.fn();
                console.timeEnd(op.name);
                console.log(`✅ ${op.name} completed`);
                return { [op.name]: result };
            } catch (error) {
                console.error(`❌ ${op.name} failed:`, error);
                throw error;
            }
        })
    );
    
    // Analyze results
    const successful = results.filter(r => r.status === 'fulfilled');
    const failed = results.filter(r => r.status === 'rejected');
    
    console.log(`📊 Results: ${successful.length} successful, ${failed.length} failed`);
    
    if (failed.length > 0) {
        console.group('❌ Failed Operations');
        failed.forEach((failure, index) => {
            console.error(`${operations[index].name}:`, failure.reason);
        });
        console.groupEnd();
    }
    
    console.groupEnd();
    
    return results;
}

// Timeout debugging for slow operations
function withTimeout(promise, timeoutMs, operationName) {
    const timeoutPromise = new Promise((_, reject) => {
        setTimeout(() => {
            reject(new Error(`${operationName} timed out after ${timeoutMs}ms`));
        }, timeoutMs);
    });
    
    return Promise.race([
        promise.then(result => {
            console.log(`⏱️ ${operationName} completed within ${timeoutMs}ms`);
            return result;
        }),
        timeoutPromise
    ]);
}

// Usage
const timeoutTest = withTimeout(
    slowAsyncOperation(),
    5000,
    'Slow Operation'
).catch(error => {
    if (error.message.includes('timed out')) {
        console.error('🐌 Operation was too slow:', error.message);
    } else {
        console.error('💥 Operation failed:', error.message);
    }
});
```

**Q: How do you debug performance issues and memory leaks in JavaScript?**

**A:** Use profiling tools and monitoring techniques to identify performance bottlenecks:

```javascript
// Performance monitoring utilities
const PerfMonitor = {
    // Function execution time tracking
    timeFunction(fn, name = fn.name) {
        return function(...args) {
            const start = performance.now();
            const result = fn.apply(this, args);
            const end = performance.now();
            
            console.log(`⏱️ ${name}: ${(end - start).toFixed(2)}ms`);
            
            if (result instanceof Promise) {
                return result.finally(() => {
                    const asyncEnd = performance.now();
                    console.log(`⏱️ ${name} (async): ${(asyncEnd - start).toFixed(2)}ms`);
                });
            }
            
            return result;
        };
    },
    
    // Memory usage tracking
    trackMemory(label) {
        if (performance.memory) {
            const used = performance.memory.usedJSHeapSize;
            const total = performance.memory.totalJSHeapSize;
            
            console.log(`🧠 ${label} - Memory: ${(used / 1048576).toFixed(2)}MB / ${(total / 1048576).toFixed(2)}MB`);
            
            return { used, total, percentage: (used / total * 100).toFixed(1) };
        }
    },
    
    // Frame rate monitoring
    monitorFPS() {
        let frames = 0;
        let lastTime = performance.now();
        
        function countFrame(currentTime) {
            frames++;
            
            if (currentTime - lastTime >= 1000) {
                console.log(`📺 FPS: ${frames}`);
                frames = 0;
                lastTime = currentTime;
            }
            
            requestAnimationFrame(countFrame);
        }
        
        requestAnimationFrame(countFrame);
    },
    
    // DOM operation performance
    measureDOMOperations(operation, description) {
        const start = performance.now();
        
        // Force reflow to get accurate measurements
        document.body.offsetHeight;
        
        const result = operation();
        
        // Force another reflow
        document.body.offsetHeight;
        
        const end = performance.now();
        console.log(`🖼️ ${description}: ${(end - start).toFixed(2)}ms`);
        
        return result;
    }
};

// Memory leak detection patterns
const MemoryLeakDetector = {
    // Event listener leak detection
    trackEventListeners() {
        const originalAddEventListener = EventTarget.prototype.addEventListener;
        const originalRemoveEventListener = EventTarget.prototype.removeEventListener;
        const listenerMap = new WeakMap();
        
        EventTarget.prototype.addEventListener = function(type, listener, options) {
            if (!listenerMap.has(this)) {
                listenerMap.set(this, new Set());
            }
            listenerMap.get(this).add(`${type}:${listener.name || 'anonymous'}`);
            
            console.log(`➕ Added listener: ${type} on`, this);
            return originalAddEventListener.call(this, type, listener, options);
        };
        
        EventTarget.prototype.removeEventListener = function(type, listener, options) {
            if (listenerMap.has(this)) {
                listenerMap.get(this).delete(`${type}:${listener.name || 'anonymous'}`);
            }
            
            console.log(`➖ Removed listener: ${type} on`, this);
            return originalRemoveEventListener.call(this, type, listener, options);
        };
        
        // Global function to check remaining listeners
        window.getActiveListeners = () => {
            const elements = document.querySelectorAll('*');
            let totalListeners = 0;
            
            elements.forEach(el => {
                if (listenerMap.has(el)) {
                    const listeners = listenerMap.get(el);
                    if (listeners.size > 0) {
                        console.log('Element with listeners:', el, Array.from(listeners));
                        totalListeners += listeners.size;
                    }
                }
            });
            
            console.log(`Total active listeners: ${totalListeners}`);
            return totalListeners;
        };
    },
    
    // Closure leak detection
    detectClosureLeaks() {
        const originalSetTimeout = window.setTimeout;
        const originalSetInterval = window.setInterval;
        const activeTimers = new Map();
        
        window.setTimeout = function(callback, delay, ...args) {
            const id = originalSetTimeout.call(this, function() {
                activeTimers.delete(id);
                return callback.apply(this, args);
            }, delay);
            
            activeTimers.set(id, {
                type: 'timeout',
                callback: callback.toString().substring(0, 100),
                created: new Date().toISOString()
            });
            
            return id;
        };
        
        window.setInterval = function(callback, delay, ...args) {
            const id = originalSetInterval.call(this, callback, delay, ...args);
            
            activeTimers.set(id, {
                type: 'interval',
                callback: callback.toString().substring(0, 100),
                created: new Date().toISOString()
            });
            
            return id;
        };
        
        // Global function to check active timers
        window.getActiveTimers = () => {
            console.log(`Active timers: ${activeTimers.size}`);
            activeTimers.forEach((info, id) => {
                console.log(`Timer ${id}:`, info);
            });
            return activeTimers.size;
        };
    },
    
    // DOM node leak detection
    trackDOMNodes() {
        const nodeCreationMap = new WeakMap();
        const originalCreateElement = document.createElement;
        
        document.createElement = function(tagName) {
            const element = originalCreateElement.call(this, tagName);
            nodeCreationMap.set(element, {
                tagName,
                created: new Date().toISOString(),
                stack: new Error().stack
            });
            
            return element;
        };
        
        // Check for detached nodes
        window.findDetachedNodes = () => {
            const allElements = document.querySelectorAll('*');
            const attachedNodes = new Set(allElements);
            let detachedCount = 0;
            
            // This is a simplified check - real tools use more sophisticated methods
            console.log(`Attached nodes: ${attachedNodes.size}`);
            console.log('Use Chrome DevTools Memory tab for accurate detached node detection');
            
            return detachedCount;
        };
    }
};

// Usage examples
console.log('🔧 Initialize debugging tools:');
console.log('PerfMonitor.trackMemory("Initial")');
console.log('PerfMonitor.monitorFPS()');
console.log('MemoryLeakDetector.trackEventListeners()');
console.log('MemoryLeakDetector.detectClosureLeaks()');
```

**Best Practices:**
- 🔍 **Use meaningful names** for debugger breakpoints and console logs
- 📊 **Profile before optimizing** - measure actual performance bottlenecks
- 🧹 **Clean up debugging code** before production deployment
- 🎯 **Use conditional debugging** to avoid performance impact
- 📱 **Test on mobile devices** for performance and memory constraints
- 🔄 **Monitor async operations** carefully for timing issues

**Memory Tip:** 🕵️ "Debug like a detective - **gather evidence**, **form hypotheses**, and **test systematically**!"
