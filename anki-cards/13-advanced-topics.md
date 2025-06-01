# Advanced JavaScript Topics - Anki Cards

## Regular Expressions (RegExp)

**Q: How do you create and use regular expressions in JavaScript?**

**A:** Regular expressions (RegExp) are patterns for matching character combinations in strings. JavaScript supports both literal syntax and constructor syntax.

**Creating Regular Expressions:**
```javascript
// Literal syntax
const regex1 = /hello/;
const regex2 = /hello/gi;  // g = global, i = case-insensitive

// Constructor syntax
const regex3 = new RegExp('hello');
const regex4 = new RegExp('hello', 'gi');

// Dynamic patterns
const pattern = 'world';
const dynamicRegex = new RegExp(pattern, 'i');
```

**Common Methods:**
```javascript
const text = "Hello World, hello universe!";
const pattern = /hello/gi;

// test() - returns boolean
console.log(pattern.test(text));  // true

// exec() - returns match array or null
const match = pattern.exec(text);
console.log(match);  // ['Hello', index: 0, input: '...', groups: undefined]

// String methods with regex
console.log(text.match(pattern));     // ['Hello', 'hello']
console.log(text.search(pattern));    // 0
console.log(text.replace(pattern, 'hi')); // "hi World, hi universe!"
console.log(text.split(/\s+/));       // ['Hello', 'World,', 'hello', 'universe!']
```

**Common Patterns:**
```javascript
// Email validation
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
console.log(emailRegex.test('user@example.com'));  // true

// Phone number
const phoneRegex = /^\(\d{3}\)\s\d{3}-\d{4}$/;
console.log(phoneRegex.test('(123) 456-7890'));  // true

// Extract numbers
const numberRegex = /\d+/g;
console.log('I have 5 apples and 10 oranges'.match(numberRegex));  // ['5', '10']

// Capture groups
const nameRegex = /(\w+)\s+(\w+)/;
const result = nameRegex.exec('John Doe');
console.log(result[1]);  // 'John'
console.log(result[2]);  // 'Doe'

// Named capture groups (ES2018)
const namedRegex = /(?<first>\w+)\s+(?<last>\w+)/;
const namedResult = namedRegex.exec('Jane Smith');
console.log(namedResult.groups.first);  // 'Jane'
console.log(namedResult.groups.last);   // 'Smith'
```

---

## Symbols
**Q:** What are Symbols in JavaScript and how are they used?

**A:** Symbols are primitive data types that represent unique identifiers, often used for object properties to avoid naming collisions.

**Basic Usage:**
```javascript
// Creating symbols
const sym1 = Symbol();
const sym2 = Symbol();
const sym3 = Symbol('description');

console.log(sym1 === sym2);  // false (each symbol is unique)
console.log(typeof sym1);    // 'symbol'
console.log(sym3.toString()); // 'Symbol(description)'

// Symbols as object properties
const mySymbol = Symbol('myProperty');
const obj = {
    [mySymbol]: 'secret value',
    regularProperty: 'normal value'
};

console.log(obj[mySymbol]);        // 'secret value'
console.log(obj.regularProperty);  // 'normal value'

// Symbols don't appear in normal iteration
console.log(Object.keys(obj));           // ['regularProperty']
console.log(Object.getOwnPropertyNames(obj)); // ['regularProperty']
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(myProperty)]
```

**Well-Known Symbols:**
```javascript
// Symbol.iterator - makes objects iterable
const iterableObj = {
    data: [1, 2, 3],
    [Symbol.iterator]() {
        let index = 0;
        const data = this.data;
        return {
            next() {
                if (index < data.length) {
                    return { value: data[index++], done: false };
                } else {
                    return { done: true };
                }
            }
        };
    }
};

for (const value of iterableObj) {
    console.log(value);  // 1, 2, 3
}

// Symbol.toStringTag - custom string representation
class MyClass {
    get [Symbol.toStringTag]() {
        return 'MyClass';
    }
}

const instance = new MyClass();
console.log(instance.toString());  // '[object MyClass]'

// Symbol.hasInstance - custom instanceof behavior
class MyArray {
    static [Symbol.hasInstance](instance) {
        return Array.isArray(instance);
    }
}

console.log([] instanceof MyArray);  // true
```

**Global Symbol Registry:**
```javascript
// Symbol.for() creates/retrieves global symbols
const globalSym1 = Symbol.for('global.key');
const globalSym2 = Symbol.for('global.key');
console.log(globalSym1 === globalSym2);  // true

// Symbol.keyFor() returns the key of a global symbol
console.log(Symbol.keyFor(globalSym1));  // 'global.key'
```

---

## Generators and Iterators
**Q:** How do generators and iterators work in JavaScript?

**A:** Generators are functions that can be paused and resumed, yielding multiple values over time. Iterators are objects that implement the iteration protocol.

**Generator Functions:**
```javascript
// Basic generator
function* simpleGenerator() {
    yield 1;
    yield 2;
    yield 3;
}

const gen = simpleGenerator();
console.log(gen.next());  // { value: 1, done: false }
console.log(gen.next());  // { value: 2, done: false }
console.log(gen.next());  // { value: 3, done: false }
console.log(gen.next());  // { value: undefined, done: true }

// Generator with parameters
function* parametricGenerator() {
    const x = yield 'first';
    const y = yield `received: ${x}`;
    return `final: ${y}`;
}

const paramGen = parametricGenerator();
console.log(paramGen.next());        // { value: 'first', done: false }
console.log(paramGen.next('hello')); // { value: 'received: hello', done: false }
console.log(paramGen.next('world')); // { value: 'final: world', done: true }

// Infinite sequence generator
function* fibonacci() {
    let [prev, curr] = [0, 1];
    while (true) {
        yield curr;
        [prev, curr] = [curr, prev + curr];
    }
}

const fib = fibonacci();
console.log(fib.next().value);  // 1
console.log(fib.next().value);  // 1
console.log(fib.next().value);  // 2
console.log(fib.next().value);  // 3
console.log(fib.next().value);  // 5

// Generator delegation with yield*
function* gen1() {
    yield 1;
    yield 2;
}

function* gen2() {
    yield 3;
    yield 4;
}

function* combined() {
    yield* gen1();
    yield* gen2();
    yield 5;
}

console.log([...combined()]);  // [1, 2, 3, 4, 5]
```

**Custom Iterators:**
```javascript
// Creating a custom iterator
const range = {
    start: 1,
    end: 5,
    [Symbol.iterator]() {
        let current = this.start;
        const end = this.end;
        return {
            next() {
                if (current <= end) {
                    return { value: current++, done: false };
                } else {
                    return { done: true };
                }
            }
        };
    }
};

for (const num of range) {
    console.log(num);  // 1, 2, 3, 4, 5
}

// Using Array.from with iterables
console.log(Array.from(range));  // [1, 2, 3, 4, 5]

// Async generators
async function* asyncGenerator() {
    yield await Promise.resolve(1);
    yield await Promise.resolve(2);
    yield await Promise.resolve(3);
}

(async () => {
    for await (const value of asyncGenerator()) {
        console.log(value);  // 1, 2, 3
    }
})();
```

---

## Proxy and Reflect
**Q:** How do Proxy and Reflect objects work in JavaScript?

**A:** Proxy allows you to intercept and customize operations performed on objects. Reflect provides methods for interceptable JavaScript operations.

**Basic Proxy Usage:**
```javascript
// Creating a proxy
const target = {
    name: 'John',
    age: 30
};

const handler = {
    get(target, property, receiver) {
        console.log(`Getting property: ${property}`);
        return Reflect.get(target, property, receiver);
    },
    
    set(target, property, value, receiver) {
        console.log(`Setting property: ${property} = ${value}`);
        return Reflect.set(target, property, value, receiver);
    }
};

const proxy = new Proxy(target, handler);

console.log(proxy.name);  // "Getting property: name" -> "John"
proxy.age = 31;           // "Setting property: age = 31"
```

**Advanced Proxy Traps:**
```javascript
const person = { name: 'Alice', age: 25 };

const advancedProxy = new Proxy(person, {
    has(target, property) {
        console.log(`Checking if property exists: ${property}`);
        return Reflect.has(target, property);
    },
    
    deleteProperty(target, property) {
        if (property === 'name') {
            console.log('Cannot delete name property');
            return false;
        }
        return Reflect.deleteProperty(target, property);
    },
    
    ownKeys(target) {
        console.log('Getting property keys');
        return Reflect.ownKeys(target);
    },
    
    apply(target, thisArg, argumentsList) {
        // For function proxies
        console.log('Function called with args:', argumentsList);
        return Reflect.apply(target, thisArg, argumentsList);
    }
});

console.log('name' in advancedProxy);  // "Checking if property exists: name" -> true
delete advancedProxy.age;              // Deletes successfully
delete advancedProxy.name;             // "Cannot delete name property" -> false
console.log(Object.keys(advancedProxy)); // "Getting property keys" -> ['name']
```

**Reflect Methods:**
```javascript
const obj = { x: 1, y: 2 };

// Reflect provides all the same operations as proxy traps
console.log(Reflect.get(obj, 'x'));        // 1
console.log(Reflect.set(obj, 'z', 3));     // true
console.log(Reflect.has(obj, 'y'));        // true
console.log(Reflect.ownKeys(obj));         // ['x', 'y', 'z']
console.log(Reflect.deleteProperty(obj, 'x')); // true

// Function reflection
function greet(name) {
    return `Hello, ${name}!`;
}

console.log(Reflect.apply(greet, null, ['World'])); // "Hello, World!"

// Constructor reflection
class Person {
    constructor(name) {
        this.name = name;
    }
}

const person = Reflect.construct(Person, ['Bob']);
console.log(person instanceof Person);  // true
console.log(person.name);               // 'Bob'
```

**Practical Proxy Examples:**
```javascript
// Validation proxy
function createValidatedUser(userData) {
    return new Proxy(userData, {
        set(target, property, value) {
            if (property === 'age' && (value < 0 || value > 150)) {
                throw new Error('Age must be between 0 and 150');
            }
            if (property === 'email' && !value.includes('@')) {
                throw new Error('Email must contain @');
            }
            return Reflect.set(target, property, value);
        }
    });
}

const user = createValidatedUser({ name: 'John' });
// user.age = -5;  // Error: Age must be between 0 and 150
user.age = 25;     // Works fine

// Array with negative indexing
function createArrayWithNegativeIndexing(arr) {
    return new Proxy(arr, {
        get(target, property) {
            if (typeof property === 'string' && !isNaN(property)) {
                const index = parseInt(property);
                if (index < 0) {
                    return target[target.length + index];
                }
            }
            return Reflect.get(target, property);
        }
    });
}

const array = createArrayWithNegativeIndexing([1, 2, 3, 4, 5]);
console.log(array[-1]);  // 5 (last element)
console.log(array[-2]);  // 4 (second to last)
```

---

## WeakMap and WeakSet
**Q:** What are WeakMap and WeakSet, and how do they differ from Map and Set?

**A:** WeakMap and WeakSet are collections that hold weak references to their keys/values, allowing for garbage collection when no other references exist.

**WeakMap:**
```javascript
// WeakMap only accepts objects as keys
const wm = new WeakMap();
const obj1 = { id: 1 };
const obj2 = { id: 2 };

wm.set(obj1, 'first object');
wm.set(obj2, 'second object');

console.log(wm.get(obj1));  // 'first object'
console.log(wm.has(obj2));  // true

// Keys must be objects
// wm.set('string', 'value');  // TypeError

// WeakMap is not enumerable
// console.log(wm.size);  // undefined
// for (let key of wm) {}  // TypeError

// When obj1 goes out of scope and is garbage collected,
// its entry in WeakMap is automatically removed
```

**WeakSet:**
```javascript
// WeakSet only accepts objects
const ws = new WeakSet();
const element1 = { name: 'div' };
const element2 = { name: 'span' };

ws.add(element1);
ws.add(element2);

console.log(ws.has(element1));  // true

// Values must be objects
// ws.add('string');  // TypeError

// WeakSet is not enumerable
// console.log(ws.size);  // undefined
// for (let value of ws) {}  // TypeError

ws.delete(element1);
console.log(ws.has(element1));  // false
```

**Practical Use Cases:**
```javascript
// Private data with WeakMap
const privateData = new WeakMap();

class BankAccount {
    constructor(balance) {
        privateData.set(this, { balance });
    }
    
    getBalance() {
        return privateData.get(this).balance;
    }
    
    deposit(amount) {
        const data = privateData.get(this);
        data.balance += amount;
    }
    
    withdraw(amount) {
        const data = privateData.get(this);
        if (data.balance >= amount) {
            data.balance -= amount;
            return amount;
        }
        throw new Error('Insufficient funds');
    }
}

const account = new BankAccount(100);
console.log(account.getBalance());  // 100
account.deposit(50);
console.log(account.getBalance());  // 150

// Cannot access private data directly
// console.log(account.balance);  // undefined

// DOM element tracking with WeakSet
const processedElements = new WeakSet();

function processElement(element) {
    if (processedElements.has(element)) {
        console.log('Element already processed');
        return;
    }
    
    // Process the element
    console.log('Processing element');
    processedElements.add(element);
}

const div = document.createElement('div');
processElement(div);  // "Processing element"
processElement(div);  // "Element already processed"

// When div is removed from DOM and no longer referenced,
// it's automatically removed from WeakSet
```

---

## Memory Management and Garbage Collection

**Q: How does JavaScript handle memory management and what are the garbage collection mechanisms?**

**A:** JavaScript automatically manages memory through garbage collection. Modern engines use the mark-and-sweep algorithm to free memory that's no longer reachable.

**Memory Lifecycle:**
```javascript
// 1. Allocation - memory is allocated when variables are declared
let user = { name: 'John', age: 30 };  // Object allocated in heap memory

// 2. Usage - memory is used when variables are accessed/modified
console.log(user.name);  // Memory is accessed
user.age = 31;           // Memory is modified

// 3. Release - memory is freed when no longer reachable
user = null;  // Object becomes eligible for garbage collection
```

**Q: What's the difference between reference counting and mark-and-sweep garbage collection?**

**A:** JavaScript evolved from reference counting to mark-and-sweep to handle circular references:

**Reference Counting (older, problematic):**
```javascript
// Reference counting has issues with circular references
function createCircularReference() {
    const obj1 = { name: 'Object 1' };
    const obj2 = { name: 'Object 2' };
    
    obj1.ref = obj2;  // obj1 references obj2
    obj2.ref = obj1;  // obj2 references obj1 (circular!)
    
    return [obj1, obj2];
}
// Even when function returns, circular references prevent cleanup
// in reference counting systems
```

**Mark-and-Sweep (modern, robust):**
```javascript
// Modern engines use mark-and-sweep algorithm
// 1. Start from "roots" (global variables, call stack)
// 2. Mark all reachable objects
// 3. Sweep (delete) unreachable objects

function demonstrateGarbageCollection() {
    let largeArray = new Array(1000000).fill('data');
    
    // largeArray is reachable within this function scope
    console.log('Array created and reachable');
    
    // When function ends, largeArray becomes unreachable
    // and eligible for garbage collection
}

demonstrateGarbageCollection();
// largeArray is now unreachable and will be garbage collected
```

**Q: What are common memory leak patterns and how can you prevent them?**

**A:** Memory leaks occur when objects remain reachable when they should be cleaned up:

**Common Leak Patterns:**
```javascript
// 1. ❌ Accidental global variables
function createGlobalLeak() {
    // Forgot 'let/const' - creates global variable!
    massiveData = new Array(1000000).fill('leak');
}

// ✅ Fix: Always declare variables
function noGlobalLeak() {
    const massiveData = new Array(1000000).fill('safe');
    // Automatically cleaned up when function ends
}

// 2. ❌ Event listeners not removed
const element = document.getElementById('button');
function addPermanentListener() {
    element.addEventListener('click', function handler() {
        // This handler keeps references alive forever
        console.log('Clicked');
    });
}

// ✅ Fix: Remove event listeners
function addTemporaryListener() {
    const handler = () => console.log('Clicked');
    element.addEventListener('click', handler);
    
    // Clean up when done
    setTimeout(() => {
        element.removeEventListener('click', handler);
    }, 10000);
}

// 3. ❌ Timers not cleared
function createTimerLeak() {
    const data = new Array(1000000).fill('timer leak');
    
    setInterval(() => {
        console.log(data.length); // Keeps 'data' alive forever
    }, 1000);
}

// ✅ Fix: Clear timers
function createCleanTimer() {
    const data = new Array(1000000).fill('clean');
    
    const intervalId = setInterval(() => {
        console.log(data.length);
    }, 1000);
    
    // Clear after 10 seconds
    setTimeout(() => {
        clearInterval(intervalId);
    }, 10000);
}
```

**Memory Management Best Practices:**
- 🧹 **Clean up**: Remove event listeners, clear timers
- 🌍 **Avoid globals**: Use local scope when possible  
- 🔄 **Break cycles**: Set circular references to null
- 💾 **Use WeakMap/WeakSet**: For object associations
- 📊 **Profile regularly**: Use DevTools Memory tab

**Memory Tip:** 🗑️ "Memory management = **Mark** what you need, **Sweep** what you don't!"
    // handler function keeps reference to element
}

// Good:
function addListenerCorrectly() {
    const element = document.getElementById('button');
    function handler() {
        console.log('Clicked');
    }
    element.addEventListener('click', handler);
    
    // Clean up when done
    return () => element.removeEventListener('click', handler);
}

// 3. Closures holding references
// Bad:
function createClosure() {
    const largeData = new Array(1000000).fill('data');
    
    return function() {
        // This closure keeps largeData in memory
        console.log('Closure called');
    };
}

// Good:
function createClosureCorrectly() {
    const largeData = new Array(1000000).fill('data');
    const neededData = largeData.slice(0, 10);  // Extract only what's needed
    
    return function() {
        console.log(neededData.length);  // Only keeps small reference
    };
}

// 4. Timers and intervals
// Bad:
function startTimer() {
    const data = new Array(1000000);
    setInterval(() => {
        console.log(data.length);  // Keeps data in memory
    }, 1000);
}

// Good:
function startTimerCorrectly() {
    const data = new Array(1000000);
    const timer = setInterval(() => {
        console.log(data.length);
    }, 1000);
    
    // Clean up after some time
    setTimeout(() => {
        clearInterval(timer);
    }, 10000);
}
```

**Performance Monitoring:**
```javascript
// Monitoring memory usage (in supporting environments)
if (performance.memory) {
    console.log('Used JS heap size:', performance.memory.usedJSHeapSize);
    console.log('Total JS heap size:', performance.memory.totalJSHeapSize);
    console.log('JS heap size limit:', performance.memory.jsHeapSizeLimit);
}

// Force garbage collection (in development environments)
// if (window.gc) {
//     window.gc();  // Available with --expose-gc flag in Chrome
// }

// Weak references for manual memory management
const registry = new FinalizationRegistry(heldValue => {
    console.log(`Object with held value ${heldValue} was garbage collected`);
});

let obj = { data: 'important' };
registry.register(obj, 'my-object');

// obj = null;  // Object becomes eligible for GC
// Eventually logs: "Object with held value my-object was garbage collected"
```

---

## Performance Optimization Techniques
**Q:** What are the key JavaScript performance optimization techniques?

**A:** JavaScript performance optimization involves various strategies from code-level optimizations to runtime considerations.

**Code-Level Optimizations:**
```javascript
// 1. Variable declarations and scope
// Bad: repeated variable lookup
function inefficientLoop() {
    for (let i = 0; i < document.getElementsByTagName('div').length; i++) {
        // DOM query executed every iteration
    }
}

// Good: cache expensive operations
function efficientLoop() {
    const divs = document.getElementsByTagName('div');
    const length = divs.length;
    for (let i = 0; i < length; i++) {
        // Cached values
    }
}

// 2. Object property access
// Bad: repeated property access
function processUser(user) {
    if (user.profile.settings.notifications.email) {
        sendEmail(user.profile.contact.email);
    }
}

// Good: cache property access
function processUserEfficiently(user) {
    const { profile } = user;
    const { settings, contact } = profile;
    if (settings.notifications.email) {
        sendEmail(contact.email);
    }
}

// 3. Array operations
// Bad: creating new arrays unnecessarily
function processNumbers(numbers) {
    return numbers
        .filter(n => n > 0)
        .map(n => n * 2)
        .filter(n => n < 100);
}

// Good: combine operations
function processNumbersEfficiently(numbers) {
    const result = [];
    for (const n of numbers) {
        if (n > 0) {
            const doubled = n * 2;
            if (doubled < 100) {
                result.push(doubled);
            }
        }
    }
    return result;
}
```

**DOM Optimization:**
```javascript
// 1. Batch DOM operations
// Bad: multiple reflows
function badDOMUpdate() {
    const element = document.getElementById('myDiv');
    element.style.width = '100px';   // Reflow
    element.style.height = '100px';  // Reflow
    element.style.color = 'red';     // Repaint
}

// Good: batch style changes
function goodDOMUpdate() {
    const element = document.getElementById('myDiv');
    element.className = 'updated-style';  // Single reflow/repaint
}

// 2. Use DocumentFragment for multiple insertions
// Bad: multiple DOM insertions
function badListCreation(items) {
    const list = document.getElementById('list');
    items.forEach(item => {
        const li = document.createElement('li');
        li.textContent = item;
        list.appendChild(li);  // DOM insertion on each iteration
    });
}

// Good: use DocumentFragment
function goodListCreation(items) {
    const list = document.getElementById('list');
    const fragment = document.createDocumentFragment();
    items.forEach(item => {
        const li = document.createElement('li');
        li.textContent = item;
        fragment.appendChild(li);  // Fragment operation
    });
    list.appendChild(fragment);  // Single DOM insertion
}

// 3. Event delegation
// Bad: multiple event listeners
function badEventHandling() {
    const buttons = document.querySelectorAll('.button');
    buttons.forEach(button => {
        button.addEventListener('click', handleClick);
    });
}

// Good: single delegated event listener
function goodEventHandling() {
    document.addEventListener('click', function(event) {
        if (event.target.classList.contains('button')) {
            handleClick(event);
        }
    });
}
```

**Async Optimization:**
```javascript
// 1. Debouncing expensive operations
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

const expensiveOperation = debounce(function(input) {
    // Expensive API call or computation
    console.log('Processing:', input);
}, 300);

// 2. Throttling for scroll/resize events
function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

const throttledScroll = throttle(function() {
    console.log('Scroll event handled');
}, 100);

// 3. Lazy loading with Intersection Observer
const lazyImageObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            const img = entry.target;
            img.src = img.dataset.src;
            lazyImageObserver.unobserve(img);
        }
    });
});

document.querySelectorAll('img[data-src]').forEach(img => {
    lazyImageObserver.observe(img);
});

// 4. Web Workers for heavy computations
function performHeavyComputation(data) {
    if (typeof Worker !== 'undefined') {
        const worker = new Worker('computation-worker.js');
        worker.postMessage(data);
        worker.onmessage = function(event) {
            console.log('Result:', event.data);
        };
    } else {
        // Fallback for environments without Web Workers
        const result = heavyComputationFunction(data);
        console.log('Result:', result);
    }
}
```

**Memory and Resource Optimization:**
```javascript
// 1. Object pooling for frequently created objects
class ObjectPool {
    constructor(createFn, resetFn, initialSize = 10) {
        this.createFn = createFn;
        this.resetFn = resetFn;
        this.pool = [];
        
        // Pre-populate pool
        for (let i = 0; i < initialSize; i++) {
            this.pool.push(this.createFn());
        }
    }
    
    acquire() {
        return this.pool.length > 0 ? this.pool.pop() : this.createFn();
    }
    
    release(obj) {
        this.resetFn(obj);
        this.pool.push(obj);
    }
}

// Usage
const particlePool = new ObjectPool(
    () => ({ x: 0, y: 0, velocity: 0 }),
    (particle) => { particle.x = 0; particle.y = 0; particle.velocity = 0; }
);

// 2. Memoization for expensive function calls
function memoize(fn) {
    const cache = new Map();
    return function(...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) {
            return cache.get(key);
        }
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
}

const expensiveFunction = memoize(function(n) {
    console.log('Computing for:', n);
    return n * n * n;
});

console.log(expensiveFunction(5));  // Computes
console.log(expensiveFunction(5));  // Returns cached result

// 3. Efficient data structures
// Use Map/Set instead of objects/arrays for certain operations
// Bad: checking existence in array
function checkExistenceInArray(arr, item) {
    return arr.includes(item);  // O(n) complexity
}

// Good: checking existence in Set
function checkExistenceInSet(set, item) {
    return set.has(item);  // O(1) complexity
}

// 4. Lazy evaluation
class LazyArray {
    constructor(generator) {
        this.generator = generator;
        this._cache = new Map();
    }
    
    get(index) {
        if (!this._cache.has(index)) {
            this._cache.set(index, this.generator(index));
        }
        return this._cache.get(index);
    }
}

const lazyNumbers = new LazyArray(i => {
    console.log(`Generating value for index ${i}`);
    return i * i;
});

console.log(lazyNumbers.get(5));  // Generates and caches
console.log(lazyNumbers.get(5));  // Returns cached value
```
