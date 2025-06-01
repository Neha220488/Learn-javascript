# Control Flow in JavaScript

## if Statement
The `if` statement executes a block of code if a specified condition is true.

**Q&A Format:**

**Q: How does the if statement evaluate conditions and what values are considered truthy/falsy?**
A: The if statement evaluates conditions to boolean values:
- **Falsy values**: `false`, `0`, `""`, `null`, `undefined`, `NaN`, `-0`, `0n`
- **Truthy values**: Everything else (including `"0"`, `[]`, `{}`)
- Condition is automatically converted to boolean using JavaScript's truthiness rules

**Q: What are the different forms of conditional statements in JavaScript?**
A: JavaScript provides several conditional forms:
- **if**: Execute code if condition is true
- **if...else**: Execute one block or another
- **if...else if...else**: Chain multiple conditions
- **Ternary operator**: `condition ? value1 : value2` for simple conditionals

**Q: What are common patterns and best practices for using if statements?**
A: Common if statement patterns:
- **Guard clauses**: Early returns to handle edge cases first
- **Default values**: Check existence before assignment
- **Validation**: Verify data before processing
- **Avoid nested conditions**: Use logical operators and early returns for cleaner code

```javascript
// Basic if statement
let hour = 10;

if (hour < 12) {
    console.log("Good morning!");
}

// Using multiple conditions with logical operators
let hour = 10;
let isWeekend = true;

if (hour < 12 && !isWeekend) {
    console.log("Good morning! Time to go to work.");
}

// if statement with block of multiple statements
let score = 85;

if (score >= 70) {
    console.log("You passed!");
    console.log("Your score is: " + score);
    let grade = "B";
    if (score >= 90) {
        grade = "A";
    }
    console.log("Grade: " + grade);
}

// Truthy and falsy values
// The condition is automatically converted to a boolean
// The following values are falsy: false, 0, "", null, undefined, NaN
// All other values are truthy
let username = "John";
if (username) {  // Non-empty string is truthy
    console.log("Hello, " + username);
}

let noUser = "";
if (noUser) {  // This block won't execute because empty string is falsy
    console.log("This won't show");
}

// Common patterns
// 1. Checking if a variable exists before using it
let user;
if (user) {
    console.log("User: " + user.name);
}

// 2. Setting default values
let count;
if (!count) {
    count = 0;
}

// 3. Checking for valid array/object properties
let users = ["John", "Jane"];
if (users.length > 0) {
    console.log("First user: " + users[0]);
}

// 4. Early return pattern (in functions)
function greet(name) {
    if (!name) {
        return "Hello, Guest!";
    }
    return "Hello, " + name + "!";
}
```

## else and else if Statements
The `else` and `else if` statements provide alternative execution paths for conditional logic.

**Q&A Format:**

**Q: How do else and else if statements work together with if statements?**
A: They create branching logic:
- **else**: Executes when the if condition is false
- **else if**: Allows testing multiple conditions in sequence
- **Order matters**: Conditions are evaluated top to bottom, first true condition wins
- **Only one branch executes**: Once a condition is met, remaining conditions are skipped

**Q: What are best practices for structuring if-else chains?**
A: Best practices for if-else chains:
- **Most likely conditions first**: Put common cases at the top for performance
- **Specific to general**: Handle specific cases before general ones
- **Avoid deep nesting**: Use early returns or logical operators
- **Consider switch**: For many discrete values, switch might be cleaner

```javascript
// Basic if-else
let hour = 14;

if (hour < 12) {
    console.log("Good morning!");
} else {
    console.log("Good afternoon!");
}

// Multiple conditions with else if
let score = 87;
let grade;

if (score >= 90) {
    grade = "A";
} else if (score >= 80) {
    grade = "B";
} else if (score >= 70) {
    grade = "C";
} else if (score >= 60) {
    grade = "D";
} else {
    grade = "F";
}

console.log(`Your grade is: ${grade}`);

// Complex conditional logic
let age = 25;
let hasLicense = true;
let hasInsurance = false;

if (age >= 18) {
    if (hasLicense) {
        if (hasInsurance) {
            console.log("You can drive!");
        } else {
            console.log("You need insurance to drive.");
        }
    } else {
        console.log("You need a license to drive.");
    }
} else {
    console.log("You must be 18 or older to drive.");
}

// Better version using logical operators
if (age >= 18 && hasLicense && hasInsurance) {
    console.log("You can drive!");
} else if (age < 18) {
    console.log("You must be 18 or older to drive.");
} else if (!hasLicense) {
    console.log("You need a license to drive.");
} else if (!hasInsurance) {
    console.log("You need insurance to drive.");
}

// Guard clause pattern (recommended)
function canDrive(age, hasLicense, hasInsurance) {
    if (age < 18) {
        return "You must be 18 or older to drive.";
    }
    if (!hasLicense) {
        return "You need a license to drive.";
    }
    if (!hasInsurance) {
        return "You need insurance to drive.";
    }
    return "You can drive!";
}

// Ternary operator for simple conditions
let status = age >= 18 ? "adult" : "minor";
let message = hasLicense ? "Can drive" : "Cannot drive";

// Nested ternary (use sparingly)
let category = age < 13 ? "child" : age < 18 ? "teenager" : "adult";

// Better approach for complex conditions
function getAgeCategory(age) {
    if (age < 13) return "child";
    if (age < 18) return "teenager";
    return "adult";
}
```

## switch Statement
The `switch` statement provides a clean way to handle multiple discrete values.

**Q&A Format:**

**Q: When should you use switch instead of if-else chains?**
A: Use switch when:
- **Testing one variable**: Against multiple specific values
- **Discrete values**: Like strings, numbers, or enums (not ranges)
- **Many conditions**: More than 3-4 conditions make switch cleaner
- **Performance**: Switch can be faster for many conditions

**Q: What are the key rules and gotchas of switch statements?**
A: Important switch statement rules:
- **Strict equality**: Uses `===` comparison (no type coercion)
- **Break statements**: Required to prevent fall-through
- **Fall-through**: Can be intentional for shared logic
- **Default case**: Optional catch-all, can be anywhere (conventionally last)

```javascript
// Basic switch statement
let day = "Monday";

switch (day) {
    case "Monday":
        console.log("Start of the work week");
        break;
    case "Tuesday":
        console.log("Getting into the groove");
        break;
    case "Wednesday":
        console.log("Hump day!");
        break;
    case "Thursday":
        console.log("Almost there");
        break;
    case "Friday":
        console.log("TGIF!");
        break;
    case "Saturday":
    case "Sunday":
        console.log("Weekend time!");
        break;
    default:
        console.log("Invalid day");
}

// Intentional fall-through for shared logic
let month = "February";
let days;

switch (month) {
    case "January":
    case "March":
    case "May":
    case "July":
    case "August":
    case "October":
    case "December":
        days = 31;
        break;
    case "April":
    case "June":
    case "September":
    case "November":
        days = 30;
        break;
    case "February":
        days = 28; // Ignoring leap years for simplicity
        break;
    default:
        days = 0;
        console.log("Invalid month");
}

console.log(`${month} has ${days} days`);

// Switch with return statements (no break needed)
function getSeasonByMonth(month) {
    switch (month) {
        case "December":
        case "January":
        case "February":
            return "Winter";
        case "March":
        case "April":
        case "May":
            return "Spring";
        case "June":
        case "July":
        case "August":
            return "Summer";
        case "September":
        case "October":
        case "November":
            return "Fall";
        default:
            return "Invalid month";
    }
}

// Switch with expressions (modern pattern)
function getGradeDescription(grade) {
    switch (grade) {
        case "A":
            return "Excellent work!";
        case "B":
            return "Good job!";
        case "C":
            return "Satisfactory";
        case "D":
            return "Needs improvement";
        case "F":
            return "Please see instructor";
        default:
            return "Invalid grade";
    }
}

// Numbers in switch
let statusCode = 404;

switch (statusCode) {
    case 200:
        console.log("OK - Success");
        break;
    case 201:
        console.log("Created");
        break;
    case 400:
        console.log("Bad Request");
        break;
    case 401:
        console.log("Unauthorized");
        break;
    case 404:
        console.log("Not Found");
        break;
    case 500:
        console.log("Internal Server Error");
        break;
    default:
        console.log("Unknown status code");
}

// Dangerous: Missing breaks (unintentional fall-through)
let action = "save";

switch (action) {
    case "save":
        console.log("Saving...");
        // Missing break! Falls through to next case
    case "load":
        console.log("Loading..."); // This will also execute!
        break;
    case "delete":
        console.log("Deleting...");
        break;
}
// Output: "Saving..." and "Loading..."
```

## for Loop
The `for` loop provides a compact way to repeat code with initialization, condition, and increment.

**Q&A Format:**

**Q: What are the three parts of a for loop and what does each do?**
A: The for loop has three parts:
- **Initialization**: `let i = 0` - executed once before the loop starts
- **Condition**: `i < 10` - checked before each iteration
- **Increment**: `i++` - executed after each iteration
- **Syntax**: `for (initialization; condition; increment) { ... }`

**Q: What are the different types of for loops available in JavaScript?**
A: JavaScript has several for loop variants:
- **Classic for**: `for (let i = 0; i < arr.length; i++)` - index-based iteration
- **for...in**: `for (let key in obj)` - iterates over object keys/array indices
- **for...of**: `for (let value of arr)` - iterates over iterable values (ES6)
- **forEach**: `arr.forEach()` - array method for functional iteration

```javascript
// Basic for loop
console.log("Counting from 1 to 5:");
for (let i = 1; i <= 5; i++) {
    console.log(i);
}

// Iterating through an array
let fruits = ["apple", "banana", "orange", "grape"];

console.log("Fruits list:");
for (let i = 0; i < fruits.length; i++) {
    console.log(`${i + 1}. ${fruits[i]}`);
}

// Reverse iteration
console.log("Fruits in reverse:");
for (let i = fruits.length - 1; i >= 0; i--) {
    console.log(fruits[i]);
}

// Step by different amounts
console.log("Even numbers from 0 to 10:");
for (let i = 0; i <= 10; i += 2) {
    console.log(i);
}

// Multiple variables in for loop
console.log("Convergence:");
for (let i = 0, j = 10; i <= j; i++, j--) {
    console.log(`i: ${i}, j: ${j}`);
}

// Nested for loops
console.log("Multiplication table:");
for (let i = 1; i <= 3; i++) {
    for (let j = 1; j <= 3; j++) {
        console.log(`${i} × ${j} = ${i * j}`);
    }
}

// for...in loop (for object properties)
let person = {
    name: "John",
    age: 30,
    city: "New York"
};

console.log("Person properties:");
for (let key in person) {
    console.log(`${key}: ${person[key]}`);
}

// for...in with arrays (not recommended, but works)
console.log("Array indices with for...in:");
for (let index in fruits) {
    console.log(`Index ${index}: ${fruits[index]}`);
}

// for...of loop (ES6) - for iterable values
console.log("Fruits with for...of:");
for (let fruit of fruits) {
    console.log(fruit);
}

// for...of with index using entries()
console.log("Fruits with index:");
for (let [index, fruit] of fruits.entries()) {
    console.log(`${index}: ${fruit}`);
}

// for...of with strings
let message = "Hello";
console.log("Characters in 'Hello':");
for (let char of message) {
    console.log(char);
}

// Break and continue in loops
console.log("Numbers 1-10, skipping 5, stopping at 8:");
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        continue; // Skip this iteration
    }
    if (i === 8) {
        break; // Exit the loop
    }
    console.log(i);
}

// Performance comparison example
let numbers = Array.from({length: 1000000}, (_, i) => i);

// Classic for loop (fastest)
console.time("for loop");
for (let i = 0; i < numbers.length; i++) {
    // Process numbers[i]
}
console.timeEnd("for loop");

// for...of loop (readable)
console.time("for...of loop");
for (let number of numbers) {
    // Process number
}
console.timeEnd("for...of loop");

// forEach method (functional)
console.time("forEach");
numbers.forEach(number => {
    // Process number
});
console.timeEnd("forEach");
```

## while and do-while Loops
The `while` and `do-while` loops repeat code as long as a condition remains true.

**Q&A Format:**

**Q: What's the difference between while and do-while loops?**
A: Key differences:
- **while**: Tests condition before executing (may never execute)
- **do-while**: Tests condition after executing (always executes at least once)
- **Use while**: When you might not need to execute the loop at all
- **Use do-while**: When you need to execute the loop body at least once

**Q: When should you use while loops instead of for loops?**
A: Use while loops when:
- **Unknown iterations**: Don't know how many times to loop in advance
- **Condition-dependent**: Loop depends on changing conditions, not counters
- **User input**: Waiting for specific user input or external events
- **Complex conditions**: Multiple variables affect the loop condition

```javascript
// Basic while loop
let count = 0;

console.log("Counting with while loop:");
while (count < 5) {
    console.log(count);
    count++; // Don't forget to update the condition!
}

// while loop with array processing
let numbers = [1, 2, 3, 4, 5];
let index = 0;

console.log("Processing array with while:");
while (index < numbers.length) {
    console.log(`Number at index ${index}: ${numbers[index]}`);
    index++;
}

// User input simulation (in real app, use prompt() or similar)
let userInput = "";
let attempts = 0;
let maxAttempts = 3;

console.log("Simulating user input validation:");
while (userInput !== "quit" && attempts < maxAttempts) {
    attempts++;
    // Simulate user input
    userInput = attempts === 1 ? "hello" : attempts === 2 ? "world" : "quit";
    console.log(`Attempt ${attempts}: User entered "${userInput}"`);
    
    if (userInput === "quit") {
        console.log("User chose to quit.");
    } else if (attempts >= maxAttempts) {
        console.log("Maximum attempts reached.");
    }
}

// do-while loop (executes at least once)
let password;
let isValidPassword = false;

console.log("Password validation with do-while:");
do {
    // Simulate password input
    password = "weak"; // In real app, get from user input
    console.log(`Checking password: "${password}"`);
    
    if (password.length >= 8) {
        isValidPassword = true;
        console.log("Password accepted!");
    } else {
        console.log("Password too short. Must be at least 8 characters.");
        password = "strongpassword"; // Simulate user entering correct password
    }
} while (!isValidPassword);

// Practical example: Reading data until end
let data = [1, 2, 3, 4, 5, -1]; // -1 is sentinel value
let dataIndex = 0;
let sum = 0;

console.log("Reading data until sentinel value:");
while (dataIndex < data.length && data[dataIndex] !== -1) {
    sum += data[dataIndex];
    console.log(`Added ${data[dataIndex]}, running sum: ${sum}`);
    dataIndex++;
}
console.log(`Final sum: ${sum}`);

// Infinite loop prevention
let counter = 0;
let maxIterations = 1000;

console.log("Safe loop with iteration limit:");
while (counter < maxIterations) {
    // Simulate some condition that might not be met
    if (Math.random() > 0.995) { // Very rare condition
        console.log("Condition met, breaking loop");
        break;
    }
    counter++;
}

if (counter >= maxIterations) {
    console.log("Loop terminated due to iteration limit");
} else {
    console.log(`Loop completed in ${counter} iterations`);
}

// Common while loop patterns

// 1. Processing user input
function simulateUserMenu() {
    let choice;
    
    do {
        console.log("\n--- Menu ---");
        console.log("1. Option A");
        console.log("2. Option B");
        console.log("3. Exit");
        
        // Simulate user choice
        choice = Math.floor(Math.random() * 4) + 1;
        console.log(`User selected: ${choice}`);
        
        switch (choice) {
            case 1:
                console.log("Executing Option A");
                break;
            case 2:
                console.log("Executing Option B");
                break;
            case 3:
                console.log("Goodbye!");
                break;
            default:
                console.log("Invalid choice, try again");
        }
    } while (choice !== 3);
}

// 2. Waiting for a condition
function waitForConnection() {
    let isConnected = false;
    let retries = 0;
    
    while (!isConnected && retries < 5) {
        console.log(`Attempting to connect... (attempt ${retries + 1})`);
        
        // Simulate connection attempt
        isConnected = Math.random() > 0.7; // 30% success rate
        
        if (!isConnected) {
            retries++;
            console.log("Connection failed, retrying...");
        } else {
            console.log("Connected successfully!");
        }
    }
    
    if (!isConnected) {
        console.log("Failed to connect after maximum retries");
    }
}

// 3. Processing variable-length data
function processVariableLengthData() {
    let data = [1, 2, 3, 4, 5, 0]; // 0 terminates
    let i = 0;
    let result = [];
    
    while (data[i] !== 0) {
        result.push(data[i] * 2);
        i++;
    }
    
    console.log("Processed data:", result);
}

// Run examples
simulateUserMenu();
waitForConnection();
processVariableLengthData();
```

## break and continue Statements
The `break` and `continue` statements provide flow control within loops.

**Q&A Format:**

**Q: What do break and continue statements do in loops?**
A: Flow control statements:
- **break**: Immediately exits the current loop completely
- **continue**: Skips the rest of the current iteration and moves to the next
- **Scope**: Only affect the innermost loop they're in
- **Usage**: Useful for handling special cases or optimizing performance

**Q: Can you use break and continue with labels, and when is this useful?**
A: Yes, JavaScript supports labeled statements:
- **Syntax**: `labelName: for(...)` then `break labelName;`
- **Use case**: Breaking out of nested loops to outer loop
- **Best practice**: Use sparingly, often better code structure can avoid labels
- **Alternative**: Extract nested loops into functions with return statements

```javascript
// break statement examples
console.log("=== break Examples ===");

// Basic break - exit loop early
console.log("Finding first even number:");
for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        console.log(`Found first even number: ${i}`);
        break; // Exit the loop
    }
    console.log(`${i} is odd, continuing...`);
}

// break in while loop
console.log("\nBreaking from while loop:");
let num = 1;
while (true) { // Infinite loop
    console.log(`Processing number: ${num}`);
    if (num >= 5) {
        console.log("Reached limit, breaking");
        break;
    }
    num++;
}

// continue statement examples
console.log("\n=== continue Examples ===");

// Basic continue - skip current iteration
console.log("Printing odd numbers only:");
for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        continue; // Skip even numbers
    }
    console.log(`Odd number: ${i}`);
}

// continue with more complex logic
console.log("\nProcessing valid items only:");
let items = ["apple", "", "banana", null, "cherry", undefined, "date"];

for (let i = 0; i < items.length; i++) {
    let item = items[i];
    
    // Skip invalid items
    if (!item || item.trim() === "") {
        console.log(`Skipping invalid item at index ${i}`);
        continue;
    }
    
    console.log(`Processing: ${item}`);
}

// Nested loops with break and continue
console.log("\n=== Nested Loops ===");

console.log("Break affects only inner loop:");
for (let i = 1; i <= 3; i++) {
    console.log(`Outer loop: ${i}`);
    
    for (let j = 1; j <= 5; j++) {
        if (j === 3) {
            console.log("  Breaking inner loop at j=3");
            break; // Only breaks inner loop
        }
        console.log(`  Inner loop: ${j}`);
    }
}

console.log("\nContinue affects only inner loop:");
for (let i = 1; i <= 3; i++) {
    console.log(`Outer loop: ${i}`);
    
    for (let j = 1; j <= 5; j++) {
        if (j === 3) {
            console.log("  Skipping inner loop at j=3");
            continue; // Only continues inner loop
        }
        console.log(`  Inner loop: ${j}`);
    }
}

// Labeled statements for complex control flow
console.log("\n=== Labeled Statements ===");

console.log("Breaking outer loop with label:");
outerLoop: for (let i = 1; i <= 3; i++) {
    console.log(`Outer loop: ${i}`);
    
    for (let j = 1; j <= 3; j++) {
        if (i === 2 && j === 2) {
            console.log("  Breaking outer loop from inner loop");
            break outerLoop; // Breaks the labeled outer loop
        }
        console.log(`  Inner loop: ${j}`);
    }
}

console.log("\nContinuing outer loop with label:");
outerContinue: for (let i = 1; i <= 3; i++) {
    console.log(`Outer loop start: ${i}`);
    
    for (let j = 1; j <= 3; j++) {
        if (i === 2 && j === 2) {
            console.log("  Continuing outer loop from inner loop");
            continue outerContinue; // Continues the labeled outer loop
        }
        console.log(`  Inner loop: ${j}`);
    }
    
    console.log(`Outer loop end: ${i}`);
}

// Practical examples
console.log("\n=== Practical Examples ===");

// Example 1: Search and process
function findAndProcessUser(users, targetId) {
    console.log(`Searching for user with ID: ${targetId}`);
    
    for (let user of users) {
        // Skip invalid users
        if (!user || !user.id) {
            console.log("Skipping invalid user");
            continue;
        }
        
        console.log(`Checking user: ${user.name} (ID: ${user.id})`);
        
        if (user.id === targetId) {
            console.log(`Found user: ${user.name}`);
            console.log("Processing user data...");
            break; // Stop searching once found
        }
    }
}

let users = [
    { id: 1, name: "Alice" },
    null, // Invalid user
    { id: 2, name: "Bob" },
    { id: 3, name: "Charlie" }
];

findAndProcessUser(users, 2);

// Example 2: Data validation with continue
function validateAndProcessData(data) {
    let validCount = 0;
    let invalidCount = 0;
    
    for (let item of data) {
        // Validation checks
        if (typeof item !== 'number') {
            console.log(`Skipping non-number: ${item}`);
            invalidCount++;
            continue;
        }
        
        if (item < 0) {
            console.log(`Skipping negative number: ${item}`);
            invalidCount++;
            continue;
        }
        
        if (item > 100) {
            console.log(`Skipping number too large: ${item}`);
            invalidCount++;
            continue;
        }
        
        // Process valid data
        console.log(`Processing valid number: ${item}`);
        validCount++;
    }
    
    console.log(`Processed ${validCount} valid items, skipped ${invalidCount} invalid items`);
}

let testData = [10, -5, "hello", 25, 150, 75, null, 90];
validateAndProcessData(testData);

// Example 3: Game loop with break conditions
function simulateGameLoop() {
    let health = 100;
    let level = 1;
    let experience = 0;
    
    gameLoop: while (true) {
        console.log(`\n--- Level ${level}, Health: ${health}, XP: ${experience} ---`);
        
        // Simulate random events
        let event = Math.floor(Math.random() * 4);
        
        switch (event) {
            case 0: // Gain experience
                experience += 10;
                console.log("Gained 10 experience!");
                
                if (experience >= 50) {
                    level++;
                    experience = 0;
                    health = 100; // Full health on level up
                    console.log(`Level up! Now level ${level}`);
                }
                break;
                
            case 1: // Take damage
                health -= 15;
                console.log("Took 15 damage!");
                
                if (health <= 0) {
                    console.log("Game Over!");
                    break gameLoop; // End game
                }
                break;
                
            case 2: // Find health potion
                if (health < 100) {
                    health = Math.min(100, health + 20);
                    console.log("Found health potion! +20 health");
                } else {
                    console.log("Already at full health, potion wasted");
                }
                break;
                
            case 3: // Nothing happens
                console.log("Nothing happens this turn");
                break;
        }
        
        // End game if reached max level
        if (level >= 5) {
            console.log("Congratulations! You've reached max level!");
            break gameLoop;
        }
        
        // Simulate turn delay
        console.log("Press any key for next turn...");
    }
    
    console.log("Game ended");
}

// Uncomment to run game simulation
// simulateGameLoop();
```