# Objects in JavaScript

## Object Literals
The simplest way to create an object is using an object literal.

**Q&A Format:**

**Q: What are object literals and how do they work?**
A: Object literals provide a convenient syntax for creating objects using curly braces `{}`. They consist of key-value pairs separated by colons, with multiple properties separated by commas. Object literals create objects directly without using constructors.

**Q: What types of property keys and values can object literals contain?**
A: 
- **Keys**: Can be strings, identifiers (without quotes if valid), or computed property names using `[expression]`
- **Values**: Any valid JavaScript expression including primitives, objects, arrays, functions
- **Methods**: Functions stored as object properties can use shorthand syntax

**Q: How do you access, modify, and check properties in object literals?**
A: 
- **Access**: Dot notation (`obj.prop`) or bracket notation (`obj['prop']`)
- **Modify**: Same as access, plus `delete` keyword to remove properties
- **Check**: `'prop' in obj`, `obj.hasOwnProperty('prop')`, or `Object.keys(obj)`

```javascript
// Basic object literal
const person = {
    firstName: "John",
    lastName: "Doe",
    age: 30,
    isEmployed: true,
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }
};

// Accessing properties with dot notation
console.log(person.firstName);      // "John"
console.log(person.age);            // 30

// Accessing properties with bracket notation
console.log(person["lastName"]);    // "Doe"
console.log(person["isEmployed"]);  // true

// Calling a method
console.log(person.fullName());     // "John Doe"

// Nested objects
const user = {
    id: 101,
    name: "John Doe",
    email: "john@example.com",
    address: {
        street: "123 Main St",
        city: "Boston",
        state: "MA",
        zipCode: "02101"
    }
};

console.log(user.address.city);  // "Boston"
console.log(user.address.zipCode);  // "02101"

// Object with array properties
const student = {
    name: "Jane Smith",
    grades: [95, 87, 92, 88],
    calculateAverage: function() {
        const sum = this.grades.reduce((total, grade) => total + grade, 0);
        return sum / this.grades.length;
    }
};

console.log(student.grades[0]);  // 95
console.log(student.calculateAverage());  // 90.5

// Adding properties to an existing object
person.email = "john.doe@example.com";
person["phoneNumber"] = "555-123-4567";

console.log(person.email);  // "john.doe@example.com"
console.log(person.phoneNumber);  // "555-123-4567"

// Updating properties
person.age = 31;
console.log(person.age);  // 31

// Deleting properties
delete person.isEmployed;
console.log(person.isEmployed);  // undefined

// Object with property names that need brackets
const specialObject = {
    "first-name": "John",  // Hyphenated property name
    "2nd": "value",        // Property name starting with a number
    "!special": true       // Property name with special character
};

// These must be accessed with bracket notation
console.log(specialObject["first-name"]);  // "John"
console.log(specialObject["2nd"]);         // "value"

// Using variables as property keys
const propertyName = "color";
const car = {
    [propertyName]: "red"  // Computed property name
};

console.log(car.color);  // "red"

// Method shorthand (ES6)
const calculator = {
    add(a, b) {  // Shorthand for add: function(a, b)
        return a + b;
    },
    subtract(a, b) {
        return a - b;
    }
};

console.log(calculator.add(5, 3));       // 8
console.log(calculator.subtract(10, 4));  // 6

// Property shorthand (ES6)
const x = 10;
const y = 20;
const coords = { x, y };  // Shorthand for { x: x, y: y }

console.log(coords);  // {x: 10, y: 20}

// Checking if a property exists
console.log("firstName" in person);  // true
console.log("middleName" in person);  // false
console.log(person.hasOwnProperty("lastName"));  // true

// Object.keys() - get array of property names
console.log(Object.keys(person));  // ["firstName", "lastName", "age", "fullName", "email", "phoneNumber"]

// Object.values() - get array of property values
console.log(Object.values(person));  // ["John", "Doe", 31, ƒ, "john.doe@example.com", "555-123-4567"]
````

## Object Constructor and Object.create()
JavaScript provides multiple ways to create objects beyond literals.

**Q&A Format:**

**Q: What are the different ways to create objects in JavaScript?**
A: 
- **Object literals**: `{ key: value }` - simplest and most common
- **Object constructor**: `new Object()` - creates empty object
- **Object.create()**: `Object.create(prototype)` - creates object with specific prototype
- **Constructor functions**: `new Constructor()` - pattern before ES6 classes

**Q: How does Object.create() work and when should you use it?**
A: `Object.create(prototype)` creates a new object with the specified prototype object. Use it when you need precise control over inheritance or want to create objects with specific prototype chains.

**Q: What's the difference between Object.create() and object literals?**
A: 
- **Object literals**: Always have `Object.prototype` as their prototype
- **Object.create()**: Can specify any object (or `null`) as the prototype
- Object.create() is more flexible for inheritance patterns

```javascript
// Object literal
const person = {
    firstName: "John",
    lastName: "Doe",
    age: 30,
    isEmployed: true,
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }
};

// Accessing properties with dot notation
console.log(person.firstName);      // "John"
console.log(person.age);            // 30

// Accessing properties with bracket notation
console.log(person["lastName"]);    // "Doe"
console.log(person["isEmployed"]);  // true

// Calling a method
console.log(person.fullName());     // "John Doe"

// Nested objects
const user = {
    id: 101,
    name: "John Doe",
    email: "john@example.com",
    address: {
        street: "123 Main St",
        city: "Boston",
        state: "MA",
        zipCode: "02101"
    }
};

console.log(user.address.city);  // "Boston"
console.log(user.address.zipCode);  // "02101"

// Object with array properties
const student = {
    name: "Jane Smith",
    grades: [95, 87, 92, 88],
    calculateAverage: function() {
        const sum = this.grades.reduce((total, grade) => total + grade, 0);
        return sum / this.grades.length;
    }
};

console.log(student.grades[0]);  // 95
console.log(student.calculateAverage());  // 90.5

// Adding properties to an existing object
person.email = "john.doe@example.com";
person["phoneNumber"] = "555-123-4567";

console.log(person.email);  // "john.doe@example.com"
console.log(person.phoneNumber);  // "555-123-4567"

// Updating properties
person.age = 31;
console.log(person.age);  // 31

// Deleting properties
delete person.isEmployed;
console.log(person.isEmployed);  // undefined

// Object with property names that need brackets
const specialObject = {
    "first-name": "John",  // Hyphenated property name
    "2nd": "value",        // Property name starting with a number
    "!special": true       // Property name with special character
};

// These must be accessed with bracket notation
console.log(specialObject["first-name"]);  // "John"
console.log(specialObject["2nd"]);         // "value"

// Using variables as property keys
const propertyName = "color";
const car = {
    [propertyName]: "red"  // Computed property name
};

console.log(car.color);  // "red"

// Method shorthand (ES6)
const calculator = {
    add(a, b) {  // Shorthand for add: function(a, b)
        return a + b;
    },
    subtract(a, b) {
        return a - b;
    }
};

console.log(calculator.add(5, 3));       // 8
console.log(calculator.subtract(10, 4));  // 6

// Property shorthand (ES6)
const x = 10;
const y = 20;
const coords = { x, y };  // Shorthand for { x: x, y: y }

console.log(coords);  // {x: 10, y: 20}

// Checking if a property exists
console.log("firstName" in person);  // true
console.log("middleName" in person);  // false
console.log(person.hasOwnProperty("lastName"));  // true

// Object.keys() - get array of property names
console.log(Object.keys(person));  // ["firstName", "lastName", "age", "fullName", "email", "phoneNumber"]

// Object.values() - get array of property values
console.log(Object.values(person));  // ["John", "Doe", 31, ƒ, "john.doe@example.com", "555-123-4567"]

// Object constructor (less common)
const emptyObj = new Object();
emptyObj.name = "Created with constructor";
console.log(emptyObj.name);  // "Created with constructor"

// Object.create() with prototype
const personPrototype = {
    greet: function() {
        return `Hello, I'm ${this.name}`;
    },
    sayAge: function() {
        return `I am ${this.age} years old`;
    }
};

const john = Object.create(personPrototype);
john.name = "John";
john.age = 25;

console.log(john.greet());  // "Hello, I'm John"
console.log(john.sayAge()); // "I am 25 years old"

// Object.create() with null prototype (no inherited properties)
const cleanObject = Object.create(null);
cleanObject.name = "Clean";
console.log(cleanObject.toString);  // undefined (no inherited methods)

// Object.create() with property descriptors
const employee = Object.create(personPrototype, {
    name: {
        value: "Jane",
        writable: true,
        enumerable: true,
        configurable: true
    },
    employeeId: {
        value: 12345,
        writable: false,
        enumerable: true,
        configurable: false
    }
});

console.log(employee.name);       // "Jane"
console.log(employee.employeeId); // 12345
employee.employeeId = 54321;      // Fails silently (non-writable)
console.log(employee.employeeId); // Still 12345
```

## Object Properties and Methods
Understanding property types, descriptors, and object methods is crucial for advanced JavaScript.

**Q&A Format:**

**Q: What are the different types of object properties?**
A: 
- **Data properties**: Store actual values (value, writable, enumerable, configurable)
- **Accessor properties**: Use getter/setter functions (get, set, enumerable, configurable)
- **Own properties**: Directly on the object vs inherited from prototype
- **Enumerable vs non-enumerable**: Whether they appear in `for...in` loops

**Q: How do you control property behavior using descriptors?**
A: Use `Object.defineProperty()` or `Object.defineProperties()` with descriptor objects containing:
- **value**: The property's value
- **writable**: Can the value be changed?
- **enumerable**: Does it appear in `for...in` loops?
- **configurable**: Can the property be deleted or descriptor modified?

**Q: What are getters and setters and how do they work?**
A: Accessor properties that execute functions when accessed or assigned. Getters use `get` keyword, setters use `set`. They allow computed properties, validation, and encapsulation while appearing as normal properties.

```javascript
// Property descriptors
const person = {};

// Define property with full descriptor
Object.defineProperty(person, 'name', {
    value: 'John',
    writable: true,      // Can be changed
    enumerable: true,    // Shows up in for...in loops and Object.keys()
    configurable: true   // Can be deleted or descriptor modified
});

console.log(person.name);  // 'John'

// Get property descriptor
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// { value: 'John', writable: true, enumerable: true, configurable: true }

// Non-writable property (read-only)
Object.defineProperty(person, 'id', {
    value: 12345,
    writable: false,
    enumerable: true,
    configurable: true
});

console.log(person.id);  // 12345
person.id = 54321;       // Fails silently in non-strict mode
console.log(person.id);  // Still 12345

// Non-enumerable property (hidden from enumeration)
Object.defineProperty(person, 'ssn', {
    value: '123-45-6789',
    writable: true,
    enumerable: false,   // Won't show up in Object.keys() or for...in
    configurable: true
});

console.log(person.ssn);  // '123-45-6789'
console.log(Object.keys(person));  // ['name', 'id'] (ssn not included)

// Non-configurable property
Object.defineProperty(person, 'species', {
    value: 'human',
    writable: true,
    enumerable: true,
    configurable: false  // Cannot be deleted or reconfigured
});

// This will fail
try {
    delete person.species;  // Cannot delete
} catch (e) {
    console.log('Cannot delete non-configurable property');
}

// Define multiple properties at once
Object.defineProperties(person, {
    firstName: {
        value: 'John',
        writable: true,
        enumerable: true,
        configurable: true
    },
    lastName: {
        value: 'Doe',
        writable: true,
        enumerable: true,
        configurable: true
    }
});

// Getters and Setters
const account = {
    _balance: 0,  // Convention: underscore indicates "private"
    
    get balance() {
        return `$${this._balance}`;
    },
    
    set balance(value) {
        if (value < 0) {
            throw new Error('Balance cannot be negative');
        }
        this._balance = value;
    },
    
    get accountType() {
        return this._balance > 1000 ? 'Premium' : 'Standard';
    }
};

account.balance = 1500;
console.log(account.balance);      // '$1500'
console.log(account.accountType);  // 'Premium'

// Define getter/setter with Object.defineProperty
Object.defineProperty(person, 'fullName', {
    get: function() {
        return `${this.firstName} ${this.lastName}`;
    },
    set: function(name) {
        const parts = name.split(' ');
        this.firstName = parts[0];
        this.lastName = parts[1];
    },
    enumerable: true,
    configurable: true
});

console.log(person.fullName);  // 'John Doe'
person.fullName = 'Jane Smith';
console.log(person.firstName);  // 'Jane'
console.log(person.lastName);   // 'Smith'

// Object property enumeration
const testObj = {
    a: 1,
    b: 2
};

Object.defineProperty(testObj, 'hidden', {
    value: 'secret',
    enumerable: false
});

console.log(Object.keys(testObj));                    // ['a', 'b']
console.log(Object.getOwnPropertyNames(testObj));     // ['a', 'b', 'hidden']
console.log(Object.getOwnPropertyDescriptors(testObj)); // All descriptors

// Check property attributes
for (const key in testObj) {
    console.log(`${key}: ${testObj[key]}`);  // Only 'a' and 'b' (enumerable)
}

// Property existence checks
console.log('a' in testObj);                    // true
console.log('hidden' in testObj);               // true
console.log(testObj.hasOwnProperty('a'));       // true
console.log(testObj.hasOwnProperty('toString')); // false (inherited)
console.log(testObj.propertyIsEnumerable('a'));  // true
console.log(testObj.propertyIsEnumerable('hidden')); // false
```

## Object Methods and Utilities
JavaScript provides many built-in methods for working with objects.

**Q&A Format:**

**Q: What are the essential Object static methods for object manipulation?**
A: 
- **Object.keys()**: Get enumerable property names as array
- **Object.values()**: Get enumerable property values as array  
- **Object.entries()**: Get [key, value] pairs as arrays
- **Object.assign()**: Copy properties from source to target objects
- **Object.freeze()**: Make object immutable

**Q: How do Object.assign() and the spread operator compare for object copying?**
A: Both perform shallow copying:
- **Object.assign()**: `Object.assign(target, ...sources)` - modifies target
- **Spread operator**: `{...obj1, ...obj2}` - creates new object
- Spread syntax is generally preferred for readability and immutability

**Q: What's the difference between Object.freeze(), Object.seal(), and Object.preventExtensions()?**
A: Different levels of object protection:
- **preventExtensions()**: Cannot add new properties (can modify/delete existing)
- **seal()**: Cannot add/delete properties (can modify existing values)
- **freeze()**: Cannot add/delete/modify anything (completely immutable)

```javascript
// Object.keys(), Object.values(), Object.entries()
const person = {
    name: 'John',
    age: 30,
    city: 'New York'
};

console.log(Object.keys(person));    // ['name', 'age', 'city']
console.log(Object.values(person));  // ['John', 30, 'New York']
console.log(Object.entries(person)); // [['name', 'John'], ['age', 30], ['city', 'New York']]

// Iterating with Object.entries()
for (const [key, value] of Object.entries(person)) {
    console.log(`${key}: ${value}`);
}

// Object.fromEntries() - reverse of Object.entries()
const entries = [['a', 1], ['b', 2], ['c', 3]];
const objFromEntries = Object.fromEntries(entries);
console.log(objFromEntries);  // { a: 1, b: 2, c: 3 }

// Object.assign() - copying properties
const target = { a: 1, b: 2 };
const source1 = { b: 3, c: 4 };
const source2 = { c: 5, d: 6 };

const result = Object.assign(target, source1, source2);
console.log(result);  // { a: 1, b: 3, c: 5, d: 6 }
console.log(target);  // Same as result (target was modified)

// Object.assign() for cloning
const original = { name: 'John', age: 30 };
const clone = Object.assign({}, original);
clone.age = 31;
console.log(original.age);  // 30 (unchanged)
console.log(clone.age);     // 31

// Spread operator (preferred for cloning)
const person2 = { ...original };
const merged = { ...original, city: 'Boston', age: 32 };
console.log(merged);  // { name: 'John', age: 32, city: 'Boston' }

// Object.freeze() - make immutable
const frozenObj = Object.freeze({
    name: 'John',
    hobbies: ['reading', 'coding']
});

// These operations will fail silently in non-strict mode
frozenObj.name = 'Jane';        // Cannot modify
frozenObj.age = 30;             // Cannot add
delete frozenObj.name;          // Cannot delete
frozenObj.hobbies.push('gaming'); // This works! (shallow freeze)

console.log(frozenObj.name);     // 'John' (unchanged)
console.log(frozenObj.hobbies);  // ['reading', 'coding', 'gaming']
console.log(Object.isFrozen(frozenObj));  // true

// Deep freeze utility
function deepFreeze(obj) {
    Object.getOwnPropertyNames(obj).forEach(prop => {
        const value = obj[prop];
        if (value && typeof value === 'object') {
            deepFreeze(value);
        }
    });
    return Object.freeze(obj);
}

const deepFrozenObj = deepFreeze({
    name: 'John',
    hobbies: ['reading', 'coding']
});

deepFrozenObj.hobbies.push('gaming'); // This now fails too

// Object.seal() - prevent add/delete, allow modification
const sealedObj = Object.seal({
    name: 'John',
    age: 30
});

sealedObj.name = 'Jane';  // This works (can modify)
sealedObj.city = 'Boston'; // This fails (cannot add)
delete sealedObj.age;     // This fails (cannot delete)

console.log(sealedObj.name);  // 'Jane'
console.log(Object.isSealed(sealedObj));  // true

// Object.preventExtensions() - prevent adding properties
const nonExtensible = Object.preventExtensions({
    name: 'John',
    age: 30
});

nonExtensible.name = 'Jane';  // This works (can modify)
nonExtensible.city = 'Boston'; // This fails (cannot add)
delete nonExtensible.age;     // This works (can delete)

console.log(Object.isExtensible(nonExtensible));  // false

// Object comparison and equality
const obj1 = { a: 1 };
const obj2 = { a: 1 };
const obj3 = obj1;

console.log(obj1 === obj2);  // false (different objects)
console.log(obj1 === obj3);  // true (same reference)

// Shallow equality check utility
function shallowEqual(obj1, obj2) {
    const keys1 = Object.keys(obj1);
    const keys2 = Object.keys(obj2);
    
    if (keys1.length !== keys2.length) return false;
    
    for (const key of keys1) {
        if (obj1[key] !== obj2[key]) return false;
    }
    return true;
}

console.log(shallowEqual(obj1, obj2));  // true
console.log(shallowEqual(obj1, { a: 1, b: 2 }));  // false

// Object.is() - stricter equality
console.log(Object.is(NaN, NaN));        // true
console.log(Object.is(+0, -0));          // false
console.log(Object.is(5, 5));            // true
console.log(Object.is('foo', 'foo'));    // true
```

## this Keyword in Objects
Understanding how `this` works in object contexts is fundamental to JavaScript.

**Q&A Format:**

**Q: How does the `this` keyword work in object methods?**
A: `this` refers to the object that called the method. The value of `this` is determined by how the function is called, not where it's defined. In object methods, `this` typically refers to the object before the dot.

**Q: What happens to `this` when methods are assigned to variables or passed as callbacks?**
A: The method loses its context and `this` becomes `undefined` (strict mode) or the global object (non-strict). This is a common source of bugs. Solutions include `.bind()`, arrow functions, or wrapper functions.

**Q: How do arrow functions handle `this` differently from regular functions?**
A: Arrow functions don't have their own `this` - they inherit `this` from the enclosing scope. This makes them useful for callbacks but unsuitable for object methods when you need the object as `this`.

```javascript
// Basic 'this' in object methods
const person = {
    name: 'John',
    age: 30,
    
    greet: function() {
        return `Hello, I'm ${this.name}`;
    },
    
    getAge: function() {
        return this.age;
    },
    
    // Method shorthand (ES6)
    introduce() {
        return `Hi, I'm ${this.name} and I'm ${this.age} years old`;
    }
};

console.log(person.greet());      // "Hello, I'm John"
console.log(person.getAge());     // 30
console.log(person.introduce());  // "Hi, I'm John and I'm 30 years old"

// Lost context when assigning to variable
const greetFunction = person.greet;
console.log(greetFunction());  // "Hello, I'm undefined" (this is not person)

// Solutions for maintaining context:

// 1. Using .bind()
const boundGreet = person.greet.bind(person);
console.log(boundGreet());  // "Hello, I'm John"

// 2. Using arrow function wrapper
const wrappedGreet = () => person.greet();
console.log(wrappedGreet());  // "Hello, I'm John"

// 3. Using .call() or .apply()
console.log(greetFunction.call(person));   // "Hello, I'm John"
console.log(greetFunction.apply(person));  // "Hello, I'm John"

// Arrow functions and 'this'
const objectWithArrow = {
    name: 'Jane',
    
    // Regular function - 'this' is the object
    regularMethod: function() {
        return `Regular: ${this.name}`;
    },
    
    // Arrow function - 'this' is from enclosing scope (not the object)
    arrowMethod: () => {
        return `Arrow: ${this.name}`;  // 'this' is not objectWithArrow!
    },
    
    // Arrow function in nested context
    nestedExample: function() {
        // Regular function - 'this' is the object
        console.log(`Outer this.name: ${this.name}`);
        
        // Arrow function inherits 'this' from outer function
        const inner = () => {
            console.log(`Inner this.name: ${this.name}`);
        };
        inner();
        
        // Regular function would lose context
        const innerRegular = function() {
            console.log(`Inner regular this.name: ${this.name}`);
        };
        innerRegular(); // 'this' is undefined/global
    }
};

console.log(objectWithArrow.regularMethod());  // "Regular: Jane"
console.log(objectWithArrow.arrowMethod());    // "Arrow: undefined"
objectWithArrow.nestedExample();

// Callback context issues
const timer = {
    seconds: 0,
    
    start: function() {
        // Problem: 'this' in setTimeout callback
        setTimeout(function() {
            this.seconds++;  // 'this' is not timer object
            console.log(this.seconds);
        }, 1000);
    },
    
    startWithBind: function() {
        // Solution 1: Use .bind()
        setTimeout(function() {
            this.seconds++;
            console.log(`Bind solution: ${this.seconds}`);
        }.bind(this), 1000);
    },
    
    startWithArrow: function() {
        // Solution 2: Use arrow function
        setTimeout(() => {
            this.seconds++;
            console.log(`Arrow solution: ${this.seconds}`);
        }, 1000);
    },
    
    startWithSelf: function() {
        // Solution 3: Store reference to 'this'
        const self = this;
        setTimeout(function() {
            self.seconds++;
            console.log(`Self solution: ${self.seconds}`);
        }, 1000);
    }
};

// Demonstrating different behaviors
timer.startWithArrow();  // This will work correctly
timer.startWithBind();   // This will work correctly
timer.startWithSelf();   // This will work correctly

// Method chaining with 'this'
const calculator = {
    value: 0,
    
    add(num) {
        this.value += num;
        return this;  // Return 'this' for chaining
    },
    
    subtract(num) {
        this.value -= num;
        return this;
    },
    
    multiply(num) {
        this.value *= num;
        return this;
    },
    
    getValue() {
        return this.value;
    },
    
    reset() {
        this.value = 0;
        return this;
    }
};

// Method chaining
const result = calculator
    .add(10)
    .multiply(2)
    .subtract(5)
    .getValue();

console.log(result);  // 15

calculator.reset();

// 'this' in different call contexts
const obj = {
    name: 'Test Object',
    method: function() {
        return this.name;
    }
};

console.log(obj.method());              // "Test Object" (method call)

const standalone = obj.method;
console.log(standalone());              // undefined (function call)

const newObj = { name: 'New Object' };
console.log(obj.method.call(newObj));   // "New Object" (explicit binding)
console.log(obj.method.apply(newObj));  // "New Object" (explicit binding)

const boundMethod = obj.method.bind(newObj);
console.log(boundMethod());             // "New Object" (bound context)
```

## Object Destructuring and Modern Patterns
Modern JavaScript provides powerful syntax for working with objects.

**Q&A Format:**

**Q: How does object destructuring work and what are its advanced features?**
A: Object destructuring extracts properties into variables using `{property}` syntax. Advanced features include renaming, default values, nested destructuring, and rest operator for remaining properties.

**Q: What are computed property names and how do you use them?**
A: Computed property names use `[expression]` syntax to dynamically create property keys. The expression is evaluated and the result becomes the property name. Useful for dynamic object creation.

**Q: How do modern object patterns like shorthand properties and method definitions improve code?**
A: ES6+ provides cleaner syntax:
- Property shorthand: `{name}` instead of `{name: name}`
- Method shorthand: `method() {}` instead of `method: function() {}`
- These reduce boilerplate and improve readability

```javascript
// Object destructuring basics
const person = {
    firstName: 'John',
    lastName: 'Doe',
    age: 30,
    email: 'john@example.com',
    address: {
        street: '123 Main St',
        city: 'Boston',
        country: 'USA'
    }
};

// Basic destructuring
const { firstName, lastName, age } = person;
console.log(firstName);  // 'John'
console.log(lastName);   // 'Doe'
console.log(age);        // 30

// Destructuring with renaming
const { firstName: fName, lastName: lName } = person;
console.log(fName);  // 'John'
console.log(lName);  // 'Doe'

// Destructuring with default values
const { firstName: first, middleName = 'N/A', phone = 'Not provided' } = person;
console.log(first);      // 'John'
console.log(middleName); // 'N/A'
console.log(phone);      // 'Not provided'

// Nested destructuring
const { address: { street, city, country } } = person;
console.log(street);   // '123 Main St'
console.log(city);     // 'Boston'
console.log(country);  // 'USA'

// Nested destructuring with renaming
const { address: { city: hometown, country: nation } } = person;
console.log(hometown);  // 'Boston'
console.log(nation);    // 'USA'

// Rest operator in destructuring
const { firstName: name, ...rest } = person;
console.log(name);  // 'John'
console.log(rest);  // { lastName: 'Doe', age: 30, email: 'john@example.com', address: {...} }

// Function parameter destructuring
function greetPerson({ firstName, age, address: { city } }) {
    return `Hello ${firstName}, age ${age}, from ${city}`;
}

console.log(greetPerson(person));  // "Hello John, age 30, from Boston"

// Function parameter destructuring with defaults
function createUser({ 
    name = 'Anonymous', 
    age = 0, 
    role = 'user',
    active = true 
} = {}) {
    return { name, age, role, active, createdAt: new Date() };
}

console.log(createUser({ name: 'Alice', age: 25 }));
console.log(createUser());  // Uses all defaults

// Computed property names
const propertyName = 'dynamicKey';
const value = 'dynamicValue';
const prefix = 'user';

const dynamicObject = {
    [propertyName]: value,
    [`${prefix}Name`]: 'John',
    [`${prefix}Age`]: 30,
    [Date.now()]: 'timestamp key',
    ['key' + Math.random()]: 'random key'
};

console.log(dynamicObject.dynamicKey);    // 'dynamicValue'
console.log(dynamicObject.userName);      // 'John'
console.log(dynamicObject.userAge);       // 30

// Computed property names with methods
const methodName = 'computedMethod';
const obj = {
    [methodName]: function() {
        return 'This method was defined with computed property name';
    },
    
    [`get${methodName.charAt(0).toUpperCase() + methodName.slice(1)}`]: function() {
        return 'Getter method';
    }
};

console.log(obj.computedMethod());     // 'This method was defined...'
console.log(obj.getComputedMethod());  // 'Getter method'

// Property and method shorthand
const name = 'Jane';
const age = 28;
const email = 'jane@example.com';

// Old way
const userOld = {
    name: name,
    age: age,
    email: email,
    greet: function() {
        return `Hello, I'm ${this.name}`;
    }
};

// Modern shorthand
const userModern = {
    name,      // Property shorthand
    age,
    email,
    greet() {  // Method shorthand
        return `Hello, I'm ${this.name}`;
    },
    
    // Can mix shorthand with regular syntax
    fullInfo: function() {
        return `${this.name}, ${this.age}, ${this.email}`;
    }
};

console.log(userModern.greet());     // "Hello, I'm Jane"
console.log(userModern.fullInfo());  // "Jane, 28, jane@example.com"

// Advanced destructuring patterns
const users = [
    { id: 1, name: 'Alice', role: 'admin' },
    { id: 2, name: 'Bob', role: 'user' },
    { id: 3, name: 'Charlie', role: 'user' }
];

// Destructuring in array iteration
users.forEach(({ name, role }) => {
    console.log(`${name} is a ${role}`);
});

// Destructuring with array methods
const adminUsers = users
    .filter(({ role }) => role === 'admin')
    .map(({ name, id }) => ({ displayName: name, userId: id }));

console.log(adminUsers);  // [{ displayName: 'Alice', userId: 1 }]

// Swapping variables with destructuring
let x = 1;
let y = 2;
[x, y] = [y, x];
console.log(x, y);  // 2, 1

// Object destructuring for configuration objects
function configureApp({
    theme = 'light',
    language = 'en',
    debug = false,
    features: {
        notifications = true,
        analytics = false
    } = {}
} = {}) {
    return {
        theme,
        language,
        debug,
        notifications,
        analytics
    };
}

const config1 = configureApp({
    theme: 'dark',
    features: { notifications: false }
});

const config2 = configureApp();  // All defaults

console.log(config1);  // { theme: 'dark', language: 'en', debug: false, notifications: false, analytics: false }
console.log(config2);  // { theme: 'light', language: 'en', debug: false, notifications: true, analytics: false }

// Object shorthand in function returns
function createResponse(data, success = true, message = 'OK') {
    return {
        data,
        success,
        message,
        timestamp: Date.now()
    };
}

console.log(createResponse({ user: 'John' }));
// { data: { user: 'John' }, success: true, message: 'OK', timestamp: 1234567890 }
```

## JSON and Object Serialization
Working with JSON (JavaScript Object Notation) for data exchange and storage.

**Q&A Format:**

**Q: What is JSON and how does it relate to JavaScript objects?**
A: JSON (JavaScript Object Notation) is a text-based data format for data exchange. While similar to JavaScript object literals, JSON has stricter rules: strings must use double quotes, no functions/undefined/symbols, no comments, and limited data types.

**Q: How do JSON.parse() and JSON.stringify() work, and what are their limitations?**
A: 
- **JSON.stringify()**: Converts JavaScript values to JSON strings
- **JSON.parse()**: Converts JSON strings back to JavaScript values
- **Limitations**: Functions, undefined, symbols are omitted; dates become strings; circular references cause errors

**Q: How do you handle complex objects and custom serialization with JSON?**
A: Use the replacer/reviver parameters in JSON methods, or implement `toJSON()` methods on objects. For complex cases, consider custom serialization libraries or manual handling of problematic properties.

```javascript
// Basic JSON operations
const person = {
    name: 'John',
    age: 30,
    city: 'New York',
    hobbies: ['reading', 'coding', 'hiking'],
    isActive: true
};

// Convert object to JSON string
const jsonString = JSON.stringify(person);
console.log(jsonString);
// '{"name":"John","age":30,"city":"New York","hobbies":["reading","coding","hiking"],"isActive":true}'

// Convert JSON string back to object
const parsedObject = JSON.parse(jsonString);
console.log(parsedObject);  // Object identical to original person

// JSON.stringify with formatting (pretty print)
const prettyJson = JSON.stringify(person, null, 2);
console.log(prettyJson);
/*
{
  "name": "John",
  "age": 30,
  "city": "New York",
  "hobbies": [
    "reading",
    "coding",
    "hiking"
  ],
  "isActive": true
}
*/

// JSON limitations - what gets lost
const complexObject = {
    name: 'Test',
    func: function() { return 'hello'; },      // Functions are omitted
    undef: undefined,                          // undefined is omitted
    sym: Symbol('test'),                       // Symbols are omitted
    date: new Date(),                          // Dates become strings
    regex: /test/g,                           // RegExp becomes empty object {}
    nan: NaN,                                 // NaN becomes null
    infinity: Infinity                        // Infinity becomes null
};

console.log('Original:', complexObject);
const jsonComplex = JSON.stringify(complexObject);
console.log('JSON:', jsonComplex);
const parsedComplex = JSON.parse(jsonComplex);
console.log('Parsed:', parsedComplex);

// Using replacer function in JSON.stringify
const personWithSecrets = {
    name: 'John',
    age: 30,
    password: 'secret123',
    ssn: '123-45-6789',
    email: 'john@example.com'
};

// Replacer function to exclude sensitive data
const safeStringify = JSON.stringify(personWithSecrets, (key, value) => {
    if (key === 'password' || key === 'ssn') {
        return undefined;  // Exclude these properties
    }
    return value;
});

console.log(safeStringify);  // Password and SSN are excluded

// Using replacer array to include only specific properties
const limitedStringify = JSON.stringify(personWithSecrets, ['name', 'email']);
console.log(limitedStringify);  // Only name and email included

// Using reviver function in JSON.parse
const jsonWithDates = '{"name":"John","birthDate":"2023-01-15T10:30:00.000Z","lastLogin":"2023-12-01T14:45:00.000Z"}';

const parsedWithDates = JSON.parse(jsonWithDates, (key, value) => {
    // Convert date strings back to Date objects
    if (key.includes('Date') || key.includes('Login')) {
        return new Date(value);
    }
    return value;
});

console.log(parsedWithDates);
console.log(parsedWithDates.birthDate instanceof Date);  // true

// Custom toJSON method
class User {
    constructor(name, email, password) {
        this.name = name;
        this.email = email;
        this.password = password;
        this.createdAt = new Date();
    }
    
    // Custom serialization method
    toJSON() {
        return {
            name: this.name,
            email: this.email,
            // Exclude password from serialization
            createdAt: this.createdAt.toISOString(),
            type: 'User'  // Add type information
        };
    }
}

const user = new User('Alice', 'alice@example.com', 'secret123');
console.log(JSON.stringify(user));  // Uses toJSON method

// Deep cloning with JSON (with limitations)
const original = {
    name: 'John',
    address: {
        street: '123 Main St',
        city: 'Boston'
    },
    hobbies: ['reading', 'coding']
};

// Simple deep clone (loses functions, dates, etc.)
const clone = JSON.parse(JSON.stringify(original));
clone.address.city = 'New York';
console.log(original.address.city);  // 'Boston' (unchanged)
console.log(clone.address.city);     // 'New York'

// Handling circular references
const objA = { name: 'A' };
const objB = { name: 'B' };
objA.ref = objB;
objB.ref = objA;  // Circular reference

// This would throw an error:
// JSON.stringify(objA);  // TypeError: Converting circular structure to JSON

// Solution: Use a WeakSet to track visited objects
function stringifyWithCircular(obj) {
    const seen = new WeakSet();
    return JSON.stringify(obj, (key, value) => {
        if (typeof value === 'object' && value !== null) {
            if (seen.has(value)) {
                return '[Circular]';
            }
            seen.add(value);
        }
        return value;
    });
}

console.log(stringifyWithCircular(objA));

// Working with nested objects and arrays
const complexData = {
    users: [
        { id: 1, name: 'Alice', roles: ['admin', 'user'] },
        { id: 2, name: 'Bob', roles: ['user'] }
    ],
    settings: {
        theme: 'dark',
        notifications: {
            email: true,
            push: false,
            sms: true
        }
    }
};

// Safe property access after parsing
const jsonData = JSON.stringify(complexData);
const parsed = JSON.parse(jsonData);

// Access nested properties safely
console.log(parsed.users[0].name);  // 'Alice'
console.log(parsed.settings.notifications.email);  // true

// Validating JSON before parsing
function safeJsonParse(jsonString, defaultValue = null) {
    try {
        return JSON.parse(jsonString);
    } catch (error) {
        console.error('Invalid JSON:', error.message);
        return defaultValue;
    }
}

console.log(safeJsonParse('{"valid": "json"}'));     // { valid: 'json' }
console.log(safeJsonParse('invalid json', {}));      // {} (default value)

// JSON schema validation pattern
function validateUserJson(jsonData) {
    const required = ['name', 'email'];
    const optional = ['age', 'city', 'hobbies'];
    
    for (const field of required) {
        if (!(field in jsonData)) {
            throw new Error(`Missing required field: ${field}`);
        }
    }
    
    // Validate types
    if (typeof jsonData.name !== 'string') {
        throw new Error('Name must be a string');
    }
    
    if (typeof jsonData.email !== 'string') {
        throw new Error('Email must be a string');
    }
    
    return true;
}

// Example usage
try {
    const userData = JSON.parse('{"name":"John","email":"john@example.com","age":30}');
    validateUserJson(userData);
    console.log('Valid user data');
} catch (error) {
    console.error('Validation failed:', error.message);
}
```