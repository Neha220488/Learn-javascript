# DOM (Document Object Model) in JavaScript

## DOM Fundamentals

**Q: What is the DOM and why is it important?**

**A:** The DOM (Document Object Model) is JavaScript's way to interact with web pages. Think of it as a bridge between your HTML and JavaScript code.

```javascript
// The DOM represents your HTML as a tree structure
/*
document
  ├── html
  │   ├── head
  │   │   ├── title
  │   │   └── meta
  │   └── body
  │       ├── div (class="container")
  │       │   ├── h1
  │       │   └── p
  │       └── footer
*/

// Basic DOM access
console.log(document);        // The entire HTML document
console.log(document.title);  // Page title
console.log(document.body);   // Body element
console.log(document.head);   // Head element

// DOM lets you change the page dynamically
document.title = "New Page Title";  // Changes browser tab title
document.body.style.backgroundColor = "lightblue";  // Changes page color
```

**Key Benefits:**
- ✅ Make pages interactive (respond to clicks, form input, etc.)
- ✅ Update content without page reload
- ✅ Create dynamic web applications
- ✅ Respond to user actions in real-time

**Memory Tip:** 🌳 "DOM is like a family tree - elements have parents, children, and siblings"

## Selecting Elements

**Q: What are the main ways to select elements from the DOM?**

**A:** JavaScript provides several methods to find elements, from specific to general:

```javascript
// HTML for examples:
/*
<div id="header">Header Content</div>
<p class="text">First paragraph</p>
<p class="text">Second paragraph</p>
<img src="photo.jpg" alt="Photo">
<button data-action="save">Save</button>
*/

// 1. getElementById - Find ONE element by ID (most specific)
const header = document.getElementById("header");
console.log(header);  // <div id="header">Header Content</div>

// 2. getElementsByClassName - Find MULTIPLE elements by class
const paragraphs = document.getElementsByClassName("text");
console.log(paragraphs);     // HTMLCollection [<p>, <p>]
console.log(paragraphs[0]);  // First paragraph
console.log(paragraphs[1]);  // Second paragraph

// 3. getElementsByTagName - Find MULTIPLE elements by tag
const allParagraphs = document.getElementsByTagName("p");
const allImages = document.getElementsByTagName("img");

// 4. querySelector - Find FIRST element matching CSS selector (modern & flexible)
const firstText = document.querySelector(".text");        // First element with class="text"
const headerById = document.querySelector("#header");     // Element with id="header"
const firstP = document.querySelector("p");              // First <p> element
const saveButton = document.querySelector("[data-action='save']");  // Button with data attribute

// 5. querySelectorAll - Find ALL elements matching CSS selector
const allTexts = document.querySelectorAll(".text");     // NodeList [<p>, <p>]
const allPs = document.querySelectorAll("p");           // All <p> elements

// Converting HTMLCollection/NodeList to Array (for array methods)
const paragraphArray = Array.from(paragraphs);
const textArray = [...allTexts];  // Using spread operator
```

**Which to use when:**
- 🎯 **getElementById:** When you know the specific ID
- 🔍 **querySelector:** Most flexible, use CSS selectors  
- 📋 **querySelectorAll:** When you need all matching elements
- ⚠️ **getElementsBy...:** Older methods, less flexible but still valid

```javascript
// Select an element by ID
const header = document.getElementById('header');
console.log(header);  // <div id="header">...</div>

// Select elements by class name (returns a live HTMLCollection)
const items = document.getElementsByClassName('item');
console.log(items);  // HTMLCollection [div.item, div.item, ...]
console.log(items.length);  // Number of elements with class "item"

// Select elements by tag name (returns a live HTMLCollection)
const paragraphs = document.getElementsByTagName('p');
console.log(paragraphs);  // HTMLCollection [p, p, ...]

// Select the first element that matches a CSS selector
const firstButton = document.querySelector('button');
console.log(firstButton);  // <button>...</button>

// Select the first element with a specific class
const firstItem = document.querySelector('.item');
console.log(firstItem);  // <div class="item">...</div>

// Select an element with a specific ID using querySelector
const header2 = document.querySelector('#header');
console.log(header2);  // <div id="header">...</div>

// Select all elements that match a CSS selector (returns a static NodeList)
const allButtons = document.querySelectorAll('button');
console.log(allButtons);  // NodeList [button, button, ...]

// Select elements with a specific attribute
const linksWithTarget = document.querySelectorAll('a[target="_blank"]');
console.log(linksWithTarget);  // NodeList of links with target="_blank"

// Iterating through NodeList
allButtons.forEach(button => {
    console.log(button.textContent);
});

// Iterating through HTMLCollection (needs conversion or traditional for loop)
for (let i = 0; i < items.length; i++) {
    console.log(items[i].textContent);
}

// Or convert HTMLCollection to array first
Array.from(items).forEach(item => {
    console.log(item.textContent);
});

// Selecting nested elements
const list = document.querySelector('ul');
const listItems = list.querySelectorAll('li');
console.log(listItems);  // NodeList of li elements within the selected ul
```

## Manipulating DOM Elements

**Q: How do you change the content of DOM elements?**

**A:** JavaScript provides multiple ways to modify element content, each suited for different scenarios:

```javascript
// Changing text content
const heading = document.getElementById('title');
heading.textContent = 'New Title';  // Updates just the text (safe from XSS)

// Changing HTML content
const container = document.querySelector('.container');
container.innerHTML = '<p>This is <strong>new</strong> content</p>';  // Parses and renders HTML

// Security consideration: innerHTML can be vulnerable to XSS
const userInput = '<script>alert("XSS")</script>';
container.innerHTML = userInput;  // ❌ Dangerous with user input
container.textContent = userInput;  // ✅ Safe - renders as text
```

**Q: How do you modify element attributes effectively?**

**A:** JavaScript provides several methods for working with HTML attributes:

```javascript
// Setting attributes
const link = document.querySelector('a');
link.setAttribute('href', 'https://example.com');
link.setAttribute('target', '_blank');
link.setAttribute('data-analytics', 'click-track');

// Getting attribute values
console.log(link.getAttribute('href'));  // "https://example.com"
console.log(link.getAttribute('target'));  // "_blank"

// Checking if attribute exists
console.log(link.hasAttribute('target'));  // true or false

// Removing attributes
link.removeAttribute('target');

// Shorthand properties for common attributes
const image = document.querySelector('img');
image.src = 'new-image.jpg';        // Same as setAttribute('src', 'new-image.jpg')
image.alt = 'Description';          // Same as setAttribute('alt', 'Description')
link.href = 'https://example.com';  // Same as setAttribute('href', 'https://example.com')

// Working with data attributes
const element = document.querySelector('.item');
element.dataset.userId = '123';      // Sets data-user-id="123"
element.dataset.status = 'active';   // Sets data-status="active"
console.log(element.dataset.userId); // "123"
```

**Q: What are the different ways to modify element styles?**

**A:** You can change styles using inline styles, CSS classes, or CSS custom properties:

```javascript
// 1. Inline styles (immediate, high specificity)
const paragraph = document.querySelector('p');
paragraph.style.color = 'blue';
paragraph.style.fontSize = '18px';
paragraph.style.fontWeight = 'bold';
paragraph.style.backgroundColor = '#f0f0f0';
paragraph.style.padding = '10px';
paragraph.style.borderRadius = '5px';

// Camel case for CSS properties with hyphens
paragraph.style.backgroundColor = '#f0f0f0';  // background-color
paragraph.style.borderRadius = '5px';         // border-radius
paragraph.style.fontSize = '18px';            // font-size

// Setting multiple styles at once (less preferred)
paragraph.style.cssText = 'color: blue; font-size: 18px; background-color: #f0f0f0;';

// 2. CSS Custom Properties (CSS Variables)
document.documentElement.style.setProperty('--main-color', '#007bff');
paragraph.style.setProperty('--text-size', '20px');

// 3. Reading computed styles
const computedStyles = window.getComputedStyle(paragraph);
console.log(computedStyles.color);      // Computed color value
console.log(computedStyles.fontSize);   // Computed font size
```

**Q: How do you work with CSS classes dynamically?**

**A:** The `classList` API provides powerful methods for managing element classes:

```javascript
// Working with classes using classList API
const element = document.querySelector('.item');

// Adding classes
element.classList.add('highlight');               // Add single class
element.classList.add('active', 'visible');      // Add multiple classes

// Removing classes
element.classList.remove('highlight');            // Remove single class
element.classList.remove('active', 'visible');   // Remove multiple classes

// Toggling classes (adds if absent, removes if present)
const isActive = element.classList.toggle('selected');  // Returns true if class was added
element.classList.toggle('hidden', false);              // Force remove
element.classList.toggle('visible', true);              // Force add

// Checking if an element has a class
if (element.classList.contains('active')) {
    console.log('Element is active');
}

// Replacing one class with another
element.classList.replace('active', 'inactive');

// Alternative: Setting the entire className (overwrites all existing classes)
element.className = 'new-class another-class';  // ❌ Less flexible, overwrites all

// Iterating through classes
element.classList.forEach(className => {
    console.log('Class:', className);
});

// Getting all classes as array
const classArray = Array.from(element.classList);
console.log('All classes:', classArray);
```

**Best Practices:**
- ✅ **Use classList** over className for better control
- ✅ **Prefer CSS classes** over inline styles for maintainability  
- ✅ **Use textContent** for user input to prevent XSS
- ✅ **Cache element references** for repeated operations

**Memory Tip:** 🎨 "**classList** is your **paintbrush** - add, remove, toggle classes like an artist!"
```

## Creating and Removing DOM Elements

**Q: How do you create new DOM elements from scratch?**

**A:** JavaScript provides methods to create elements, set their properties, and add them to the DOM:

```javascript
// Creating a new element
const newParagraph = document.createElement('p');
newParagraph.textContent = 'This is a new paragraph.';
newParagraph.className = 'info';
newParagraph.id = 'newInfo';

// Creating elements with attributes
const newImage = document.createElement('img');
newImage.src = 'photo.jpg';
newImage.alt = 'Beautiful photo';
newImage.setAttribute('data-id', '123');

// Creating a text node (rarely used directly)
const textNode = document.createTextNode('Just some text');

// Creating a more complex element with HTML
const newListItem = document.createElement('li');
newListItem.innerHTML = '<span class="label">Item:</span> <a href="#" class="link">Click here</a>';

// Creating elements with modern template literals
const productCard = document.createElement('div');
productCard.className = 'product-card';
productCard.innerHTML = `
    <img src="${product.image}" alt="${product.name}">
    <h3>${product.name}</h3>
    <p class="price">$${product.price}</p>
    <button class="add-to-cart" data-id="${product.id}">Add to Cart</button>
`;
```

**Q: What are the different ways to insert elements into the DOM?**

**A:** There are both traditional and modern methods for inserting elements at specific positions:

```javascript
const container = document.querySelector('.container');
const newElement = document.createElement('div');
newElement.textContent = 'New content';

// Traditional methods
container.appendChild(newElement);  // Adds at the end of container

// Insert before another element
const referenceElement = document.querySelector('.reference');
const parent = referenceElement.parentNode;
parent.insertBefore(newElement, referenceElement);

// Modern insertion methods (more intuitive)
const targetElement = document.querySelector('.target');

targetElement.prepend(newElement);    // Insert as the first child
targetElement.append(newElement);     // Insert as the last child (same as appendChild)
targetElement.before(newElement);     // Insert before the target element
targetElement.after(newElement);      // Insert after the target element

// Insert multiple elements at once
targetElement.append(element1, element2, 'Some text', element3);

// Insert HTML strings (modern browsers)
targetElement.insertAdjacentHTML('beforebegin', '<p>Before target</p>');
targetElement.insertAdjacentHTML('afterbegin', '<p>First child</p>');
targetElement.insertAdjacentHTML('beforeend', '<p>Last child</p>');
targetElement.insertAdjacentHTML('afterend', '<p>After target</p>');
```

**Q: How do you clone existing elements and handle element removal?**

**A:** Cloning is useful for templates, while removal cleans up the DOM:

```javascript
// Cloning an element
const original = document.querySelector('.template');
const clone = original.cloneNode(true);  // true = clone all descendants
clone.id = 'cloned-element';             // Give unique ID
clone.classList.add('cloned');
document.body.appendChild(clone);

// Shallow clone (element only, no children)
const shallowClone = original.cloneNode(false);

// Replacing elements
const oldElement = document.querySelector('.old');
const newElement = document.createElement('div');
newElement.textContent = 'Replacement content';

// Traditional way
oldElement.parentNode.replaceChild(newElement, oldElement);

// Modern way (more intuitive)
oldElement.replaceWith(newElement);

// Removing elements
const elementToRemove = document.querySelector('.remove-me');

// Traditional way
elementToRemove.parentNode.removeChild(elementToRemove);

// Modern way (simpler)
elementToRemove.remove();

// Removing all children
const parent = document.querySelector('.parent');

// Method 1: Loop removal
while (parent.firstChild) {
    parent.removeChild(parent.firstChild);
}

// Method 2: Clear innerHTML (fastest but loses event listeners)
parent.innerHTML = '';

// Method 3: Modern approach
parent.replaceChildren();  // Removes all children

// Remove specific child elements
parent.querySelectorAll('.child').forEach(child => child.remove());
```

**Q: What are best practices for dynamic DOM creation?**

**A:** Use document fragments for performance and maintain clean, accessible markup:

```javascript
// Use document fragments for batch operations (better performance)
const fragment = document.createDocumentFragment();
for (let i = 0; i < 1000; i++) {
    const li = document.createElement('li');
    li.textContent = `Item ${i}`;
    fragment.appendChild(li);  // No DOM reflow until final append
}
document.querySelector('ul').appendChild(fragment);

// Template element for reusable markup
const template = document.querySelector('#product-template');
const clone = template.content.cloneNode(true);
// Populate clone with data
clone.querySelector('.product-name').textContent = product.name;
clone.querySelector('.product-price').textContent = product.price;
document.querySelector('.products').appendChild(clone);

// Ensure accessibility when creating elements
const button = document.createElement('button');
button.textContent = 'Delete Item';
button.setAttribute('aria-label', 'Delete this item from your cart');
button.setAttribute('type', 'button');
```

**Best Practices:**
- ✅ **Use document fragments** for multiple elements
- ✅ **Set textContent** for user data to prevent XSS
- ✅ **Include accessibility attributes** (aria-label, etc.)
- ✅ **Use modern insertion methods** (append, prepend, etc.)
- ✅ **Clean up event listeners** before removing elements

**Memory Tip:** 🏗️ "**Create**, **Insert**, **Clone**, **Remove** - the DOM construction toolkit!"
```

## DOM Events

**Q: What are DOM events and how do you handle them?**

**A:** DOM events are signals that something has happened (user interactions, browser actions, API events). JavaScript can register event handlers to respond to these events:

```javascript
// Adding an event listener
const button = document.querySelector('button');
button.addEventListener('click', function() {
    console.log('Button clicked!');
});

// Using arrow functions
button.addEventListener('click', () => {
    console.log('Button clicked with arrow function!');
});

// Using named function as event handler (better for debugging)
function handleClick() {
    console.log('Button clicked!');
}
button.addEventListener('click', handleClick);

// Removing an event listener (must use the same function reference)
button.removeEventListener('click', handleClick);

// Adding options to event listeners
button.addEventListener('click', handleClick, {
    once: true,      // Remove after first trigger
    passive: true,   // Never calls preventDefault()
    capture: true    // Trigger during capture phase
});
```

**Q: How do you work with the event object and control event behavior?**

**A:** The event object contains information about the event and provides methods to control its behavior:

```javascript
button.addEventListener('click', function(event) {
    // Event information
    console.log('Event type:', event.type);              // "click"
    console.log('Target element:', event.target);        // The clicked element
    console.log('Current target:', event.currentTarget); // The element with the listener
    console.log('Mouse position:', event.clientX, event.clientY);
    console.log('Timestamp:', event.timeStamp);
    
    // Event control methods
    event.preventDefault();   // Prevents default behavior
    event.stopPropagation();  // Stops event from bubbling up
    event.stopImmediatePropagation(); // Stops all other listeners on this element
});

// Preventing default behavior examples
const link = document.querySelector('a');
link.addEventListener('click', function(event) {
    event.preventDefault();  // Prevents navigating to the link's href
    console.log('Link click prevented, custom action instead');
    // Custom navigation logic here
});

const form = document.querySelector('form');
form.addEventListener('submit', function(event) {
    event.preventDefault();  // Prevents form submission
    // Custom form handling logic
    const formData = new FormData(event.target);
    console.log('Form data:', Object.fromEntries(formData));
});

// Event delegation example
document.querySelector('.parent').addEventListener('click', function(event) {
    // Only respond to clicks on child buttons
    if (event.target.matches('button.child')) {
        console.log('Child button clicked:', event.target.textContent);
    }
});
```

**Q: What are the different types of events and when do you use them?**

**A:** JavaScript supports many event types for different user interactions and browser actions:

```javascript
// Mouse events
const element = document.querySelector('.interactive');

element.addEventListener('click', () => console.log('Single click'));
element.addEventListener('dblclick', () => console.log('Double click'));
element.addEventListener('mousedown', () => console.log('Mouse button pressed'));
element.addEventListener('mouseup', () => console.log('Mouse button released'));
element.addEventListener('mouseover', () => console.log('Mouse entered element'));
element.addEventListener('mouseout', () => console.log('Mouse left element'));
element.addEventListener('mouseenter', () => console.log('Mouse entered (no bubbling)'));
element.addEventListener('mouseleave', () => console.log('Mouse left (no bubbling)'));
element.addEventListener('mousemove', (e) => {
    console.log('Mouse coordinates:', e.clientX, e.clientY);
});

// Keyboard events
document.addEventListener('keydown', (e) => {
    console.log('Key pressed:', e.key, 'Code:', e.code);
    console.log('Modifiers - Ctrl:', e.ctrlKey, 'Shift:', e.shiftKey, 'Alt:', e.altKey);
    
    // Handle specific key combinations
    if (e.ctrlKey && e.key === 's') {
        e.preventDefault();
        console.log('Ctrl+S pressed - custom save action');
    }
});

document.addEventListener('keyup', (e) => console.log('Key released:', e.key));

// Form events
const input = document.querySelector('input');
input.addEventListener('focus', () => console.log('Input focused'));
input.addEventListener('blur', () => console.log('Input lost focus'));
input.addEventListener('input', (e) => console.log('Input value changed:', e.target.value));
input.addEventListener('change', (e) => console.log('Input value committed:', e.target.value));

// Window/Document events
window.addEventListener('load', () => console.log('Page fully loaded (including images)'));
document.addEventListener('DOMContentLoaded', () => console.log('DOM fully loaded'));
window.addEventListener('resize', () => console.log('Window resized'));
window.addEventListener('scroll', () => console.log('Window scrolled'));
window.addEventListener('beforeunload', (e) => {
    e.preventDefault();
    e.returnValue = ''; // Shows browser confirmation dialog
});
```

**Q: What is event delegation and why is it useful?**

**A:** Event delegation uses event bubbling to handle events on multiple elements with a single listener:

```javascript
// ❌ Inefficient: Multiple event listeners
document.querySelectorAll('.item').forEach(item => {
    item.addEventListener('click', handleItemClick);
});

// ✅ Efficient: Event delegation
document.querySelector('.container').addEventListener('click', function(event) {
    // Check if clicked element matches our criteria
    if (event.target.matches('.item')) {
        handleItemClick.call(event.target, event);
    }
    
    // Handle different types of child elements
    if (event.target.matches('.delete-btn')) {
        deleteItem(event.target.closest('.item'));
    }
    
    if (event.target.matches('.edit-btn')) {
        editItem(event.target.closest('.item'));
    }
});

// Benefits of event delegation:
// ✅ Works with dynamically added elements
// ✅ Better performance with many elements
// ✅ Simpler memory management
// ✅ Single source of truth for event handling

// Dynamic content example
function addNewItem() {
    const container = document.querySelector('.container');
    const newItem = document.createElement('div');
    newItem.className = 'item';
    newItem.innerHTML = '<button class="delete-btn">Delete</button>';
    container.appendChild(newItem);
    // No need to add event listener - delegation handles it!
}
```

**Best Practices:**
- ✅ **Use event delegation** for dynamic content and multiple similar elements
- ✅ **Prevent default** when needed (forms, links)
- ✅ **Use named functions** for event handlers that you might need to remove
- ✅ **Check event.target** in delegated handlers
- ✅ **Use passive listeners** for scroll/touch events when not preventing default

**Memory Tip:** 🎯 "Events **bubble up** like balloons - catch them at any level with **delegation**!"

**Q: What is event bubbling and how does it work?**

**A:** Event bubbling is the process where an event starts at the target element and "bubbles up" through its parent elements to the document root:

```javascript
// HTML structure for examples:
/*
<div id="grandparent">
    <div id="parent">
        <button id="child">Click me!</button>
    </div>
</div>
*/

// Event bubbling demonstration
document.getElementById('grandparent').addEventListener('click', function(e) {
    console.log('Grandparent clicked!', e.target.id, e.currentTarget.id);
});

document.getElementById('parent').addEventListener('click', function(e) {
    console.log('Parent clicked!', e.target.id, e.currentTarget.id);
});

document.getElementById('child').addEventListener('click', function(e) {
    console.log('Child clicked!', e.target.id, e.currentTarget.id);
});

// When button is clicked, you'll see:
// "Child clicked! child child"
// "Parent clicked! child parent"  
// "Grandparent clicked! child grandparent"

// Key differences:
// - e.target: The element that was actually clicked (always 'child')
// - e.currentTarget: The element that has the event listener (changes as it bubbles)

// Controlling event bubbling
document.getElementById('child').addEventListener('click', function(e) {
    console.log('Child clicked - stopping propagation');
    e.stopPropagation();  // Prevents bubbling to parent elements
    // Parent and grandparent listeners won't fire
});

// Stop immediate propagation (stops other listeners on same element too)
document.getElementById('child').addEventListener('click', function(e) {
    console.log('First listener');
    e.stopImmediatePropagation();  // Stops all other listeners
});

document.getElementById('child').addEventListener('click', function(e) {
    console.log('Second listener - will not fire');
});
```

**Q: What are the three phases of event flow?**

**A:** DOM events go through three phases: Capture → Target → Bubble:

```javascript
// Event flow phases demonstration
const grandparent = document.getElementById('grandparent');
const parent = document.getElementById('parent');
const child = document.getElementById('child');

// Capture phase (top to bottom)
grandparent.addEventListener('click', function(e) {
    console.log('1. Grandparent CAPTURE phase');
}, true);  // true = capture phase

parent.addEventListener('click', function(e) {
    console.log('2. Parent CAPTURE phase');
}, true);

// Target phase (at the target element)
child.addEventListener('click', function(e) {
    console.log('3. Child TARGET phase');
});

// Bubble phase (bottom to top)
parent.addEventListener('click', function(e) {
    console.log('4. Parent BUBBLE phase');
}, false);  // false = bubble phase (default)

grandparent.addEventListener('click', function(e) {
    console.log('5. Grandparent BUBBLE phase');
}, false);

// Event phase property
document.addEventListener('click', function(e) {
    const phases = {
        0: 'NONE',
        1: 'CAPTURING_PHASE',
        2: 'AT_TARGET',
        3: 'BUBBLING_PHASE'
    };
    console.log('Current phase:', phases[e.eventPhase]);
});

// Practical use case: Modal closing
document.querySelector('.modal-overlay').addEventListener('click', function(e) {
    // Close modal when clicking overlay, but not when clicking modal content
    if (e.target === e.currentTarget) {  // Clicked directly on overlay
        closeModal();
    }
    // Don't stop propagation - let content handle its own clicks
});

document.querySelector('.modal-content').addEventListener('click', function(e) {
    e.stopPropagation();  // Prevent modal from closing when clicking content
});
```

**Q: How do you use event bubbling for efficient event delegation?**

**A:** Event delegation leverages bubbling to handle events on multiple elements with a single listener:

```javascript
// Without delegation (inefficient for many elements)
document.querySelectorAll('.todo-item').forEach(item => {
    item.addEventListener('click', handleTodoClick);  // Many listeners
});

// With delegation (efficient)
document.querySelector('.todo-list').addEventListener('click', function(e) {
    // Use bubbling to catch clicks from any child element
    const todoItem = e.target.closest('.todo-item');
    if (todoItem) {
        handleTodoClick.call(todoItem, e);
    }
    
    // Handle different child elements
    if (e.target.matches('.delete-btn')) {
        deleteTodo(e.target.closest('.todo-item'));
    }
    
    if (e.target.matches('.edit-btn')) {
        editTodo(e.target.closest('.todo-item'));
    }
    
    if (e.target.matches('.checkbox')) {
        toggleTodo(e.target.closest('.todo-item'));
    }
});

// Table row delegation example
document.querySelector('table tbody').addEventListener('click', function(e) {
    const row = e.target.closest('tr');
    if (!row) return;
    
    if (e.target.matches('.edit-btn')) {
        editRow(row);
    } else if (e.target.matches('.delete-btn')) {
        deleteRow(row);
    } else {
        selectRow(row);  // Click anywhere else selects row
    }
});

// Form delegation for dynamic inputs
document.querySelector('.form-container').addEventListener('input', function(e) {
    if (e.target.matches('input[type="email"]')) {
        validateEmail(e.target);
    } else if (e.target.matches('input[type="password"]')) {
        validatePassword(e.target);
    } else if (e.target.matches('input[required]')) {
        validateRequired(e.target);
    }
});

// Benefits of delegation:
// ✅ Works with dynamically added elements
// ✅ Better memory usage (fewer listeners)
// ✅ Easier to manage and debug
// ✅ Automatically handles element removal
```

**Best Practices for Event Bubbling:**
- ✅ **Use delegation** for lists, tables, and dynamic content
- ✅ **Check e.target** to identify the actual clicked element
- ✅ **Use e.target.closest()** to find ancestor elements
- ✅ **Stop propagation** only when necessary
- ✅ **Consider capture phase** for special cases (document-wide handlers)

**Memory Tips:** 
- 🎈 "Events **bubble up** like air bubbles in water"
- 🎯 "**Delegation** = one parent catches events from all children"
- 🛑 "**stopPropagation()** = burst the bubble before it rises"
```

## DOM Traversal

**Q: How do you navigate between DOM elements using parent-child relationships?**

**A:** DOM nodes have family-like relationships that allow navigation between elements:

```javascript
// Starting with a child element
const child = document.querySelector('.child');

// Get the parent node
const parent = child.parentNode;        // Any node (including text nodes)
const parentElement = child.parentElement;  // Only element nodes (recommended)
console.log('Parent:', parentElement);

// Check if element has a parent
if (child.parentElement) {
    console.log('Has parent:', child.parentElement.tagName);
}

// Get children of an element
const parentElement = document.querySelector('.parent');
const children = parentElement.children;  // HTMLCollection of child elements only
const childNodes = parentElement.childNodes;  // NodeList of all child nodes (including text)

console.log('Number of child elements:', children.length);
console.log('Number of child nodes:', childNodes.length);

// Access specific children
const firstChild = parentElement.firstElementChild;  // First child element
const lastChild = parentElement.lastElementChild;   // Last child element
const secondChild = children[1];  // Access by index

// Check if element has children
if (parentElement.hasChildNodes()) {
    console.log('Element has children');
}

// Iterate through child elements
Array.from(children).forEach((child, index) => {
    console.log(`Child ${index}:`, child.tagName);
});
```

**Q: How do you navigate between sibling elements?**

**A:** Sibling navigation allows moving horizontally through elements at the same level:

```javascript
// Starting with a middle element
const middle = document.querySelector('.middle');

// Get siblings (be careful of text nodes)
const previousSibling = middle.previousSibling;          // Previous node (might be text)
const previousElement = middle.previousElementSibling;   // Previous element (recommended)
const nextSibling = middle.nextSibling;                  // Next node (might be text)
const nextElement = middle.nextElementSibling;           // Next element (recommended)

console.log('Previous element:', previousElement?.tagName);
console.log('Next element:', nextElement?.tagName);

// Navigate through all siblings
let current = parentElement.firstElementChild;
while (current) {
    console.log('Sibling:', current.tagName);
    current = current.nextElementSibling;
}

// Get all siblings (excluding self)
function getSiblings(element) {
    return Array.from(element.parentElement.children).filter(child => child !== element);
}

const siblings = getSiblings(middle);
console.log('All siblings:', siblings);
```

**Q: What are advanced DOM traversal techniques?**

**A:** Use Intersection Observer API for efficient visibility detection and modern positioning strategies:

```javascript
// Intersection Observer for efficient scroll handling
const observerOptions = {
    root: null,           // Use viewport as root
    rootMargin: '0px',    // Margin around root
    threshold: [0, 0.25, 0.5, 0.75, 1.0]  // Multiple thresholds
};

const observer = new IntersectionObserver(function(entries) {
    entries.forEach(entry => {
        const element = entry.target;
        const visibilityRatio = entry.intersectionRatio;
        
        if (entry.isIntersecting) {
            // Element is visible
            element.classList.add('in-view');
            
            // Lazy load images
            if (element.tagName === 'IMG' && element.dataset.src) {
                element.src = element.dataset.src;
                element.removeAttribute('data-src');
            }
            
            // Animate on scroll
            if (element.classList.contains('animate-on-scroll')) {
                element.classList.add('animated');
            }
        } else {
            // Element is not visible
            element.classList.remove('in-view');
        }
        
        // Handle different visibility levels
        if (visibilityRatio > 0.5) {
            element.classList.add('mostly-visible');
        } else {
            element.classList.remove('mostly-visible');
        }
    });
}, observerOptions);

// Observe elements
document.querySelectorAll('.lazy-load, .animate-on-scroll').forEach(el => {
    observer.observe(el);
});

// Position calculation utilities
class PositionUtils {
    // Get element center point
    static getElementCenter(element) {
        const rect = element.getBoundingClientRect();
        return {
            x: rect.left + rect.width / 2,
            y: rect.top + rect.height / 2
        };
    }
    
    // Calculate distance between two elements
    static getDistance(element1, element2) {
        const center1 = this.getElementCenter(element1);
        const center2 = this.getElementCenter(element2);
        
        return Math.sqrt(
            Math.pow(center2.x - center1.x, 2) + 
            Math.pow(center2.y - center1.y, 2)
        );
    }
    
    // Check if point is inside element
    static isPointInElement(x, y, element) {
        const rect = element.getBoundingClientRect();
        return (
            x >= rect.left &&
            x <= rect.right &&
            y >= rect.top &&
            y <= rect.bottom
        );
    }
    
    // Get closest element to point
    static getClosestElement(x, y, elements) {
        let closest = null;
        let minDistance = Infinity;
        
        elements.forEach(element => {
            const center = this.getElementCenter(element);
            const distance = Math.sqrt(
                Math.pow(x - center.x, 2) + 
                Math.pow(y - center.y, 2)
            );
            
            if (distance < minDistance) {
                minDistance = distance;
                closest = element;
            }
        });
        
        return closest;
    }
}

// Virtual scrolling for large lists
class VirtualScroller {
    constructor(container, items, itemHeight, visibleCount) {
        this.container = container;
        this.items = items;
        this.itemHeight = itemHeight;
        this.visibleCount = visibleCount;
        this.scrollTop = 0;
        this.init();
    }
    
    init() {
        this.container.style.height = this.items.length * this.itemHeight + 'px';
        this.container.style.position = 'relative';
        this.container.style.overflow = 'auto';
        
        this.viewport = document.createElement('div');
        this.viewport.style.position = 'absolute';
        this.viewport.style.top = '0';
        this.viewport.style.left = '0';
        this.viewport.style.right = '0';
        this.container.appendChild(this.viewport);
        
        this.container.addEventListener('scroll', () => this.handleScroll());
        this.render();
    }
    
    handleScroll() {
        this.scrollTop = this.container.scrollTop;
        this.render();
    }
    
    render() {
        const startIndex = Math.floor(this.scrollTop / this.itemHeight);
        const endIndex = Math.min(startIndex + this.visibleCount, this.items.length);
        
        this.viewport.innerHTML = '';
        this.viewport.style.transform = `translateY(${startIndex * this.itemHeight}px)`;
        
        for (let i = startIndex; i < endIndex; i++) {
            const itemElement = document.createElement('div');
            itemElement.style.height = this.itemHeight + 'px';
            itemElement.textContent = this.items[i];
            itemElement.classList.add('virtual-item');
            this.viewport.appendChild(itemElement);
        }
    }
}

// Usage examples
document.addEventListener('DOMContentLoaded', function() {
    // Smooth scroll to top button
    const backToTop = document.querySelector('.back-to-top');
    backToTop?.addEventListener('click', function() {
        scrollToPosition(0, 0, 'smooth');
    });
    
    // Parallax scrolling effect
    const parallaxElements = document.querySelectorAll('.parallax');
    window.addEventListener('scroll', function() {
        const scrolled = window.scrollY;
        
        parallaxElements.forEach(element => {
            const speed = element.dataset.speed || 0.5;
            const yPos = -(scrolled * speed);
            element.style.transform = `translateY(${yPos}px)`;
        });
    });
    
    // Sticky sidebar
    const sidebar = document.querySelector('.sidebar');
    if (sidebar) {
        makeSticky(sidebar, 20); // 20px from top when sticky
    }
});
```

**Best Practices:**
- ✅ **Use Intersection Observer** for efficient scroll-based effects
- ✅ **Throttle scroll events** with requestAnimationFrame
- ✅ **Cache dimension calculations** to avoid layout thrashing
- ✅ **Use transform** instead of changing position for animations
- ✅ **Consider virtual scrolling** for large lists

**Memory Tip:** 📐 "**Position** is about **where**, **scroll** is about **how far**, and **intersection** is about **what's visible**!"
```

## Element Positioning and Scrolling

**Q: How do you get and manipulate element positions and dimensions?**

**A:** JavaScript provides several properties and methods to work with element positioning, dimensions, and viewport coordinates:

```javascript
// Getting element dimensions and position
const element = document.querySelector('.box');

// Dimensions (including padding, excluding border and margin)
console.log('Width:', element.clientWidth);
console.log('Height:', element.clientHeight);

// Dimensions (including padding and border, excluding margin)
console.log('Offset Width:', element.offsetWidth);
console.log('Offset Height:', element.offsetHeight);

// Dimensions (including padding, border, and scrollbars)
console.log('Scroll Width:', element.scrollWidth);   // Full content width
console.log('Scroll Height:', element.scrollHeight); // Full content height

// Position relative to offset parent
console.log('Offset Left:', element.offsetLeft);
console.log('Offset Top:', element.offsetTop);
console.log('Offset Parent:', element.offsetParent);

// Position relative to viewport
const rect = element.getBoundingClientRect();
console.log('Viewport position:', {
    left: rect.left,
    top: rect.top,
    right: rect.right,
    bottom: rect.bottom,
    width: rect.width,
    height: rect.height
});

// Scroll position
console.log('Scroll Left:', element.scrollLeft);
console.log('Scroll Top:', element.scrollTop);

// Window/document dimensions and scroll
console.log('Window dimensions:', {
    width: window.innerWidth,
    height: window.innerHeight,
    scrollX: window.scrollX,     // Horizontal scroll
    scrollY: window.scrollY      // Vertical scroll
});

// Document dimensions
console.log('Document dimensions:', {
    width: document.documentElement.scrollWidth,
    height: document.documentElement.scrollHeight
});
```

**Q: How do you programmatically scroll and control element visibility?**

**A:** Use modern scrolling APIs and intersection detection for smooth user experiences:

```javascript
// Smooth scrolling to element
function scrollToElement(element, behavior = 'smooth') {
    element.scrollIntoView({ 
        behavior: behavior,        // 'smooth' or 'auto'
        block: 'start',           // 'start', 'center', 'end', 'nearest'
        inline: 'nearest'         // 'start', 'center', 'end', 'nearest'
    });
}

// Scroll to specific position
function scrollToPosition(x, y, behavior = 'smooth') {
    window.scrollTo({
        left: x,
        top: y,
        behavior: behavior
    });
}

// Scroll by specific amount
function scrollBy(x, y, behavior = 'smooth') {
    window.scrollBy({
        left: x,
        top: y,
        behavior: behavior
    });
}

// Check if element is in viewport
function isElementInViewport(element) {
    const rect = element.getBoundingClientRect();
    return (
        rect.top >= 0 &&
        rect.left >= 0 &&
        rect.bottom <= window.innerHeight &&
        rect.right <= window.innerWidth
    );
}

// Get element's visibility percentage
function getVisibilityPercentage(element) {
    const rect = element.getBoundingClientRect();
    const elementArea = rect.width * rect.height;
    
    if (elementArea === 0) return 0;
    
    const visibleWidth = Math.min(rect.right, window.innerWidth) - Math.max(rect.left, 0);
    const visibleHeight = Math.min(rect.bottom, window.innerHeight) - Math.max(rect.top, 0);
    
    if (visibleWidth <= 0 || visibleHeight <= 0) return 0;
    
    const visibleArea = visibleWidth * visibleHeight;
    return (visibleArea / elementArea) * 100;
}

// Scroll tracking and handling
let isScrolling = false;
window.addEventListener('scroll', function() {
    if (!isScrolling) {
        requestAnimationFrame(function() {
            handleScroll();
            isScrolling = false;
        });
        isScrolling = true;
    }
});

function handleScroll() {
    const scrollPercent = (window.scrollY / (document.documentElement.scrollHeight - window.innerHeight)) * 100;
    console.log('Scroll percentage:', scrollPercent);
    
    // Update scroll indicator
    const indicator = document.querySelector('.scroll-indicator');
    if (indicator) {
        indicator.style.width = scrollPercent + '%';
    }
    
    // Show/hide back-to-top button
    const backToTop = document.querySelector('.back-to-top');
    if (window.scrollY > 300) {
        backToTop?.classList.add('visible');
    } else {
        backToTop?.classList.remove('visible');
    }
}

// Sticky positioning simulation (for older browsers)
function makeSticky(element, offsetTop = 0) {
    const originalTop = element.offsetTop;
    
    window.addEventListener('scroll', function() {
        if (window.scrollY >= originalTop - offsetTop) {
            element.style.position = 'fixed';
            element.style.top = offsetTop + 'px';
            element.classList.add('sticky');
        } else {
            element.style.position = 'static';
            element.style.top = 'auto';
            element.classList.remove('sticky');
        }
    });
}
```

**Q: What are advanced positioning techniques and intersection observer?**

**A:** Use Intersection Observer API for efficient visibility detection and modern positioning strategies:

```javascript
// Intersection Observer for efficient scroll handling
const observerOptions = {
    root: null,           // Use viewport as root
    rootMargin: '0px',    // Margin around root
    threshold: [0, 0.25, 0.5, 0.75, 1.0]  // Multiple thresholds
};

const observer = new IntersectionObserver(function(entries) {
    entries.forEach(entry => {
        const element = entry.target;
        const visibilityRatio = entry.intersectionRatio;
        
        if (entry.isIntersecting) {
            // Element is visible
            element.classList.add('in-view');
            
            // Lazy load images
            if (element.tagName === 'IMG' && element.dataset.src) {
                element.src = element.dataset.src;
                element.removeAttribute('data-src');
            }
            
            // Animate on scroll
            if (element.classList.contains('animate-on-scroll')) {
                element.classList.add('animated');
            }
        } else {
            // Element is not visible
            element.classList.remove('in-view');
        }
        
        // Handle different visibility levels
        if (visibilityRatio > 0.5) {
            element.classList.add('mostly-visible');
        } else {
            element.classList.remove('mostly-visible');
        }
    });
}, observerOptions);

// Observe elements
document.querySelectorAll('.lazy-load, .animate-on-scroll').forEach(el => {
    observer.observe(el);
});

// Position calculation utilities
class PositionUtils {
    // Get element center point
    static getElementCenter(element) {
        const rect = element.getBoundingClientRect();
        return {
            x: rect.left + rect.width / 2,
            y: rect.top + rect.height / 2
        };
    }
    
    // Calculate distance between two elements
    static getDistance(element1, element2) {
        const center1 = this.getElementCenter(element1);
        const center2 = this.getElementCenter(element2);
        
        return Math.sqrt(
            Math.pow(center2.x - center1.x, 2) + 
            Math.pow(center2.y - center1.y, 2)
        );
    }
    
    // Check if point is inside element
    static isPointInElement(x, y, element) {
        const rect = element.getBoundingClientRect();
        return (
            x >= rect.left &&
            x <= rect.right &&
            y >= rect.top &&
            y <= rect.bottom
        );
    }
    
    // Get closest element to point
    static getClosestElement(x, y, elements) {
        let closest = null;
        let minDistance = Infinity;
        
        elements.forEach(element => {
            const center = this.getElementCenter(element);
            const distance = Math.sqrt(
                Math.pow(x - center.x, 2) + 
                Math.pow(y - center.y, 2)
            );
            
            if (distance < minDistance) {
                minDistance = distance;
                closest = element;
            }
        });
        
        return closest;
    }
}

// Virtual scrolling for large lists
class VirtualScroller {
    constructor(container, items, itemHeight, visibleCount) {
        this.container = container;
        this.items = items;
        this.itemHeight = itemHeight;
        this.visibleCount = visibleCount;
        this.scrollTop = 0;
        this.init();
    }
    
    init() {
        this.container.style.height = this.items.length * this.itemHeight + 'px';
        this.container.style.position = 'relative';
        this.container.style.overflow = 'auto';
        
        this.viewport = document.createElement('div');
        this.viewport.style.position = 'absolute';
        this.viewport.style.top = '0';
        this.viewport.style.left = '0';
        this.viewport.style.right = '0';
        this.container.appendChild(this.viewport);
        
        this.container.addEventListener('scroll', () => this.handleScroll());
        this.render();
    }
    
    handleScroll() {
        this.scrollTop = this.container.scrollTop;
        this.render();
    }
    
    render() {
        const startIndex = Math.floor(this.scrollTop / this.itemHeight);
        const endIndex = Math.min(startIndex + this.visibleCount, this.items.length);
        
        this.viewport.innerHTML = '';
        this.viewport.style.transform = `translateY(${startIndex * this.itemHeight}px)`;
        
        for (let i = startIndex; i < endIndex; i++) {
            const itemElement = document.createElement('div');
            itemElement.style.height = this.itemHeight + 'px';
            itemElement.textContent = this.items[i];
            itemElement.classList.add('virtual-item');
            this.viewport.appendChild(itemElement);
        }
    }
}

// Usage examples
document.addEventListener('DOMContentLoaded', function() {
    // Smooth scroll to top button
    const backToTop = document.querySelector('.back-to-top');
    backToTop?.addEventListener('click', function() {
        scrollToPosition(0, 0, 'smooth');
    });
    
    // Parallax scrolling effect
    const parallaxElements = document.querySelectorAll('.parallax');
    window.addEventListener('scroll', function() {
        const scrolled = window.scrollY;
        
        parallaxElements.forEach(element => {
            const speed = element.dataset.speed || 0.5;
            const yPos = -(scrolled * speed);
            element.style.transform = `translateY(${yPos}px)`;
        });
    });
    
    // Sticky sidebar
    const sidebar = document.querySelector('.sidebar');
    if (sidebar) {
        makeSticky(sidebar, 20); // 20px from top when sticky
    }
});
```

**Best Practices:**
- ✅ **Use Intersection Observer** for efficient scroll-based effects
- ✅ **Throttle scroll events** with requestAnimationFrame
- ✅ **Cache dimension calculations** to avoid layout thrashing
- ✅ **Use transform** instead of changing position for animations
- ✅ **Consider virtual scrolling** for large lists

**Memory Tip:** 📐 "**Position** is about **where**, **scroll** is about **how far**, and **intersection** is about **what's visible**!"
