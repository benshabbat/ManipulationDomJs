# DOM Manipulation Exercises - JavaScript

## 🎯 Project Goal
A comprehensive collection of progressive exercises for learning DOM manipulation from beginner to advanced level.

---

## 📚 Level 1: Selecting Elements

### Exercise 1.1: Selecting by ID
```html
<!-- HTML -->
<div id="mainContent">Main content</div>
```

**Goal:** Select an element by ID

**Hint:** Use `document.getElementById()` to select an element by its ID

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
// Select by ID
const mainContent = document.getElementById("mainContent");
console.log(mainContent);
```

</details>

---

### Exercise 1.2: Selecting by Class
```html
<!-- HTML -->
<p class="paragraph">Paragraph 1</p>
<p class="paragraph">Paragraph 2</p>
```

**Goal:** Select elements by class name

**Hint:** Use `getElementsByClassName()` or `querySelectorAll()` to select multiple elements

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
// Select by class (returns HTMLCollection)
const paragraphs = document.getElementsByClassName("paragraph");
console.log(paragraphs);

// Alternative way with querySelectorAll
const paragraphs2 = document.querySelectorAll(".paragraph");
console.log(paragraphs2); // NodeList
```

</details>

---

### Exercise 1.3: Selecting with querySelector
```html
<!-- HTML -->
<div id="container">
  <button class="btn">Button 1</button>
  <button class="btn">Button 2</button>
</div>
```

**Goal:** Select elements using CSS selectors

**Hint:** `querySelector()` works with any CSS selector (ID, class, tag, etc.)

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
// Select first element with selector
const firstBtn = document.querySelector("button");
console.log(firstBtn); // First button

// Select by ID
const container = document.querySelector("#container");

// Select by class
const btn = document.querySelector(".btn");

// Select nested element
const btnInDiv = document.querySelector("div button");
```

</details>

---

### Exercise 1.4: Selecting Multiple Elements
```html
<!-- HTML -->
<p>Paragraph 1</p>
<p>Paragraph 2</p>
<p>Paragraph 3</p>
```

**Goal:** Select multiple elements and iterate through them

**Hint:** Use `querySelectorAll()` to get all matching elements, then loop with `forEach()`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
// Select all paragraphs
const allParagraphs = document.querySelectorAll("p");
console.log(allParagraphs); // NodeList with 3 elements

// Loop through each element
allParagraphs.forEach(function(p) {
  console.log(p.textContent);
});

// Or with for loop
for (let i = 0; i < allParagraphs.length; i++) {
  console.log(allParagraphs[i].textContent);
}
```

</details>

---

## 📚 Level 2: Changing Content

### Exercise 2.1: Changing textContent
```html
<!-- HTML -->
<div id="box">Old content</div>
```

**Goal:** Change text content of an element

**Hint:** Use `.textContent` property to set text

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const div = document.getElementById("box");

// Change text content
div.textContent = "New content";
console.log(div.textContent); // "New content"
```

</details>

---

### Exercise 2.2: Changing innerHTML
```html
<!-- HTML -->
<div id="box">Old content</div>
```

**Goal:** Change HTML content with tags

**Hint:** Use `.innerHTML` to set HTML tags

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const div = document.getElementById("box");

// Change content with HTML
div.innerHTML = "<strong>Formatted content</strong>";
div.innerHTML = "<p>New paragraph</p> <p>Another paragraph</p>";

// Add to existing content
div.innerHTML += "<span> - Additional text</span>";
```

</details>

---

### Exercise 2.3: Changing input value
```html
<!-- HTML -->
<input id="field" type="text" value="Old value">
```

**Goal:** Change input field values

**Hint:** Use `.value` property on input elements

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const field = document.getElementById("field");

// Read the value
console.log(field.value); // "Old value"

// Change the value
field.value = "New value";

// Clear the value
field.value = "";
```

</details>

---

## 📚 Level 3: Styling Elements

### Exercise 3.1: Changing color and font size
```html
<!-- HTML -->
<div id="box">Text to style</div>
```

**Goal:** Change basic styles

**Hint:** Use `element.style.propertyName` to change CSS properties (use camelCase)

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const div = document.getElementById("box");

// Change color
div.style.color = "red";

// Change font size
div.style.fontSize = "20px";

// Change background color
div.style.backgroundColor = "yellow";

// Change padding
div.style.padding = "10px";
```

</details>

---

### Exercise 3.2: Working with Classes
```html
<!-- HTML -->
<style>
  .active {
    color: green;
    font-weight: bold;
  }
  .highlight {
    background-color: yellow;
  }
</style>
<div id="box">Text</div>
```

**Goal:** Work with CSS classes

**Hint:** Use `classList.add()`, `classList.remove()`, `classList.toggle()`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const div = document.getElementById("box");

// Add class
div.classList.add("active");

// Remove class
div.classList.remove("highlight");

// Toggle class (add if not exists, remove if exists)
div.classList.toggle("active");

// Check if has class
if (div.classList.contains("active")) {
  console.log("Has active class");
}
```

</details>

---

### Exercise 3.3: Changing display
```html
<!-- HTML -->
<div id="box1">Section 1</div>
<div id="box2">Section 2</div>
```

**Goal:** Show and hide elements

**Hint:** Use `display: "none"` to hide and `display: "block"` to show, or `visibility: "hidden"`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const box1 = document.getElementById("box1");
const box2 = document.getElementById("box2");

// Hide (still takes space)
box1.style.visibility = "hidden";

// Hide (doesn't take space)
box2.style.display = "none";

// Show again
box2.style.display = "block";
```

</details>

---

## 📚 Level 4: Working with Attributes

### Exercise 4.1: Reading and changing attributes
```html
<!-- HTML -->
<img id="photo" src="image1.jpg" alt="Photo">
<a id="link" href="https://google.com">Google</a>
```

**Goal:** Read and change basic attributes

**Hint:** Access attributes directly as properties: `element.src`, `element.href`, `element.alt`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
// Read attribute
const img = document.getElementById("photo");
console.log(img.src); // "image1.jpg"
console.log(img.alt); // "Photo"

// Change attribute
img.src = "image2.jpg";
img.alt = "New photo";

// With link
const link = document.getElementById("link");
console.log(link.href); // "https://google.com"
link.href = "https://facebook.com";
```

</details>

---

### Exercise 4.2: setAttribute and getAttribute
```html
<!-- HTML -->
<div id="box" class="old-class">Text</div>
```

**Goal:** Use getAttribute/setAttribute functions

**Hint:** These work for any attribute, including custom ones

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const div = document.getElementById("box");

// Read attribute with getAttribute
const classAttr = div.getAttribute("class");
console.log(classAttr); // "old-class"

// Change attribute with setAttribute
div.setAttribute("class", "new-class");
div.setAttribute("data-id", "123");
div.setAttribute("title", "This is a title");

// Remove attribute
div.removeAttribute("title");
```

</details>

---

### Exercise 4.3: Data attributes
```html
<!-- HTML -->
<div id="box" data-user-id="456" data-name="David">Text</div>
```

**Goal:** Work with data attributes

**Hint:** Use `element.dataset.attributeName` (camelCase, without "data-" prefix)

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const div = document.getElementById("box");

// Read data attributes
console.log(div.dataset.userId); // "456"
console.log(div.dataset.name); // "David"

// Change data attributes
div.dataset.userId = "789";
div.dataset.role = "admin"; // Add new data attribute

// Read another way
console.log(div.getAttribute("data-user-id")); // "789"
```

</details>

---

## 📚 Level 5: Creating and Deleting Elements

### Exercise 5.1: Creating a new element
```html
<!-- HTML -->
<div id="container"></div>
```

**Goal:** Create new elements

**Hint:** Use `document.createElement()` to create, then `appendChild()` to add to DOM

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const container = document.getElementById("container");

// Create new element
const newP = document.createElement("p");
newP.textContent = "This is a new paragraph";

// Add to DOM
container.appendChild(newP);

// Another way - create div
const newDiv = document.createElement("div");
newDiv.innerHTML = "<h3>New heading</h3>";
container.appendChild(newDiv);
```

</details>

---

### Exercise 5.2: Inserting at specific location
```html
<!-- HTML -->
<ul id="list">
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```

**Goal:** Insert elements at specific positions

**Hint:** Use `insertBefore()` to insert before a specific child

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const list = document.getElementById("list");

// Create new element
const newLi = document.createElement("li");
newLi.textContent = "Item 1.5";

// Insert before specific element
list.insertBefore(newLi, list.children[1]);

// Or append at the end
list.appendChild(newLi);
```

</details>

---

### Exercise 5.3: Deleting elements
```html
<!-- HTML -->
<div id="container">
  <p id="p1">Paragraph 1</p>
  <p id="p2">Paragraph 2</p>
  <p id="p3">Paragraph 3</p>
</div>
```

**Goal:** Delete elements

**Hint:** Use `.remove()` for modern browsers, or `.removeChild()` with parent

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const container = document.getElementById("container");
const p2 = document.getElementById("p2");

// Delete single element
p2.remove(); // Modern way

// Old way - delete through parent
container.removeChild(p2);

// Delete all children
container.innerHTML = ""; // Remove everything
```

</details>

---

## 📚 Level 6: Events

### Exercise 6.1: Click event
```html
<!-- HTML -->
<button id="btn">Click me</button>
<div id="message"></div>
```

**Goal:** Listen for click events

**Hint:** Use `addEventListener("click", function)`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("btn");

btn.addEventListener("click", function() {
  console.log("Button clicked!");
  document.getElementById("message").textContent = "Button was clicked";
});
```

</details>

---

### Exercise 6.2: Input event
```html
<!-- HTML -->
<input id="field" type="text" placeholder="Enter text">
<div id="message"></div>
```

**Goal:** Listen for input changes

**Hint:** Use `"input"` event for real-time changes, `"change"` for when field loses focus

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const field = document.getElementById("field");

// Change text while typing
field.addEventListener("input", function(event) {
  console.log(event.target.value);
  document.getElementById("message").textContent = event.target.value;
});

// Change - when field loses focus
field.addEventListener("change", function() {
  console.log("Field value changed!");
});
```

</details>

---

### Exercise 6.3: Mouse events
```html
<!-- HTML -->
<div id="box">Hover over me</div>
```

**Goal:** Handle mouse events

**Hint:** Use `"mouseover"` and `"mouseleave"` events

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const box = document.getElementById("box");

// When mouse enters
box.addEventListener("mouseover", function() {
  this.style.backgroundColor = "yellow";
});

// When mouse leaves
box.addEventListener("mouseleave", function() {
  this.style.backgroundColor = "white";
});
```

</details>

---

### Exercise 6.4: event.target
```html
<!-- HTML -->
<button class="btn">Button 1</button>
<button class="btn">Button 2</button>
<button class="btn">Button 3</button>
```

**Goal:** Use event.target

**Hint:** `event.target` is the element that triggered the event

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const buttons = document.querySelectorAll(".btn");

buttons.forEach(function(btn) {
  btn.addEventListener("click", function(event) {
    // event.target is the clicked button
    console.log(event.target.textContent); // "Button 1", etc.
    event.target.style.backgroundColor = "green";
  });
});
```

</details>

---

## 📚 Level 7: Dynamic Addition with Events

### Exercise 7.1: Dynamic addition of items
```html
<!-- HTML -->
<button id="addBtn">Add Item</button>
<ul id="list">
  <li>Item 1</li>
</ul>
```

**Goal:** Dynamically add elements

**Hint:** Increment a counter and create new elements in the event listener

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("addBtn");
const list = document.getElementById("list");
let count = 1;

btn.addEventListener("click", function() {
  count++;
  
  // Create new element
  const newLi = document.createElement("li");
  newLi.textContent = "Item " + count;
  
  // Add to list
  list.appendChild(newLi);
});
```

</details>

---

### Exercise 7.2: Dynamic deletion of items
```html
<!-- HTML -->
<button id="addBtn">Add</button>
<ul id="list"></ul>
```

**Goal:** Dynamically delete elements

**Hint:** Create a delete button for each item with its own event listener

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("addBtn");
const list = document.getElementById("list");
let count = 0;

btn.addEventListener("click", function() {
  count++;
  
  // Create new element
  const li = document.createElement("li");
  li.textContent = "Item " + count + " ";
  
  // Delete button
  const deleteBtn = document.createElement("button");
  deleteBtn.textContent = "Delete";
  deleteBtn.addEventListener("click", function() {
    li.remove();
  });
  
  li.appendChild(deleteBtn);
  list.appendChild(li);
});
```

</details>

---

## 📚 Level 8: Template Literals

### Exercise 8.1: Basic template literals
```html
<!-- HTML -->
<div id="output"></div>
```

**Goal:** Use template literals for string interpolation

**Hint:** Use backticks `` ` `` and `${variable}` syntax

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const name = "John";
const age = 25;
const city = "New York";

// Using template literals with backticks
const message = `Hello, ${name}! You are ${age} years old and live in ${city}.`;
console.log(message);

// Multiple lines
const multiLine = `
  Name: ${name}
  Age: ${age}
  City: ${city}
`;
console.log(multiLine);

document.getElementById("output").innerHTML = message;
```

</details>

---

### Exercise 8.2: Creating HTML with template literals
```html
<!-- HTML -->
<div id="output"></div>
```

**Goal:** Create HTML with template literals

**Hint:** Put HTML tags inside the template literal

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const person = {
  name: "Alice",
  age: 28,
  job: "Developer"
};

// Create HTML using template literal
const html = `
  <div class="card">
    <h2>${person.name}</h2>
    <p>Age: ${person.age}</p>
    <p>Job: ${person.job}</p>
  </div>
`;

document.getElementById("output").innerHTML = html;
```

</details>

---

### Exercise 8.3: List creation with template literals
```html
<!-- HTML -->
<div id="output"></div>
```

**Goal:** Create lists with template literals and map

**Hint:** Use `array.map()` to transform each item, then `.join("")` to combine

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const fruits = ["Apple", "Banana", "Orange", "Mango"];

// Create list HTML
const html = `
  <ul>
    ${fruits.map(fruit => `<li>${fruit}</li>`).join("")}
  </ul>
`;

document.getElementById("output").innerHTML = html;
```

</details>

---

### Exercise 8.4: Creating cards from objects
```html
<!-- HTML -->
<div id="output"></div>
```

**Goal:** Create complex layouts from data

**Hint:** Use map() on array of objects and access properties with dot notation

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const users = [
  { id: 1, name: "John", email: "john@example.com" },
  { id: 2, name: "Jane", email: "jane@example.com" },
  { id: 3, name: "Bob", email: "bob@example.com" }
];

// Create HTML for each user
const html = `
  <div class="users">
    ${users.map(user => `
      <div class="user-card">
        <h3>${user.name}</h3>
        <p>Email: ${user.email}</p>
      </div>
    `).join("")}
  </div>
`;

document.getElementById("output").innerHTML = html;
```

</details>

---

## 📚 Level 9: HTML Template Elements

### Exercise 9.1: Basic template element
```html
<!-- HTML -->
<template id="cardTemplate">
  <div class="card">
    <h2></h2>
    <p></p>
  </div>
</template>
<div id="output"></div>
```

**Goal:** Use HTML template elements

**Hint:** Clone with `cloneNode(true)`, then fill in the content with `querySelector()`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("cardTemplate");

// Clone the template
const clone = template.content.cloneNode(true);

// Fill in the data
clone.querySelector("h2").textContent = "Product A";
clone.querySelector("p").textContent = "Price: $100";

// Add to DOM
document.getElementById("output").appendChild(clone);
```

</details>

---

### Exercise 9.2: Template with multiple items
```html
<!-- HTML -->
<template id="itemTemplate">
  <li>
    <span class="title"></span>
    <span class="price"></span>
  </li>
</template>
<ul id="list"></ul>
```

**Goal:** Create multiple items from template

**Hint:** Loop through data array and clone for each item

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("itemTemplate");
const items = [
  { title: "Laptop", price: "$800" },
  { title: "Phone", price: "$500" },
  { title: "Tablet", price: "$300" }
];

items.forEach(item => {
  const clone = template.content.cloneNode(true);
  
  clone.querySelector(".title").textContent = item.title;
  clone.querySelector(".price").textContent = item.price;
  
  document.getElementById("list").appendChild(clone);
});
```

</details>

---

### Exercise 9.3: Template with event listeners
```html
<!-- HTML -->
<template id="buttonTemplate">
  <button class="action-btn"></button>
</template>
<div id="output"></div>
```

**Goal:** Add event listeners to template elements

**Hint:** Add the event listener to each cloned element before appending

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("buttonTemplate");
const buttons = ["Save", "Delete", "Edit", "Cancel"];

buttons.forEach(btnText => {
  const clone = template.content.cloneNode(true);
  const btn = clone.querySelector(".action-btn");
  
  btn.textContent = btnText;
  btn.addEventListener("click", function() {
    console.log(`${btnText} button clicked!`);
  });
  
  document.getElementById("output").appendChild(clone);
});
```

</details>

---

## 📚 Level 10: Advanced Template Patterns

### Exercise 10.1: Dynamic form generation
```html
<!-- HTML -->
<template id="inputTemplate">
  <div class="form-group">
    <label></label>
    <input type="text">
  </div>
</template>
<form id="form"></form>
```

**Goal:** Generate forms dynamically

**Hint:** Create a template with one form field, then clone it for each field

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("inputTemplate");
const fields = ["First Name", "Last Name", "Email"];

fields.forEach(field => {
  const clone = template.content.cloneNode(true);
  
  clone.querySelector("label").textContent = field;
  clone.querySelector("input").placeholder = field;
  
  document.getElementById("form").appendChild(clone);
});
```

</details>

---

### Exercise 10.2: Table generation from data
```html
<!-- HTML -->
<template id="rowTemplate">
  <tr>
    <td class="name"></td>
    <td class="email"></td>
    <td class="role"></td>
  </tr>
</template>
<table id="table">
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Role</th>
    </tr>
  </thead>
  <tbody id="tbody"></tbody>
</table>
```

**Goal:** Generate tables from data

**Hint:** Clone the row template for each data item

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("rowTemplate");
const data = [
  { name: "John", email: "john@example.com", role: "Admin" },
  { name: "Jane", email: "jane@example.com", role: "User" },
  { name: "Bob", email: "bob@example.com", role: "Editor" }
];

const tbody = document.getElementById("tbody");

data.forEach(row => {
  const clone = template.content.cloneNode(true);
  
  clone.querySelector(".name").textContent = row.name;
  clone.querySelector(".email").textContent = row.email;
  clone.querySelector(".role").textContent = row.role;
  
  tbody.appendChild(clone);
});
```

</details>

---

### Exercise 10.3: Interactive component creation
```html
<!-- HTML -->
<template id="todoTemplate">
  <div class="todo-item">
    <input type="checkbox" class="checkbox">
    <span class="text"></span>
    <button class="delete-btn">✕</button>
  </div>
</template>
<input id="todoInput" type="text" placeholder="Add new todo">
<button id="addBtn">Add</button>
<div id="todos"></div>
```

**Goal:** Create interactive components with templates

**Hint:** Add event listeners to each cloned element's checkbox and delete button

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("todoTemplate");
const input = document.getElementById("todoInput");
const addBtn = document.getElementById("addBtn");
const todosContainer = document.getElementById("todos");

addBtn.addEventListener("click", function() {
  if (input.value.trim()) {
    const clone = template.content.cloneNode(true);
    
    // Set text
    clone.querySelector(".text").textContent = input.value;
    
    // Add delete functionality
    clone.querySelector(".delete-btn").addEventListener("click", function(e) {
      e.target.parentElement.remove();
    });
    
    // Add checkbox functionality
    clone.querySelector(".checkbox").addEventListener("change", function() {
      if (this.checked) {
        clone.querySelector(".text").style.textDecoration = "line-through";
      } else {
        clone.querySelector(".text").style.textDecoration = "none";
      }
    });
    
    todosContainer.appendChild(clone);
    input.value = "";
  }
});

// Support Enter key
input.addEventListener("keypress", function(e) {
  if (e.key === "Enter") {
    addBtn.click();
  }
});
```

</details>

---

## 📚 Level 11: Template Literals vs Templates

### Exercise 11.1: Comparison - Speed and simplicity
```html
<!-- HTML -->
<div id="output1"></div>
<div id="output2"></div>
```

**Goal:** Understand when to use each method

**Hint:** Template literals are simpler for quick rendering, templates are better for reusable components

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const products = [
  { name: "Laptop", price: 800 },
  { name: "Phone", price: 500 }
];

// Method 1: Template literals (simpler for small projects)
const html1 = `
  <div class="products">
    ${products.map(p => `
      <div class="product">
        <h3>${p.name}</h3>
        <p>$${p.price}</p>
      </div>
    `).join("")}
  </div>
`;
document.getElementById("output1").innerHTML = html1;

// Method 2: Template elements (better for reusable components)
// See Exercise 9.2 for template element approach
```

</details>

---

### Exercise 11.2: Complex template literal example
```html
<!-- HTML -->
<div id="output"></div>
```

**Goal:** Use advanced template literals with conditionals

**Hint:** Use ternary operator inside `${}` for conditional content

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const products = [
  { id: 1, name: "Laptop", price: 800, inStock: true },
  { id: 2, name: "Phone", price: 500, inStock: false },
  { id: 3, name: "Tablet", price: 300, inStock: true }
];

const html = `
  <div class="product-grid">
    ${products.map(p => `
      <div class="product-card ${p.inStock ? 'in-stock' : 'out-of-stock'}">
        <h3>${p.name}</h3>
        <p class="price">$${p.price}</p>
        <p class="status">${p.inStock ? 'Available' : 'Out of Stock'}</p>
        <button class="buy-btn" ${p.inStock ? '' : 'disabled'}>
          ${p.inStock ? 'Buy Now' : 'Unavailable'}
        </button>
      </div>
    `).join("")}
  </div>
`;

document.getElementById("output").innerHTML = html;

// Add event listeners
document.querySelectorAll(".buy-btn:not(:disabled)").forEach(btn => {
  btn.addEventListener("click", function() {
    alert("Product added to cart!");
  });
});
```

</details>

---

## 📚 Level 12: Mini Projects

### Project 12.1: Todo List with Templates
```html
<!-- HTML -->
<template id="todoTemplate">
  <li class="todo-item" data-id="?">
    <input type="checkbox" class="complete">
    <span class="todo-text"></span>
    <button class="delete-btn">Delete</button>
  </li>
</template>

<div class="container">
  <h1>My Todo List</h1>
  <input id="todoInput" type="text" placeholder="Add a new todo">
  <button id="addBtn">Add Todo</button>
  <ul id="todoList"></ul>
</div>
```

**Goal:** Build a complete todo list application

**Hint:** Combine template cloning, event handling, and dynamic data management

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("todoTemplate");
const input = document.getElementById("todoInput");
const addBtn = document.getElementById("addBtn");
const list = document.getElementById("todoList");
let todoId = 1;

function addTodo(text) {
  const clone = template.content.cloneNode(true);
  const item = clone.querySelector(".todo-item");
  
  item.dataset.id = todoId;
  clone.querySelector(".todo-text").textContent = text;
  
  clone.querySelector(".delete-btn").addEventListener("click", function() {
    item.remove();
  });
  
  clone.querySelector(".complete").addEventListener("change", function() {
    clone.querySelector(".todo-text").style.textDecoration = this.checked ? "line-through" : "none";
  });
  
  list.appendChild(clone);
  todoId++;
}

addBtn.addEventListener("click", function() {
  if (input.value.trim()) {
    addTodo(input.value);
    input.value = "";
  }
});

input.addEventListener("keypress", function(e) {
  if (e.key === "Enter") addBtn.click();
});
```

</details>

---

### Project 12.2: Card Layout Generator
```html
<!-- HTML -->
<template id="cardTemplate">
  <div class="card">
    <img class="card-image" src="" alt="">
    <div class="card-content">
      <h3 class="card-title"></h3>
      <p class="card-description"></p>
      <span class="card-price"></span>
    </div>
  </div>
</template>

<div id="cardsContainer" class="cards-grid"></div>
```

**Goal:** Generate product cards from data

**Hint:** Use template cloning to create multiple cards with different data

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("cardTemplate");
const container = document.getElementById("cardsContainer");

const products = [
  { title: "Laptop", description: "High-end laptop", price: "$999", img: "laptop.jpg" },
  { title: "Phone", description: "Latest smartphone", price: "$699", img: "phone.jpg" },
  { title: "Tablet", description: "Portable tablet", price: "$499", img: "tablet.jpg" }
];

products.forEach(product => {
  const clone = template.content.cloneNode(true);
  
  clone.querySelector(".card-image").src = product.img;
  clone.querySelector(".card-image").alt = product.title;
  clone.querySelector(".card-title").textContent = product.title;
  clone.querySelector(".card-description").textContent = product.description;
  clone.querySelector(".card-price").textContent = product.price;
  
  container.appendChild(clone);
});
```

</details>

---

### Project 12.3: Search and Filter
```html
<!-- HTML -->
<template id="itemTemplate">
  <div class="item" data-category="?">
    <h4 class="item-name"></h4>
    <p class="item-description"></p>
  </div>
</template>

<input id="searchInput" type="text" placeholder="Search items...">
<select id="categoryFilter">
  <option value="">All Categories</option>
  <option value="electronics">Electronics</option>
  <option value="books">Books</option>
  <option value="clothing">Clothing</option>
</select>
<div id="itemsContainer"></div>
```

**Goal:** Create a searchable and filterable item list

**Hint:** Combine filtering logic with template rendering

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("itemTemplate");
const container = document.getElementById("itemsContainer");
const searchInput = document.getElementById("searchInput");
const categoryFilter = document.getElementById("categoryFilter");

const items = [
  { name: "Laptop", description: "Gaming laptop", category: "electronics" },
  { name: "JavaScript Guide", description: "Learn JS", category: "books" },
  { name: "T-Shirt", description: "Cotton shirt", category: "clothing" },
  { name: "Phone", description: "Smartphone", category: "electronics" },
  { name: "Novel", description: "Fiction book", category: "books" }
];

function renderItems(itemsToRender) {
  container.innerHTML = "";
  
  itemsToRender.forEach(item => {
    const clone = template.content.cloneNode(true);
    const itemDiv = clone.querySelector(".item");
    
    itemDiv.dataset.category = item.category;
    clone.querySelector(".item-name").textContent = item.name;
    clone.querySelector(".item-description").textContent = item.description;
    
    container.appendChild(clone);
  });
}

function filterItems() {
  const searchTerm = searchInput.value.toLowerCase();
  const category = categoryFilter.value;
  
  const filtered = items.filter(item => {
    const matchesSearch = item.name.toLowerCase().includes(searchTerm) || 
                         item.description.toLowerCase().includes(searchTerm);
    const matchesCategory = !category || item.category === category;
    
    return matchesSearch && matchesCategory;
  });
  
  renderItems(filtered);
}

searchInput.addEventListener("input", filterItems);
categoryFilter.addEventListener("change", filterItems);

// Initial render
renderItems(items);
```

</details>

---

## 🎓 Important Tips for Beginners

1. **Use the Console** - Press F12 or Right Click → Inspect → Console
2. **Read Error Messages** - They tell you what's wrong
3. **Write Small Code Pieces** - Test before moving on
4. **Practice Regularly** - Repetition is key to learning
5. **Build Your Own Projects** - Best way to learn

6. **Choose Your Method:**
   - **Template Literals**: Simple lists, quick rendering
   - **Template Elements**: Reusable components, complex structures

---

## 📁 Additional Files

- **ADVANCED-EXERCISES.md** - Advanced topics (event delegation, DOM traversal, forms, animations, storage, fetch API)
- **MINI-CHALLENGES.md** - Quick coding challenges (coming soon)

---

## 🔗 Useful Resources

- [MDN Web Docs](https://developer.mozilla.org/)
- [JavaScript.info](https://javascript.info/)
- [W3Schools](https://www.w3schools.com/)
- [Template Literals Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
- [HTML Template Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template)
