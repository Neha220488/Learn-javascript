# Asynchronous JavaScript

# Asynchronous JavaScript

## Introduction to Asynchronous JavaScript

**Q: Why is asynchronous programming important in JavaScript?**

A: JavaScript is **single-threaded** but needs to handle **time-consuming operations** without blocking the UI:

```javascript
// ❌ Synchronous (blocking) - freezes the browser
console.log("Start");
for (let i = 0; i < 1000000000; i++) {
    // This loop blocks everything for seconds!
}
console.log("Finally done");  // Takes forever to reach here

// ✅ Asynchronous (non-blocking) - keeps browser responsive
console.log("Start");
setTimeout(() => {
    console.log("This runs later");
}, 2000);
console.log("This runs immediately");  // Doesn't wait for timeout

// Output order:
// "Start"
// "This runs immediately"  
// "This runs later" (after 2 seconds)
```

**Q: What are the key components of JavaScript's asynchronous execution model?**

A: The **Event Loop System** with these parts:

1. 🥞 **Call Stack**: Tracks function execution (LIFO - Last In, First Out)
2. 🌐 **Web APIs**: Handle async operations (timers, HTTP requests, DOM events)
3. 📋 **Callback Queue**: Holds completed async callbacks (FIFO - First In, First Out)
4. 🔄 **Event Loop**: Moves callbacks from queue to stack when stack is empty

```javascript
console.log("1");  // → Call Stack → Execute → Pop

setTimeout(() => {
    console.log("2");  // → Web API → Callback Queue → Call Stack (when empty)
}, 0);

console.log("3");  // → Call Stack → Execute → Pop

// Output: "1", "3", "2" (not "1", "2", "3"!)
```

**💡 Memory Tip:** "CWCE" - **C**all Stack, **W**eb APIs, **C**allback Queue, **E**vent Loop
┌─────────────────┐     ┌───────────────┐     ┌─────────────┐
│    Call Stack   │     │    Web APIs   │     │   Callback  │
│                 │     │               │     │    Queue    │
└─────────────────┘     └───────────────┘     └─────────────┘
        │                      │                     │
        │                      │                     │
        │                      │                     │
        │                      ▼                     │
        │                Asynchronous               │
        │                 Operations                │
        │                      │                     │
        │                      │                     │
        │                      ▼                     │
        │                  Callbacks                 │
        │                      │                     │
        │                      │                     │
        │                      ▼                     │
        ◄──────────────Event Loop──────────────────►
*/
```

## Callbacks

**Q: What is a callback function and why do we use them?**

A: A **function passed as an argument** to another function, executed after an operation completes:

```javascript
// ✅ Basic callback pattern
function greet(name, callback) {
    console.log("Hello, " + name + "!");
    callback();  // Execute the callback
}

greet("John", function() {
    console.log("Greeting completed!");
});

// Output:
// "Hello, John!"
// "Greeting completed!"
```

**Q: What is "callback hell" and how do you recognize it?**

A: **Deeply nested callbacks** that create a "pyramid of doom" - hard to read and maintain:

```javascript
// ❌ Callback hell (pyramid shape)
getData(function(a) {
    getMoreData(a, function(b) {
        getEvenMoreData(b, function(c) {
            getFinalData(c, function(d) {
                // Finally can use d
                console.log(d);
            }, function(error) {
                console.error(error);
            });
        }, function(error) {
            console.error(error);
        });
    }, function(error) {
        console.error(error);
    });
}, function(error) {
    console.error(error);
});

// ✅ Better approach: named functions
function handleA(a) {
    getMoreData(a, handleB, handleError);
}

function handleB(b) {
    getEvenMoreData(b, handleC, handleError);
}

function handleC(c) {
    getFinalData(c, handleD, handleError);
}

function handleD(d) {
    console.log(d);
}

function handleError(error) {
    console.error("Error:", error);
}

getData(handleA, handleError);
```

**Q: How do you handle errors with callbacks?**

A: Use **error-first callbacks** - first parameter is error, second is result:

```javascript
function divideNumbers(a, b, callback) {
    if (b === 0) {
        callback(new Error("Cannot divide by zero"), null);
    } else {
        callback(null, a / b);  // null for error, result as second param
    }
}

divideNumbers(10, 2, function(error, result) {
    if (error) {
        console.error("Error:", error.message);
    } else {
        console.log("Result:", result);  // 5
    }
});
```

**💡 Memory Tip:** "Callback Hell = Code that looks like a pyramid ⛰️"
```

## Promises

**Q: What is a Promise and what are its three states?**

A: A **Promise** represents the eventual result of an async operation. Three states:

1. ⏳ **Pending**: Operation in progress
2. ✅ **Fulfilled**: Operation succeeded (resolved)
3. ❌ **Rejected**: Operation failed

```javascript
// Creating a Promise
const myPromise = new Promise((resolve, reject) => {
    const success = Math.random() > 0.5;
    
    setTimeout(() => {
        if (success) {
            resolve("Operation succeeded!"); // → Fulfilled
        } else {
            reject("Operation failed!");     // → Rejected
        }
    }, 1000);
});

// Using the Promise
myPromise
    .then(result => {
        console.log("✅ Success:", result);
    })
    .catch(error => {
        console.log("❌ Error:", error);
    });
```

**Q: How do you chain multiple asynchronous operations with Promises?**

A: Use **.then()** for chaining - each returns a new Promise:

```javascript
// ✅ Promise chaining (no callback hell!)
fetch('/api/user')
    .then(response => response.json())       // Parse JSON
    .then(user => fetch(`/api/posts/${user.id}`))  // Get user's posts
    .then(response => response.json())       // Parse posts JSON
    .then(posts => {
        console.log("User posts:", posts);
    })
    .catch(error => {
        console.error("Any step failed:", error);  // Catches ALL errors
    });

// Compare to callback hell:
// fetchUser(function(user) {
//     fetchPosts(user.id, function(posts) {
//         console.log(posts);
//     }, errorHandler);
// }, errorHandler);
```

**Q: What's the difference between .then() and .catch()?**

A: 
- **.then()** handles **successful** results (fulfilled promises)
- **.catch()** handles **errors** (rejected promises)

```javascript
somePromise
    .then(result => {
        console.log("Success:", result);  // Only runs if fulfilled
        return result * 2;               // Can transform and pass along
    })
    .catch(error => {
        console.error("Error:", error);   // Only runs if rejected
        return "default value";          // Can provide fallback
    })
    .then(finalResult => {
        console.log("Final:", finalResult);  // Runs after either success or catch
    });
```

**💡 Memory Tip:** "Promise = I promise to give you a result... eventually!" 🤝
    .catch(error => {
        console.error("Error:", error);
    });

// Promise with setTimeout
function delay(ms) {
    return new Promise(resolve => {
        setTimeout(resolve, ms);
    });
}

console.log("Starting...");

delay(2000)
    .then(() => {
        console.log("Two seconds have passed");
        return "Done waiting";
    })
    .then(message => {
        console.log(message);  // "Done waiting"
    });

console.log("Continuing...");

// Output:
// "Starting..."
// "Continuing..."
// "Two seconds have passed"
// "Done waiting"

// Converting callbacks to Promises
function fetchData(url) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open("GET", url);
        
        xhr.onload = function() {
            if (xhr.status === 200) {
                resolve(xhr.responseText);
            } else {
                reject("Request failed with status: " + xhr.status);
            }
        };
        
        xhr.onerror = function() {
            reject("Network error");
        };
        
        xhr.send();
    });
}

// Using the Promise-based function
fetchData("https://api.example.com/data")
    .then(data => {
        console.log("Success:", data);
        return fetchData("https://api.example.com/more-data");
    })
    .then(moreData => {
        console.log("More data:", moreData);
    })
    .catch(error => {
        console.error("Error:", error);
    });

// Promise chaining (avoiding callback hell)
fetchData("https://api.example.com/users")
    .then(users => {
        console.log("Got users");
        return fetchData("https://api.example.com/products");
    })
    .then(products => {
        console.log("Got products");
        return fetchData("https://api.example.com/orders");
    })
    .then(orders => {
        console.log("Got orders");
        return fetchData("https://api.example.com/reviews");
    })
    .then(reviews => {
        console.log("Got reviews");
    })
    .catch(error => {
        console.error("Error:", error);
## Promise Methods

**Q: What does Promise.all() do and when do you use it?**

A: Waits for **ALL promises to succeed** or fails if **ANY fails**. Use for parallel operations where you need all results:

```javascript
const userPromise = fetch('/api/user');
const postsPromise = fetch('/api/posts');  
const settingsPromise = fetch('/api/settings');

// ✅ All must succeed
Promise.all([userPromise, postsPromise, settingsPromise])
    .then(([userResponse, postsResponse, settingsResponse]) => {
        // All three requests completed successfully
        console.log("All data loaded!");
    })
    .catch(error => {
        // If ANY request fails, this catch runs
        console.error("Something failed:", error);
    });
```

**Q: What's the difference between Promise.all() and Promise.race()?**

A: 
- **Promise.all()**: Waits for **all** to complete ⏳
- **Promise.race()**: Returns when **first** completes 🏁

```javascript
const fast = new Promise(resolve => setTimeout(() => resolve("Fast"), 1000));
const slow = new Promise(resolve => setTimeout(() => resolve("Slow"), 3000));

// Promise.all - waits for both (3 seconds)
Promise.all([fast, slow]).then(([fastResult, slowResult]) => {
    console.log(fastResult, slowResult);  // "Fast" "Slow" (after 3 seconds)
});

// Promise.race - returns first (1 second)
Promise.race([fast, slow]).then(result => {
    console.log(result);  // "Fast" (after 1 second)
});
```

**Q: When do you use Promise.allSettled() vs Promise.all()?**

A: 
- **Promise.all()**: Fails fast - if one fails, all fail ❌
- **Promise.allSettled()**: Never fails - gives you all results ✅

```javascript
const success = Promise.resolve("Success");
const failure = Promise.reject("Failed");

// Promise.all - stops at first failure
Promise.all([success, failure])
    .catch(error => console.log("All failed:", error));  // "All failed: Failed"

// Promise.allSettled - shows all results
Promise.allSettled([success, failure])
    .then(results => {
        console.log(results);
        // [
        //   { status: "fulfilled", value: "Success" },
        //   { status: "rejected", reason: "Failed" }
        // ]
    });
```

**💡 Memory Tip:** 
- **All** = **A**ll must succeed 💯
- **Race** = **R**ace to the finish line 🏃‍♂️
- **AllSettled** = **S**ettled means done (success or failure) ✔️
```

## Async/Await

**Q: What are async and await keywords and what do they make easier?**

A: **async/await** makes Promise code look like **synchronous code**:

- **async**: Declares function returns a Promise automatically 
- **await**: Pauses function until Promise resolves

```javascript
// ❌ Promise chains (harder to read)
function fetchUserPosts() {
    return fetch('/api/user')
        .then(response => response.json())
        .then(user => fetch(`/api/posts/${user.id}`))
        .then(response => response.json())
        .then(posts => {
            console.log("Posts:", posts);
            return posts;
        });
}

// ✅ Async/await (easier to read)
async function fetchUserPosts() {
    const userResponse = await fetch('/api/user');
    const user = await userResponse.json();
    const postsResponse = await fetch(`/api/posts/${user.id}`);
    const posts = await postsResponse.json();
    console.log("Posts:", posts);
    return posts;  // Automatically wrapped in Promise
}
```

**Q: How do you handle errors with async/await?**

A: Use **try/catch blocks** instead of .catch():

```javascript
async function fetchData() {
    try {
        const response = await fetch('/api/data');
        
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const data = await response.json();
        console.log("Success:", data);
        return data;
        
    } catch (error) {
        console.error("Error occurred:", error.message);
        throw error;  // Re-throw if you want caller to handle it
    }
}

// Using the async function
fetchData()
    .then(data => console.log("Got data:", data))
    .catch(error => console.log("Failed:", error));
```

**Q: Can you use await with Promise.all() for parallel operations?**

A: **Yes!** await works with any Promise, including Promise.all():

```javascript
async function loadAllData() {
    try {
        // ✅ Parallel execution (faster)
        const [users, posts, comments] = await Promise.all([
            fetch('/api/users').then(r => r.json()),
            fetch('/api/posts').then(r => r.json()),
            fetch('/api/comments').then(r => r.json())
        ]);
        
        console.log("All data loaded!", { users, posts, comments });
        
    } catch (error) {
        console.error("One or more requests failed:", error);
    }
}

// ❌ Sequential execution (slower)
async function loadAllDataSequentially() {
    const users = await fetch('/api/users').then(r => r.json());
    const posts = await fetch('/api/posts').then(r => r.json());      // Waits for users
    const comments = await fetch('/api/comments').then(r => r.json()); // Waits for posts
}
```

**Q: What happens if you forget the await keyword?**

A: You get a **Promise object** instead of the resolved value:

```javascript
async function demonstrateAwait() {
    // ❌ Without await - gets Promise object
    const promise = fetch('/api/data');
    console.log(promise);  // Promise { <pending> }
    
    // ✅ With await - gets actual data
    const response = await fetch('/api/data');
    console.log(response);  // Response object
}
```

## The Fetch API

**Q: What is the Fetch API and how is it different from XMLHttpRequest?**

A: **Modern Promise-based API** for HTTP requests. Much cleaner than old XMLHttpRequest:

```javascript
// ❌ Old XMLHttpRequest way (verbose)
const xhr = new XMLHttpRequest();
xhr.open("GET", "/api/data");
xhr.onload = function() {
    if (xhr.status === 200) {
        const data = JSON.parse(xhr.responseText);
        console.log(data);
    }
};
xhr.send();

// ✅ Modern Fetch API (clean)
fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data));

// ✅ Even better with async/await
async function getData() {
    const response = await fetch('/api/data');
    const data = await response.json();
    console.log(data);
}
```

**Q: How do you make different types of HTTP requests with fetch?**

A: Use the **options object** to specify method, headers, and body:

```javascript
// GET request (default)
const response = await fetch('/api/users');

// POST request
const newUser = await fetch('/api/users', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer token123'
    },
    body: JSON.stringify({
        name: 'John Doe',
        email: 'john@example.com'
    })
});

// PUT request
const updatedUser = await fetch(`/api/users/${userId}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: 'Jane Doe' })
});

// DELETE request
const deleted = await fetch(`/api/users/${userId}`, {
    method: 'DELETE'
});
```

**Q: Why should you always check response.ok with fetch?**

A: **Fetch doesn't reject on HTTP error status** (404, 500, etc.) - only on network errors:

```javascript
// ❌ This doesn't catch 404 or 500 errors!
try {
    const response = await fetch('/api/data');
    const data = await response.json();  // Will fail on 404!
} catch (error) {
    console.log("This only catches network errors");
}

// ✅ Always check response.ok
async function fetchData() {
    try {
        const response = await fetch('/api/data');
        
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
        
    } catch (error) {
        console.error("Error:", error.message);
    }
}
```

**Q: How do you handle different response types with fetch?**

A: Use different methods based on expected content type:

```javascript
async function handleDifferentTypes() {
    const response = await fetch('/api/data');
    
    if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
    }
    
    // For JSON data
    const jsonData = await response.json();
    
    // For plain text
    const textData = await response.text();
    
    // For binary data (images, files)
    const blobData = await response.blob();
    
    // For streams
    const streamData = await response.arrayBuffer();
    
    // For form data
    const formData = await response.formData();
}
```

**💡 Memory Tip:** "Fetch = Fetch me some data over the network!" 🌐

// Fetch with timeout
function fetchWithTimeout(url, options = {}, timeout = 5000) {
    return Promise.race([
        fetch(url, options),
        new Promise((_, reject) => 
            setTimeout(() => reject(new Error("Request timed out")), timeout)
        )
    ]);
}

// Using fetchWithTimeout
fetchWithTimeout("https://api.example.com/data", {}, 3000)
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error("Error:", error));

// Handling response headers
async function checkContentType() {
    const response = await fetch("https://api.example.com/data");
    
    // Get a specific header
    const contentType = response.headers.get("Content-Type");
    console.log("Content type:", contentType);
    
    // Iterate through all headers
    for (const [key, value] of response.headers.entries()) {
        console.log(`${key}: ${value}`);
    }
    
    return response.json();
}

// Aborting fetch requests with AbortController
async function abortableFetch() {
    const controller = new AbortController();
    const { signal } = controller;
    
    // Abort the fetch after 3 seconds
    setTimeout(() => {
        controller.abort();
        console.log("Fetch aborted");
    }, 3000);
    
    try {
        const response = await fetch("https://api.example.com/data-slow", { signal });
        const data = await response.json();
        console.log("Data:", data);
    } catch (error) {
        if (error.name === "AbortError") {
            console.log("Fetch was aborted");
        } else {
            console.error("Fetch error:", error);
        }
    }
}

// Fetch with credentials (cookies)
fetch("https://api.example.com/user-data", {
    credentials: "include"  // Send cookies, even for cross-origin requests
})
    .then(response => response.json())
    .then(data => console.log(data));

// Upload a file with fetch
async function uploadFile(file) {
    const formData = new FormData();
    formData.append("file", file);
    formData.append("filename", file.name);
    
    try {
        const response = await fetch("https://api.example.com/upload", {
            method: "POST",
            body: formData
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        return response.json();
    } catch (error) {
        console.error("Error uploading file:", error);
    }
}

// Checking fetch progress (for download)
async function downloadWithProgress(url) {
    const response = await fetch(url);
    
    if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
    }
    
    // Get the total size
    const contentLength = response.headers.get("Content-Length");
    const total = parseInt(contentLength, 10);
    
    // Create a reader to read the response body
    const reader = response.body.getReader();
    
    // Read the data
    let receivedLength = 0;
    let chunks = [];
    
    while (true) {
        const { done, value } = await reader.read();
        
        if (done) {
            break;
        }
        
        chunks.push(value);
        receivedLength += value.length;
        
        console.log(`Received ${receivedLength} of ${total} bytes`);
        const progress = Math.round((receivedLength / total) * 100);
        console.log(`Progress: ${progress}%`);
    }
    
    // Combine chunks into a single Uint8Array
    const chunksAll = new Uint8Array(receivedLength);
    let position = 0;
    for (const chunk of chunks) {
        chunksAll.set(chunk, position);
        position += chunk.length;
    }
    
    return chunksAll;
}
```

## Event Loop and Asynchronous Execution

**Q: What is the Event Loop and how does it enable JavaScript's concurrency?**

A: The **Event Loop** is JavaScript's mechanism for handling async operations despite being **single-threaded**:

Components working together:
1. 🥞 **Call Stack**: Current executing functions (LIFO)
2. 🌐 **Web APIs**: Handle async tasks (setTimeout, fetch, DOM events)
3. 📋 **Callback Queue** (Macrotasks): Completed callbacks (FIFO)
4. ⚡ **Microtask Queue**: Promises, queueMicrotask (higher priority)
5. 🔄 **Event Loop**: Moves tasks from queues to call stack

**Q: What's the difference between Macrotasks and Microtasks?**

A: **Microtasks always execute before Macrotasks** in each event loop cycle:

| Type | Examples | Priority | When executed |
|------|----------|----------|---------------|
| **Microtasks** | Promises, queueMicrotask() | ⚡ Higher | After each call stack is empty |
| **Macrotasks** | setTimeout, setInterval, I/O | 📋 Lower | After ALL microtasks are done |

```javascript
// Classic example showing execution order
console.log("Script start");  // 1. Synchronous

setTimeout(() => {
    console.log("setTimeout");  // 5. Macrotask
}, 0);

Promise.resolve().then(() => {
    console.log("Promise 1");  // 3. Microtask
}).then(() => {
    console.log("Promise 2");  // 4. Microtask
});

queueMicrotask(() => {
    console.log("queueMicrotask");  // 3. Microtask (alongside Promise)
});

console.log("Script end");  // 2. Synchronous

// Output order:
// "Script start"
// "Script end" 
// "Promise 1"
// "queueMicrotask"
// "Promise 2"
// "setTimeout"
```

**Q: How can understanding the Event Loop help with performance?**

A: Break up **blocking operations** to keep the UI responsive:

```javascript
// ❌ Blocks the main thread (freezes UI)
function blockingOperation() {
    const element = document.getElementById("output");
    element.textContent = "Processing...";
    
    // This will freeze the browser!
    for (let i = 0; i < 100000000; i++) {
        // Heavy computation
    }
    
    element.textContent = "Done!";  // UI won't update until loop finishes
}

// ✅ Non-blocking (keeps UI responsive)
function nonBlockingOperation() {
    const element = document.getElementById("output");
    element.textContent = "Processing...";
    
    // Break work into chunks
    let i = 0;
    function processChunk() {
        const chunkSize = 1000000;  // Smaller chunks
        const end = Math.min(i + chunkSize, 100000000);
        
        while (i < end) {
            // Process chunk
            i++;
        }
        
        if (i < 100000000) {
            setTimeout(processChunk, 0);  // Schedule next chunk
        } else {
            element.textContent = "Done!";
        }
    }
    
    processChunk();  // Start processing
}

// ✅ Even better with requestAnimationFrame for smooth animations
function smoothAnimation() {
    const element = document.getElementById("box");
    let position = 0;
    
    function animate() {
        position += 2;
        element.style.transform = `translateX(${position}px)`;
        
        if (position < 400) {
            requestAnimationFrame(animate);  // 60fps when possible
        }
    }
    
    requestAnimationFrame(animate);
}
```

**💡 Memory Tip:** "Microtasks = **M**ore important than **M**acrotasks!" ⚡

## Web Workers

**Q: What are Web Workers and when should you use them?**

A: **Background threads** for CPU-intensive tasks that don't block the main UI thread:

**When to use Web Workers:**
- ⚙️ Heavy calculations (image processing, data analysis)
- 📊 Processing large datasets
- 🔄 Long-running operations
- 🎮 Game logic and physics simulations

```javascript
// ❌ Without Web Worker (blocks UI)
function heavyCalculation() {
    console.log("Starting calculation... UI freezes!");
    
    let result = 0;
    for (let i = 0; i < 1000000000; i++) {
        result += Math.random();  // Browser becomes unresponsive
    }
    
    console.log("Done:", result);
}

// ✅ With Web Worker (UI stays responsive)
// main.js
const worker = new Worker('calculation-worker.js');

worker.postMessage({ command: 'calculate', numbers: [1, 2, 3, 4, 5] });

worker.onmessage = function(event) {
    console.log("Result from worker:", event.data.result);
    console.log("UI stayed responsive! 🎉");
};

worker.onerror = function(error) {
    console.error("Worker error:", error);
};
```

**Q: How do you create and communicate with a Web Worker?**

A: Use **postMessage()** to send data and **onmessage** to receive:

```javascript
// worker.js (separate file)
self.onmessage = function(event) {
    const { command, numbers } = event.data;
    
    if (command === 'calculate') {
        console.log("Worker: Starting heavy calculation");
        
        let sum = 0;
        // Simulate heavy work
        for (let i = 0; i < 100000000; i++) {
            if (i % 10000000 === 0) {
                sum += numbers[i % numbers.length];
            }
        }
        
        // Send result back to main thread
        self.postMessage({ 
            result: sum,
            message: "Calculation complete!"
        });
    }
};

// Error handling in worker
self.onerror = function(error) {
    console.error("Worker internal error:", error);
};
```

**Q: What types of Web Workers exist and what are their differences?**

A: Three main types with different capabilities:

| Type | Scope | Sharing | Use Case |
|------|-------|---------|----------|
| **Dedicated Worker** | Single page | ❌ No | Heavy calculations |
| **Shared Worker** | Multiple pages/tabs | ✅ Yes | Cross-tab communication |
| **Service Worker** | Entire site | ✅ Yes | Offline functionality, caching |

```javascript
// Dedicated Worker (most common)
const dedicatedWorker = new Worker('dedicated-worker.js');

// Shared Worker (multiple tabs can access)
const sharedWorker = new SharedWorker('shared-worker.js');
sharedWorker.port.start();
sharedWorker.port.postMessage('Hello from tab!');

// Service Worker (for PWAs and caching)
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/service-worker.js');
}
```

**Q: How do you handle errors and terminate Web Workers properly?**

A: Always include error handling and cleanup:

```javascript
function createSafeWorker() {
    const worker = new Worker('worker.js');
    
    // Handle messages
    worker.onmessage = function(event) {
        console.log("Received:", event.data);
        
        // Terminate when done
        if (event.data.done) {
            worker.terminate();
            console.log("Worker terminated");
        }
    };
    
    // Handle errors
    worker.onerror = function(error) {
        console.error("Worker error:", error.message);
        worker.terminate();  // Clean up on error
    };
    
    // Send initial task
    worker.postMessage({ task: "process", data: [1, 2, 3] });
    
    // Optional: Set timeout for long-running workers
    setTimeout(() => {
        worker.terminate();
        console.log("Worker terminated due to timeout");
    }, 30000);  // 30 seconds max
    
    return worker;
}
```

**💡 Memory Tip:** "Web Workers = **W**ork **W**ithout blocking the **W**indow!" 🪟
