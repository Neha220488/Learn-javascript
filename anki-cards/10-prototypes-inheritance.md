# Prototypes and Inheritance in JavaScript

## Understanding Prototypes
JavaScript is a prototype-based language, where objects inherit properties and methods from prototypes.

**Q&A Format:**

**Q: What is the prototype chain in JavaScript?**
A: The prototype chain is JavaScript's inheritance mechanism where every object has a `prototype` property pointing to another object. When accessing a property that doesn't exist on an object, JavaScript searches up the chain: current object → its prototype → prototype's prototype → until reaching `null`.

**Q: How does property lookup work in the prototype chain?**
A: When you access a property, JavaScript follows this sequence:
1. Check if the property exists on the object itself
2. If not found, check the object's prototype
3. Continue up the chain until the property is found or reaching `null`
4. Return `undefined` if the property is never found

**Q: What happens when you set a property on an object that inherits it from its prototype?**
A: Setting a property creates it directly on the object (not the prototype), which "shadows" the inherited property. The prototype's property remains unchanged, but the object now uses its own property instead of the inherited one.

```javascript
// Creating an object
const person = {
    firstName: 'John',
    lastName: 'Doe',
    getFullName: function() {
        return this.firstName + ' ' + this.lastName;
    }
};

// Creating another object that inherits from person
const employee = Object.create(person);
employee.jobTitle = 'Developer';

console.log(employee.firstName);  // 'John' (inherited from person)
console.log(employee.jobTitle);   // 'Developer' (own property)
console.log(employee.getFullName());  // 'John Doe' (method inherited from person)

// Check if property exists directly on the object
console.log(employee.hasOwnProperty('firstName'));  // false
console.log(employee.hasOwnProperty('jobTitle'));   // true

// Setting a property on the child object
employee.firstName = 'Jane';
console.log(employee.firstName);     // 'Jane' (own property)
console.log(person.firstName);       // 'John' (unchanged)
console.log(employee.getFullName()); // 'Jane Doe' (using own firstName but inherited lastName)

// Viewing the prototype
console.log(Object.getPrototypeOf(employee) === person);  // true

// Every object ultimately inherits from Object.prototype
console.log(Object.getPrototypeOf(person) === Object.prototype);  // true

// The end of the prototype chain is null
console.log(Object.getPrototypeOf(Object.prototype));  // null

// Modifying the prototype affects all objects that inherit from it
person.greeting = 'Hello';
console.log(employee.greeting);  // 'Hello' (inherited from person)

// But adding a property with the same name to the child object shadows the prototype's property
employee.greeting = 'Hi';
console.log(employee.greeting);  // 'Hi' (own property)
console.log(person.greeting);    // 'Hello' (unchanged)
```

## Constructor Functions and the Prototype Pattern
Constructor functions provide a way to create multiple objects with the same structure and behavior.

**Q&A Format:**

**Q: What are constructor functions and how do they work?**
A: Constructor functions are regular functions used with the `new` keyword to create objects. Before ES6 classes, they were the primary way to create structured, reusable objects. Each constructor function has a `prototype` property that becomes the prototype for all instances created with it.

**Q: Why should methods be added to the prototype instead of inside the constructor?**
A: Adding methods to the prototype provides memory efficiency because all instances share the same method reference. If methods are defined inside the constructor, each instance gets its own copy, wasting memory and reducing performance.

**Q: How can you check if an object was created by a specific constructor?**
A: You can use:
- `instanceof` operator: `obj instanceof Constructor`
- `constructor` property: `obj.constructor === Constructor`
- `Object.getPrototypeOf()`: `Object.getPrototypeOf(obj) === Constructor.prototype`

```javascript
// Constructor function
function Person(firstName, lastName) {
    // Instance properties
    this.firstName = firstName;
    this.lastName = lastName;
}

// Adding a method to the prototype
Person.prototype.getFullName = function() {
    return this.firstName + ' ' + this.lastName;
};

// Creating instances
const john = new Person('John', 'Doe');
const jane = new Person('Jane', 'Smith');

console.log(john.firstName);      // 'John'
console.log(jane.getFullName());  // 'Jane Smith'

// All instances share the same method
console.log(john.getFullName === jane.getFullName);  // true

// Adding methods to the prototype later
Person.prototype.greet = function() {
    return `Hello, my name is ${this.getFullName()}`;
};

console.log(john.greet());  // 'Hello, my name is John Doe'
console.log(jane.greet());  // 'Hello, my name is Jane Smith'

// Each instance has its own properties
john.firstName = 'Johnny';
console.log(john.firstName);      // 'Johnny'
console.log(jane.firstName);      // 'Jane' (unchanged)
console.log(john.getFullName());  // 'Johnny Doe'

// Check the constructor of an object
console.log(john.constructor === Person);  // true

// Check if an object is an instance of a constructor
console.log(john instanceof Person);  // true
console.log(john instanceof Object);  // true (all objects inherit from Object)

// The prototype chain
console.log(Object.getPrototypeOf(john) === Person.prototype);  // true
console.log(Object.getPrototypeOf(Person.prototype) === Object.prototype);  // true

// Why use prototypes? Memory efficiency
function InefficientPerson(name) {
    this.name = name;
    // Each instance gets its own copy of the method
    this.greet = function() {
        return `Hello, I'm ${this.name}`;
    };
}

function EfficientPerson(name) {
    this.name = name;
    // No methods defined here
}

// All instances share a single copy of the method
EfficientPerson.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

const inefficientPeople = Array(1000).fill(0).map(() => new InefficientPerson('Test'));
const efficientPeople = Array(1000).fill(0).map(() => new EfficientPerson('Test'));

// The efficient approach uses significantly less memory
// and has better performance for object creation
```

## Prototypal Inheritance
Prototypal inheritance allows objects to inherit properties and methods from other objects.

**Q&A Format:**

**Q: How is inheritance implemented in JavaScript?**
A: JavaScript implements inheritance through the prototype chain. Objects inherit from other objects by having their prototype point to the parent object. This creates a delegation chain where properties and methods are looked up through the prototype hierarchy.

**Q: What are the key steps to set up prototypal inheritance with constructor functions?**
A: 1. Create child constructor and call parent constructor with `Parent.call(this, ...args)`
2. Set child prototype to inherit from parent: `Child.prototype = Object.create(Parent.prototype)`
3. Fix the constructor property: `Child.prototype.constructor = Child`
4. Add or override methods on `Child.prototype`

**Q: What's the difference between overriding and extending in prototypal inheritance?**
A: 
- **Overriding**: Redefining a method with the same name replaces the parent's implementation
- **Extending**: Adding new methods or properties that don't exist in the parent
Both are done by adding to the child's prototype after setting up inheritance.

```javascript
// Base constructor
function Animal(name) {
    this.name = name;
}

// Base prototype method
Animal.prototype.makeSound = function() {
    return 'Some generic sound';
};

// Child constructor
function Dog(name, breed) {
    // Call the parent constructor
    Animal.call(this, name);
    this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
// Fix the constructor property
Dog.prototype.constructor = Dog;

// Override a method
Dog.prototype.makeSound = function() {
    return 'Woof!';
};

// Add a new method
Dog.prototype.fetch = function() {
    return `${this.name} is fetching.`;
};

// Create instances
const animal = new Animal('Generic Animal');
const dog = new Dog('Rex', 'German Shepherd');

console.log(animal.name);         // 'Generic Animal'
console.log(dog.name);            // 'Rex'
console.log(dog.breed);           // 'German Shepherd'
console.log(animal.makeSound());  // 'Some generic sound'
console.log(dog.makeSound());     // 'Woof!'
console.log(dog.fetch());         // 'Rex is fetching.'

// Check the inheritance
console.log(dog instanceof Dog);     // true
console.log(dog instanceof Animal);  // true
console.log(dog instanceof Object);  // true

// Another child constructor
function Cat(name, color) {
    Animal.call(this, name);
    this.color = color;
}

// Set up inheritance for Cat
Cat.prototype = Object.create(Animal.prototype);
Cat.prototype.constructor = Cat;

// Override makeSound for Cat
Cat.prototype.makeSound = function() {
    return 'Meow!';
};

// Add a unique method
Cat.prototype.climb = function() {
    return `${this.name} is climbing.`;
};

const cat = new Cat('Whiskers', 'gray');
console.log(cat.name);         // 'Whiskers'
console.log(cat.makeSound());  // 'Meow!'
console.log(cat.climb());      // 'Whiskers is climbing.'

// Dogs and cats are both animals but have different behaviors
console.log(dog instanceof Animal);  // true
console.log(cat instanceof Animal);  // true
console.log(dog instanceof Cat);     // false
console.log(cat instanceof Dog);     // false

// The complete prototype chain
console.log(Object.getPrototypeOf(dog) === Dog.prototype);  // true
console.log(Object.getPrototypeOf(Dog.prototype) === Animal.prototype);  // true
console.log(Object.getPrototypeOf(Animal.prototype) === Object.prototype);  // true
console.log(Object.getPrototypeOf(Object.prototype) === null);  // true
```

## ES6 Classes and Inheritance
ES6 introduced a more familiar syntax for creating objects and implementing inheritance through the `class` keyword.

**Q&A Format:**

**Q: How do ES6 classes differ from constructor functions?**
A: ES6 classes provide cleaner syntax but use the same prototypal inheritance underneath. Key differences:
- Cleaner syntax with `class` keyword and `constructor` method
- `extends` keyword for inheritance instead of manual prototype setup
- `super()` to call parent constructor and methods
- Built-in support for getters, setters, and static methods

**Q: What is the `super` keyword and when do you use it?**
A: `super` is used in child classes to:
- Call the parent constructor: `super(args)` (required in constructor if extending)
- Call parent methods: `super.methodName()`
- Access parent properties when needed
It must be called before using `this` in a child constructor.

**Q: What are static methods in ES6 classes?**
A: Static methods belong to the class itself, not instances. They're called on the class directly (`ClassName.method()`) and are useful for utility functions, factory methods, or operations that don't require instance data.

```javascript
// Base class
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    makeSound() {
        return 'Some generic sound';
    }
    
    // Getter
    get description() {
        return `Animal named ${this.name}`;
    }
}

// Child class
class Dog extends Animal {
    constructor(name, breed) {
        super(name);  // Call parent constructor
        this.breed = breed;
    }
    
    // Override parent method
    makeSound() {
        return 'Woof!';
    }
    
    // New method
    fetch() {
        return `${this.name} is fetching.`;
    }
    
    // Override parent getter
    get description() {
        return `Dog named ${this.name}, breed: ${this.breed}`;
    }
}

// Create instances
const animal = new Animal('Generic Animal');
const dog = new Dog('Rex', 'German Shepherd');

console.log(animal.name);         // 'Generic Animal'
console.log(dog.name);            // 'Rex'
console.log(dog.breed);           // 'German Shepherd'
console.log(animal.makeSound());  // 'Some generic sound'
console.log(dog.makeSound());     // 'Woof!'
console.log(dog.fetch());         // 'Rex is fetching.'
console.log(animal.description);  // 'Animal named Generic Animal'
console.log(dog.description);     // 'Dog named Rex, breed: German Shepherd'

// Check inheritance
console.log(dog instanceof Dog);     // true
console.log(dog instanceof Animal);  // true
console.log(dog instanceof Object);  // true

// Another child class
class Cat extends Animal {
    constructor(name, color) {
        super(name);
        this.color = color;
    }
    
    makeSound() {
        return 'Meow!';
    }
    
    climb() {
        return `${this.name} is climbing.`;
    }
}

const cat = new Cat('Whiskers', 'gray');
console.log(cat.name);         // 'Whiskers'
console.log(cat.makeSound());  // 'Meow!'
console.log(cat.climb());      // 'Whiskers is climbing.'

// Static methods
class MathUtils {
    static add(a, b) {
        return a + b;
    }
    
    static multiply(a, b) {
        return a * b;
    }
}

console.log(MathUtils.add(5, 3));       // 8
console.log(MathUtils.multiply(5, 3));  // 15

// Class with static and instance methods
class Counter {
    constructor() {
        this.count = 0;
    }
    
    increment() {
        this.count++;
        return this.count;
    }
    
    static createDefault() {
        return new Counter();
    }
}

const counter1 = new Counter();
const counter2 = Counter.createDefault();

console.log(counter1.increment());  // 1
console.log(counter2.increment());  // 1

// ES2022 private fields and methods
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
    
    // Public methods
    getBalance() {
        return this.#balance;
    }
    
    deposit(amount) {
        if (amount <= 0) {
            throw new Error('Deposit amount must be positive');
        }
        this.#deposit(amount);
        return this.#balance;
    }
    
    withdraw(amount) {
        if (amount <= 0) {
            throw new Error('Withdrawal amount must be positive');
        }
        if (amount > this.#balance) {
            throw new Error('Insufficient funds');
        }
        this.#balance -= amount;
        return this.#balance;
    }
}

const account = new BankAccount('John', 1000);
console.log(account.getBalance());  // 1000
account.deposit(500);
console.log(account.getBalance());  // 1500
account.withdraw(200);
console.log(account.getBalance());  // 1300
// console.log(account.#balance);  // SyntaxError: Private field
// account.#deposit(100);  // SyntaxError: Private method
```

## Object Composition
Object composition is a design pattern where objects are composed from smaller, focused objects rather than through inheritance.

**Q&A Format:**

**Q: What is object composition and why is it preferred over inheritance in many cases?**
A: Object composition builds objects by combining smaller, focused objects rather than inheriting from a parent class. It's often preferred because it:
- Provides more flexibility (compose only needed behaviors)
- Avoids inheritance hierarchies that become complex
- Follows "has-a" relationships instead of "is-a"
- Makes code more modular and testable

**Q: What are the main techniques for implementing composition in JavaScript?**
A: 
- **Object.assign()**: `Object.assign(target, ...sources)` copies properties from source objects
- **Object spread**: `{...obj1, ...obj2}` spreads properties from multiple objects
- **Factory functions**: Functions that return composed objects with specific behaviors
- **Mixins**: Objects containing methods that can be mixed into other objects

**Q: How does composition solve the problems of deep inheritance hierarchies?**
A: Composition avoids the "diamond problem" and rigid hierarchies by allowing objects to pick and choose exactly the behaviors they need. Instead of inheriting everything from a parent (including unwanted methods), objects compose only the specific capabilities required.

```javascript
// Creating behavior objects
const swimmer = {
    swim() {
        return `${this.name} is swimming.`;
    }
};

const flyer = {
    fly() {
        return `${this.name} is flying.`;
    }
};

const walker = {
    walk() {
        return `${this.name} is walking.`;
    }
};

// Composing objects using Object.assign
function createDuck(name) {
    return Object.assign(
        { name },
        swimmer,
        flyer,
        walker,
        {
            quack() {
                return `${this.name} says: Quack!`;
            }
        }
    );
}

const duck = createDuck('Donald');
console.log(duck.swim());   // 'Donald is swimming.'
console.log(duck.fly());    // 'Donald is flying.'
console.log(duck.walk());   // 'Donald is walking.'
console.log(duck.quack());  // 'Donald says: Quack!'

// Using object spread (ES6+)
function createPenguin(name) {
    return {
        name,
        ...swimmer,
        ...walker,
        slide() {
            return `${this.name} is sliding on ice.`;
        }
    };
}

const penguin = createPenguin('Pingu');
console.log(penguin.swim());   // 'Pingu is swimming.'
console.log(penguin.walk());   // 'Pingu is walking.'
console.log(penguin.slide());  // 'Pingu is sliding on ice.'
// console.log(penguin.fly());  // Error: penguin.fly is not a function

// Composition with factory functions
function createAnimal(name) {
    return {
        name,
        getName() {
            return this.name;
        }
    };
}

function addSwimming(animal) {
    return {
        ...animal,
        swim() {
            return `${this.name} is swimming.`;
        }
    };
}

function addFlying(animal) {
    return {
        ...animal,
        fly() {
            return `${this.name} is flying.`;
        }
    };
}

// Create a flying fish
const flyingFish = addFlying(addSwimming(createAnimal('Finley')));
console.log(flyingFish.getName());  // 'Finley'
console.log(flyingFish.swim());     // 'Finley is swimming.'
console.log(flyingFish.fly());      // 'Finley is flying.'

// Composition with specific-purpose objects
const userMethods = {
    getFullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    getInitials() {
        return `${this.firstName[0]}${this.lastName[0]}`;
    }
};

const addressMethods = {
    getAddress() {
        return `${this.street}, ${this.city}, ${this.country}`;
    },
    getCity() {
        return this.city;
    }
};

function createPerson(firstName, lastName, street, city, country) {
    return {
        firstName,
        lastName,
        street,
        city,
        country,
        ...userMethods,
        ...addressMethods
    };
}

const person = createPerson('John', 'Doe', '123 Main St', 'New York', 'USA');
console.log(person.getFullName());  // 'John Doe'
console.log(person.getInitials());  // 'JD'
console.log(person.getAddress());   // '123 Main St, New York, USA'

// Composition with classes
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    getName() {
        return this.name;
    }
}

// Mixin factory functions
function SwimmerMixin(Base) {
    return class extends Base {
        swim() {
            return `${this.name} is swimming.`;
        }
    };
}

function FlyerMixin(Base) {
    return class extends Base {
        fly() {
            return `${this.name} is flying.`;
        }
    };
}

// Compose a flying, swimming animal
const FlyingSwimmingAnimal = FlyerMixin(SwimmerMixin(Animal));
const flyingSwimmingAnimal = new FlyingSwimmingAnimal('Multi-talented');

console.log(flyingSwimmingAnimal.getName());  // 'Multi-talented'
console.log(flyingSwimmingAnimal.swim());     // 'Multi-talented is swimming.'
console.log(flyingSwimmingAnimal.fly());      // 'Multi-talented is flying.'
```

## The Prototype Chain
The prototype chain is the mechanism JavaScript uses to look up properties and methods on objects.

**Q&A Format:**

**Q: How does JavaScript's property lookup work in the prototype chain?**
A: When accessing a property, JavaScript follows this sequence:
1. Check if the property exists directly on the object
2. If not found, check the object's prototype (`__proto__` or `[[Prototype]]`)
3. Continue up the chain checking each prototype
4. Stop when the property is found or when reaching `null` (end of chain)
5. Return `undefined` if property is never found

**Q: What is the typical structure of the prototype chain for most JavaScript objects?**
A: For most objects created with constructors or classes:
`instance → Constructor.prototype → Object.prototype → null`

For objects created with `Object.create()`:
`object → specified prototype → Object.prototype → null`

**Q: How can you inspect and modify the prototype chain?**
A: Key methods include:
- `Object.getPrototypeOf(obj)`: Get object's prototype
- `Object.setPrototypeOf(obj, proto)`: Set object's prototype (not recommended for performance)
- `obj.hasOwnProperty(prop)`: Check if property exists directly on object
- `obj.isPrototypeOf(other)`: Check if object is in another's prototype chain

```javascript
// Creating objects with different prototypes
const animal = {
    eats: true,
    sleep() {
        return 'Sleeping...';
    }
};

const dog = Object.create(animal);
dog.barks = true;
dog.bark = function() {
    return 'Woof!';
};

const labrador = Object.create(dog);
labrador.color = 'yellow';
labrador.fetch = function() {
    return 'Fetching...';
};

// Property lookup follows the prototype chain
console.log(labrador.color);     // 'yellow' (own property)
console.log(labrador.barks);     // true (inherited from dog)
console.log(labrador.eats);      // true (inherited from animal)
console.log(labrador.fetch());   // 'Fetching...' (own method)
console.log(labrador.bark());    // 'Woof!' (inherited from dog)
console.log(labrador.sleep());   // 'Sleeping...' (inherited from animal)

// Visualizing the prototype chain
console.log(Object.getPrototypeOf(labrador) === dog);  // true
console.log(Object.getPrototypeOf(dog) === animal);    // true
console.log(Object.getPrototypeOf(animal) === Object.prototype);  // true
console.log(Object.getPrototypeOf(Object.prototype) === null);  // true

// hasOwnProperty checks if a property exists directly on the object
console.log(labrador.hasOwnProperty('color'));  // true
console.log(labrador.hasOwnProperty('barks'));  // false
console.log(dog.hasOwnProperty('barks'));       // true

// isPrototypeOf checks if an object exists in another object's prototype chain
console.log(animal.isPrototypeOf(labrador));  // true
console.log(dog.isPrototypeOf(labrador));     // true
console.log(labrador.isPrototypeOf(dog));     // false

// Properties added to Object.prototype are available to all objects
Object.prototype.commonMethod = function() {
    return 'This method is available to all objects';
};

console.log(labrador.commonMethod());  // 'This method is available to all objects'
console.log({}.commonMethod());        // 'This method is available to all objects'

// But extending Object.prototype is generally considered bad practice
delete Object.prototype.commonMethod;  // Clean up

// Method overriding
dog.sleep = function() {
    return 'Dog sleeping...';
};

console.log(animal.sleep());  // 'Sleeping...'
console.log(dog.sleep());     // 'Dog sleeping...'
console.log(labrador.sleep());  // 'Dog sleeping...' (inherited from dog, not animal)

// Shadow properties
labrador.eats = false;
console.log(animal.eats);   // true (unchanged)
console.log(dog.eats);      // true (inherited from animal)
console.log(labrador.eats); // false (own property shadows prototype)

// Checking if a property exists in the prototype chain
console.log('toString' in labrador);  // true (inherited from Object.prototype)
console.log(labrador.hasOwnProperty('toString'));  // false

// Getting all own property names
console.log(Object.getOwnPropertyNames(labrador));  // ['color', 'fetch', 'eats']

// Getting all keys (enumerable own properties)
console.log(Object.keys(labrador));  // ['color', 'fetch', 'eats']

// Iterating over own properties
for (const key in labrador) {
    if (labrador.hasOwnProperty(key)) {
        console.log(`Own property: ${key}`);
    } else {
        console.log(`Inherited property: ${key}`);
    }
}
```

## Property Descriptors and Object Configuration
JavaScript provides ways to control how properties behave and how objects can be modified.

**Q&A Format:**

**Q: What are property descriptors and what attributes can they control?**
A: Property descriptors define how properties behave with four attributes:
- **value**: The property's value
- **writable**: Whether the value can be changed
- **enumerable**: Whether the property appears in `for...in` loops and `Object.keys()`
- **configurable**: Whether the property can be deleted or its descriptor modified

**Q: How do you create properties with specific behaviors using descriptors?**
A: Use `Object.defineProperty()` for single properties or `Object.defineProperties()` for multiple:
```javascript
Object.defineProperty(obj, 'prop', {
    value: 'value',
    writable: false,    // Read-only
    enumerable: false,  // Hidden from enumeration
    configurable: true  // Can be deleted/reconfigured
});
```

**Q: What's the difference between getters/setters and regular properties?**
A: 
- **Regular properties**: Store actual values directly
- **Getters/setters**: Define accessor properties that execute functions when accessed or assigned
- Getters/setters allow computed properties, validation, and encapsulation
- They appear as normal properties but execute code behind the scenes

```javascript
// Property Descriptors
const person = {};

// Define a property with a descriptor
Object.defineProperty(person, 'name', {
    value: 'John',
    writable: true,      // Can be changed
    enumerable: true,    // Shows up in loops
    configurable: true   // Can be deleted or modified
});

console.log(person.name);  // 'John'

// Get a property descriptor
const nameDescriptor = Object.getOwnPropertyDescriptor(person, 'name');
console.log(nameDescriptor);
// { value: 'John', writable: true, enumerable: true, configurable: true }

// Non-writable property
Object.defineProperty(person, 'id', {
    value: 12345,
    writable: false,     // Cannot be changed
    enumerable: true,
    configurable: true
});

console.log(person.id);  // 12345
person.id = 54321;       // This will fail silently in non-strict mode
console.log(person.id);  // Still 12345

// Non-enumerable property
Object.defineProperty(person, 'ssn', {
    value: '123-45-6789',
    writable: true,
    enumerable: false,   // Won't show up in loops
    configurable: true
});

console.log(person.ssn);  // '123-45-6789'
console.log(Object.keys(person));  // ['name', 'id'] (ssn is not enumerable)

// Non-configurable property
Object.defineProperty(person, 'species', {
    value: 'human',
    writable: true,
    enumerable: true,
    configurable: false  // Cannot be deleted or reconfigured
});

// This will fail
// delete person.species;
// console.log(person.species);  // Still 'human'

// This will also fail
// Object.defineProperty(person, 'species', {
//     configurable: true
// });

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

console.log(`${person.firstName} ${person.lastName}`);  // 'John Doe'

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
    }
};

account.balance = 1000;
console.log(account.balance);  // '$1000'

// try {
//     account.balance = -500;  // Error: Balance cannot be negative
// } catch (e) {
//     console.error(e.message);
// }

// Define a property with getter/setter
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

// Object Configuration

// 1. Object.preventExtensions() - prevents adding new properties
const preventExtensions = { a: 1, b: 2 };
Object.preventExtensions(preventExtensions);

// This will fail
// preventExtensions.c = 3;
console.log(preventExtensions.c);  // undefined
console.log(Object.isExtensible(preventExtensions));  // false

// 2. Object.seal() - prevents adding or deleting properties
const sealed = { x: 1, y: 2 };
Object.seal(sealed);

sealed.x = 10;  // Can still modify existing properties
console.log(sealed.x);  // 10

// These will fail
// sealed.z = 3;      // Can't add new properties
// delete sealed.x;   // Can't delete properties
console.log(Object.isSealed(sealed));  // true

// 3. Object.freeze() - prevents any changes to the object
const frozen = { p: 1, q: 2 };
Object.freeze(frozen);

// These will all fail
// frozen.p = 10;     // Can't modify properties
// frozen.r = 3;      // Can't add properties
// delete frozen.p;   // Can't delete properties
console.log(Object.isFrozen(frozen));  // true

// Deep freeze (recursive)
function deepFreeze(obj) {
    // Get all properties, including non-enumerable ones
    const propNames = Object.getOwnPropertyNames(obj);
    
    // Freeze properties before freezing the object
    for (const name of propNames) {
        const value = obj[name];
        
        if (value && typeof value === 'object') {
            deepFreeze(value);
        }
    }
    
    return Object.freeze(obj);
}

const person2 = {
    name: 'John',
    address: {
        city: 'New York',
        country: 'USA'
    }
};

deepFreeze(person2);

// These will fail
// person2.name = 'Jane';            // Can't modify top-level property
// person2.address.city = 'Boston';  // Can't modify nested property
console.log(person2.address.city);  // Still 'New York'
```

## Native Prototypes
All built-in JavaScript objects have their own prototypes that provide default methods and properties.

**Q&A Format:**

**Q: What are native prototypes and what do they provide?**
A: Native prototypes are the prototypes of built-in JavaScript objects (Array, String, Number, Function, Object, etc.). They provide the default methods and properties that we commonly use on these types. For example, `Array.prototype` contains methods like `map()`, `filter()`, and `forEach()`.

**Q: How is the prototype chain structured for built-in types?**
A: Most built-in types follow this pattern:
- `String/Array/Number/Function.prototype → Object.prototype → null`
- All objects ultimately inherit from `Object.prototype`
- Primitives are temporarily wrapped in their object equivalents to access prototype methods

**Q: Should you extend native prototypes, and what are the alternatives?**
A: **Generally no** - extending native prototypes can cause:
- Conflicts with future JavaScript features
- Conflicts with other libraries
- Unpredictable behavior

**Better alternatives:**
- Utility functions: `capitalize(str)` instead of `String.prototype.capitalize`
- Helper objects: `StringUtils.capitalize(str)`
- Use established libraries like Lodash or Ramda

```javascript
// String prototype
console.log(String.prototype);  // String { ... } - contains all string methods

// All strings inherit from String.prototype
const str = 'Hello';
console.log(str.charAt(0));      // 'H' - method from String.prototype
console.log(str.__proto__ === String.prototype);  // true

// Array prototype
console.log(Array.prototype);  // Array [ ... ] - contains all array methods

// All arrays inherit from Array.prototype
const arr = [1, 2, 3];
console.log(arr.map(x => x * 2));  // [2, 4, 6] - method from Array.prototype
console.log(arr.__proto__ === Array.prototype);  // true

// Number prototype
console.log(Number.prototype);  // Number { ... } - contains all number methods

// All numbers inherit from Number.prototype
const num = 42;
console.log(num.toFixed(2));  // '42.00' - method from Number.prototype
console.log(num.__proto__ === Number.prototype);  // false (primitive vs. object)
console.log(Number(42).__proto__ === Number.prototype);  // true

// Function prototype
console.log(Function.prototype);  // Function { ... } - contains all function methods

// All functions inherit from Function.prototype
function fn() {}
console.log(fn.bind);  // function bind() { ... } - method from Function.prototype
console.log(fn.__proto__ === Function.prototype);  // true

// Object prototype
console.log(Object.prototype);  // Object { ... } - contains all object methods

// All objects ultimately inherit from Object.prototype
console.log({});  // {} - inherits from Object.prototype
console.log({}.__proto__ === Object.prototype);  // true

// The prototype chain for arrays
console.log(arr.__proto__ === Array.prototype);  // true
console.log(arr.__proto__.__proto__ === Object.prototype);  // true
console.log(arr.__proto__.__proto__.__proto__ === null);  // true

// Extending native prototypes (generally not recommended)

// Adding a method to Array.prototype
Array.prototype.first = function() {
    return this[0];
};

const numbers = [10, 20, 30];
console.log(numbers.first());  // 10

// Adding a method to String.prototype
String.prototype.capitalize = function() {
    return this.charAt(0).toUpperCase() + this.slice(1);
};

console.log('hello'.capitalize());  // 'Hello'

// Why extending native prototypes is generally discouraged:
// 1. Conflicts with future JavaScript features
// 2. Conflicts with other libraries
// 3. Makes code less predictable
// 4. Breaks the "isomorphic JavaScript" concept

// Better alternatives:

// 1. Use utility functions
function first(array) {
    return array[0];
}

function capitalize(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
}

console.log(first(numbers));        // 10
console.log(capitalize('world'));   // 'World'

// 2. Use a wrapper/helper object
const ArrayUtils = {
    first(array) {
        return array[0];
    },
    last(array) {
        return array[array.length - 1];
    }
};

const StringUtils = {
    capitalize(str) {
        return str.charAt(0).toUpperCase() + str.slice(1);
    },
    reverse(str) {
        return str.split('').reverse().join('');
    }
};

console.log(ArrayUtils.first(numbers));     // 10
console.log(StringUtils.capitalize('world'));  // 'World'

// 3. Use modern alternatives like lodash, Ramda, etc.
// import _ from 'lodash';
// console.log(_.first(numbers));  // 10
// console.log(_.capitalize('world'));  // 'World'
```
