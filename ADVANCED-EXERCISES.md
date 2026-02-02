# Advanced DOM Manipulation Exercises

## 📚 Level 13: Event Delegation

### Exercise 13.1: Click any list item
```html
<!-- HTML -->
<ul id="itemList">
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
  <li>Item 4</li>
</ul>
<p id="message"></p>
```

**Task:** When you click any list item, display which item was clicked without adding event listeners to each item individually.

**Hint:** Use a single event listener on the parent element and check `event.target`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const list = document.getElementById("itemList");
const message = document.getElementById("message");

list.addEventListener("click", function(event) {
  if (event.target.tagName === "LI") {
    message.textContent = `You clicked: ${event.target.textContent}`;
  }
});
```

</details>

---

### Exercise 13.2: Dynamic list with delegation
```html
<!-- HTML -->
<input id="input" type="text" placeholder="Enter item">
<button id="addBtn">Add</button>
<ul id="list"></ul>
<p id="selected"></p>
```

**Task:** Create a list where users can add items. Use event delegation to handle clicks on items that don't exist yet. Display the clicked item.

**Hint:** Add the event listener to the `<ul>` once, and it will work for items added later.

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const input = document.getElementById("input");
const addBtn = document.getElementById("addBtn");
const list = document.getElementById("list");
const selected = document.getElementById("selected");

addBtn.addEventListener("click", function() {
  if (input.value.trim()) {
    const li = document.createElement("li");
    li.textContent = input.value;
    list.appendChild(li);
    input.value = "";
  }
});

// Event delegation
list.addEventListener("click", function(event) {
  if (event.target.tagName === "LI") {
    selected.textContent = `Selected: ${event.target.textContent}`;
  }
});
```

</details>

---

## 📚 Level 14: DOM Traversal

### Exercise 14.1: Navigate parent and children
```html
<!-- HTML -->
<div class="container">
  <h1>Title</h1>
  <p id="paragraph">This is a paragraph</p>
  <button id="btn">Check</button>
</div>
```

**Task:** From the button, find the paragraph and change its color to blue using parent/sibling navigation.

**Hint:** Use `parentElement`, `previousElementSibling`, `nextElementSibling`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("btn");

btn.addEventListener("click", function() {
  // Navigate to parent container
  const container = btn.parentElement;
  
  // Find paragraph sibling
  const paragraph = document.getElementById("paragraph");
  paragraph.style.color = "blue";
  
  // Or using navigation:
  const para = btn.parentElement.querySelector("#paragraph");
  para.style.color = "blue";
});
```

</details>

---

### Exercise 14.2: Find all children of a parent
```html
<!-- HTML -->
<div id="parent">
  <span>Item 1</span>
  <span>Item 2</span>
  <span>Item 3</span>
</div>
```

**Task:** Get all the `<span>` elements inside the parent and add a yellow background to all of them.

**Hint:** Use `children` or `querySelectorAll()`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const parent = document.getElementById("parent");

// Method 1: Using children
const children = parent.children;
for (let i = 0; i < children.length; i++) {
  children[i].style.backgroundColor = "yellow";
}

// Method 2: Using querySelectorAll
const spans = parent.querySelectorAll("span");
spans.forEach(span => {
  span.style.backgroundColor = "yellow";
});
```

</details>

---

## 📚 Level 15: Form Handling

### Exercise 15.1: Validate form input
```html
<!-- HTML -->
<form id="form">
  <input id="email" type="email" placeholder="Email">
  <input id="password" type="password" placeholder="Password">
  <button type="submit">Submit</button>
</form>
<p id="error"></p>
```

**Task:** Prevent form submission if email is empty or password is less than 5 characters.

**Hint:** Listen to the `submit` event and use `event.preventDefault()`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const form = document.getElementById("form");
const email = document.getElementById("email");
const password = document.getElementById("password");
const error = document.getElementById("error");

form.addEventListener("submit", function(event) {
  event.preventDefault();
  
  if (email.value === "") {
    error.textContent = "Email is required!";
    return;
  }
  
  if (password.value.length < 5) {
    error.textContent = "Password must be at least 5 characters!";
    return;
  }
  
  error.textContent = "";
  console.log("Form submitted successfully!");
});
```

</details>

---

### Exercise 15.2: Get form values and create object
```html
<!-- HTML -->
<form id="form">
  <input id="name" type="text" placeholder="Name">
  <input id="age" type="number" placeholder="Age">
  <select id="country">
    <option value="">Select country</option>
    <option value="usa">USA</option>
    <option value="uk">UK</option>
    <option value="ca">Canada</option>
  </select>
  <button type="submit">Save</button>
</form>
<div id="output"></div>
```

**Task:** When form is submitted, create an object with the form values and display it.

**Hint:** Get values from inputs and select, create an object, then display it using JSON.stringify()

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const form = document.getElementById("form");
const output = document.getElementById("output");

form.addEventListener("submit", function(event) {
  event.preventDefault();
  
  const user = {
    name: document.getElementById("name").value,
    age: Number(document.getElementById("age").value),
    country: document.getElementById("country").value
  };
  
  output.innerHTML = `<pre>${JSON.stringify(user, null, 2)}</pre>`;
});
```

</details>

---

### Exercise 15.3: Form with checkboxes and radio buttons
```html
<!-- HTML -->
<form id="form">
  <fieldset>
    <legend>Select your interests:</legend>
    <label><input type="checkbox" name="interest" value="sports"> Sports</label>
    <label><input type="checkbox" name="interest" value="music"> Music</label>
    <label><input type="checkbox" name="interest" value="reading"> Reading</label>
  </fieldset>
  
  <fieldset>
    <legend>Preferred contact method:</legend>
    <label><input type="radio" name="contact" value="email"> Email</label>
    <label><input type="radio" name="contact" value="phone"> Phone</label>
    <label><input type="radio" name="contact" value="sms"> SMS</label>
  </fieldset>
  
  <button type="submit">Submit</button>
</form>
<div id="result"></div>
```

**Task:** Get all selected checkboxes and the selected radio button, then display them in an object.

**Hint:** Use `querySelectorAll()` to get all checkboxes with `name="interest"`, then filter for checked ones. For radio buttons, find the one with `checked` property.

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const form = document.getElementById("form");
const result = document.getElementById("result");

form.addEventListener("submit", function(event) {
  event.preventDefault();
  
  // Get all checked checkboxes
  const interests = Array.from(
    document.querySelectorAll('input[name="interest"]:checked')
  ).map(cb => cb.value);
  
  // Get selected radio button
  const contact = document.querySelector('input[name="contact"]:checked')?.value;
  
  const formData = {
    interests: interests,
    contact: contact
  };
  
  result.innerHTML = `<pre>${JSON.stringify(formData, null, 2)}</pre>`;
});
```

</details>

---

### Exercise 15.4: Real-time form validation with error messages
```html
<!-- HTML -->
<style>
  .error-input { border: 2px solid red; }
  .error-message { color: red; font-size: 12px; margin-top: 5px; }
  .input-group { margin-bottom: 15px; }
</style>

<form id="form">
  <div class="input-group">
    <input id="username" type="text" placeholder="Username (3+ characters)" name="username">
    <div class="error-message" id="usernameError"></div>
  </div>
  
  <div class="input-group">
    <input id="email" type="email" placeholder="Valid email" name="email">
    <div class="error-message" id="emailError"></div>
  </div>
  
  <div class="input-group">
    <input id="password" type="password" placeholder="Password (6+ characters)" name="password">
    <div class="error-message" id="passwordError"></div>
  </div>
  
  <button type="submit" id="submitBtn" disabled>Submit</button>
</form>
```

**Task:** Validate each field in real-time as the user types. Show error messages and disable the submit button until all fields are valid.

**Hint:** Add event listeners to each input for the `input` event. Validate the username (min 3 chars), email format, and password (min 6 chars).

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const form = document.getElementById("form");
const usernameInput = document.getElementById("username");
const emailInput = document.getElementById("email");
const passwordInput = document.getElementById("password");
const submitBtn = document.getElementById("submitBtn");

function validateUsername() {
  const username = usernameInput.value;
  const errorEl = document.getElementById("usernameError");
  
  if (username.length < 3) {
    usernameInput.classList.add("error-input");
    errorEl.textContent = "Username must be at least 3 characters";
    return false;
  } else {
    usernameInput.classList.remove("error-input");
    errorEl.textContent = "";
    return true;
  }
}

function validateEmail() {
  const email = emailInput.value;
  const errorEl = document.getElementById("emailError");
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  
  if (!emailRegex.test(email)) {
    emailInput.classList.add("error-input");
    errorEl.textContent = "Please enter a valid email";
    return false;
  } else {
    emailInput.classList.remove("error-input");
    errorEl.textContent = "";
    return true;
  }
}

function validatePassword() {
  const password = passwordInput.value;
  const errorEl = document.getElementById("passwordError");
  
  if (password.length < 6) {
    passwordInput.classList.add("error-input");
    errorEl.textContent = "Password must be at least 6 characters";
    return false;
  } else {
    passwordInput.classList.remove("error-input");
    errorEl.textContent = "";
    return true;
  }
}

function updateSubmitButton() {
  const isValid = validateUsername() && validateEmail() && validatePassword();
  submitBtn.disabled = !isValid;
}

usernameInput.addEventListener("input", updateSubmitButton);
emailInput.addEventListener("input", updateSubmitButton);
passwordInput.addEventListener("input", updateSubmitButton);

form.addEventListener("submit", function(event) {
  event.preventDefault();
  console.log("Form is valid! Ready to submit.");
});
```

</details>

---

### Exercise 15.5: Dynamic form with add/remove fields
```html
<!-- HTML -->
<style>
  .field-group { display: flex; gap: 10px; margin-bottom: 10px; align-items: center; }
  .remove-btn { background: red; color: white; padding: 5px 10px; border: none; cursor: pointer; }
</style>

<form id="form">
  <div id="phoneNumbers">
    <div class="field-group">
      <input type="tel" class="phone-input" placeholder="Phone number" required>
      <button type="button" class="remove-btn" style="display: none;">Remove</button>
    </div>
  </div>
  
  <button type="button" id="addBtn">+ Add Phone Number</button>
  <button type="submit">Submit</button>
</form>
<div id="result"></div>
```

**Task:** Allow users to dynamically add and remove phone number fields. When submitted, display all phone numbers.

**Hint:** Clone the first field when "Add" is clicked. Use event delegation or update listeners for remove buttons.

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const form = document.getElementById("form");
const phoneNumbers = document.getElementById("phoneNumbers");
const addBtn = document.getElementById("addBtn");
const result = document.getElementById("result");

addBtn.addEventListener("click", function(event) {
  event.preventDefault();
  
  // Clone the first field group
  const firstField = phoneNumbers.querySelector(".field-group");
  const newField = firstField.cloneNode(true);
  
  // Clear the input value
  newField.querySelector(".phone-input").value = "";
  
  // Show the remove button
  newField.querySelector(".remove-btn").style.display = "inline-block";
  
  // Add remove functionality
  newField.querySelector(".remove-btn").addEventListener("click", function() {
    newField.remove();
  });
  
  phoneNumbers.appendChild(newField);
});

// Add event listener to the first remove button when there's more than one field
phoneNumbers.addEventListener("click", function(event) {
  if (event.target.classList.contains("remove-btn")) {
    event.target.parentElement.remove();
  }
});

form.addEventListener("submit", function(event) {
  event.preventDefault();
  
  const phones = Array.from(
    phoneNumbers.querySelectorAll(".phone-input")
  ).map(input => input.value);
  
  result.innerHTML = `<pre>${JSON.stringify({ phoneNumbers: phones }, null, 2)}</pre>`;
});
```

</details>

---

## 📚 Level 16: Animations and Transitions

### Exercise 16.1: Animate element position
```html
<!-- HTML -->
<style>
  #box {
    width: 50px;
    height: 50px;
    background-color: red;
    position: relative;
    left: 0;
    transition: left 0.5s ease;
  }
</style>
<button id="moveBtn">Move Right</button>
<div id="box"></div>
```

**Task:** Click the button multiple times to move the box 50px to the right each time.

**Hint:** Change the `left` style property by incrementing a value

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("moveBtn");
const box = document.getElementById("box");
let position = 0;

btn.addEventListener("click", function() {
  position += 50;
  box.style.left = position + "px";
});
```

</details>

---

### Exercise 16.2: Fade in and out
```html
<!-- HTML -->
<style>
  #element {
    background-color: blue;
    padding: 20px;
    opacity: 1;
    transition: opacity 0.5s ease;
  }
</style>
<button id="toggleBtn">Toggle</button>
<div id="element">Click the button to fade</div>
```

**Task:** Toggle between fade in and fade out by clicking the button.

**Hint:** Change the `opacity` property between 1 and 0

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("toggleBtn");
const element = document.getElementById("element");
let isVisible = true;

btn.addEventListener("click", function() {
  isVisible = !isVisible;
  element.style.opacity = isVisible ? "1" : "0";
});
```

</details>

---

## 📚 Level 17: Data Storage

### Exercise 17.1: Save and load form data
```html
<!-- HTML -->
<input id="input" type="text" placeholder="Enter text">
<button id="saveBtn">Save</button>
<button id="loadBtn">Load</button>
<p id="display"></p>
```

**Task:** Save text to localStorage and load it when the page refreshes.

**Hint:** Use `localStorage.setItem()` and `localStorage.getItem()`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const input = document.getElementById("input");
const saveBtn = document.getElementById("saveBtn");
const loadBtn = document.getElementById("loadBtn");
const display = document.getElementById("display");

saveBtn.addEventListener("click", function() {
  localStorage.setItem("savedText", input.value);
  display.textContent = "Saved!";
});

loadBtn.addEventListener("click", function() {
  const saved = localStorage.getItem("savedText");
  if (saved) {
    input.value = saved;
    display.textContent = "Loaded!";
  }
});

// Load on page load
window.addEventListener("load", function() {
  const saved = localStorage.getItem("savedText");
  if (saved) {
    input.value = saved;
  }
});
```

</details>

---

### Exercise 17.2: Store list items
```html
<!-- HTML -->
<input id="itemInput" type="text" placeholder="Add item">
<button id="addBtn">Add</button>
<ul id="list"></ul>
<button id="clearBtn">Clear All</button>
```

**Task:** Save items to localStorage whenever they're added or removed. Load them when page refreshes.

**Hint:** Store the array of items in localStorage using JSON.stringify()

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const input = document.getElementById("itemInput");
const addBtn = document.getElementById("addBtn");
const list = document.getElementById("list");
const clearBtn = document.getElementById("clearBtn");

let items = JSON.parse(localStorage.getItem("items")) || [];

function saveItems() {
  localStorage.setItem("items", JSON.stringify(items));
}

function renderItems() {
  list.innerHTML = "";
  items.forEach((item, index) => {
    const li = document.createElement("li");
    li.innerHTML = `
      ${item}
      <button onclick="deleteItem(${index})">Delete</button>
    `;
    list.appendChild(li);
  });
}

addBtn.addEventListener("click", function() {
  if (input.value.trim()) {
    items.push(input.value);
    saveItems();
    renderItems();
    input.value = "";
  }
});

clearBtn.addEventListener("click", function() {
  items = [];
  saveItems();
  renderItems();
});

function deleteItem(index) {
  items.splice(index, 1);
  saveItems();
  renderItems();
}

// Load on page load
renderItems();
```

</details>

---

## 📚 Level 18: Advanced Filtering and Sorting

### Exercise 18.1: Sort items by name
```html
<!-- HTML -->
<button id="sortBtn">Sort A-Z</button>
<ul id="list">
  <li>Zebra</li>
  <li>Apple</li>
  <li>Mango</li>
  <li>Banana</li>
</ul>
```

**Task:** Click the button to sort the list items alphabetically.

**Hint:** Get all list items, convert to array, sort, then re-render

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("sortBtn");
const list = document.getElementById("list");

btn.addEventListener("click", function() {
  const items = Array.from(list.querySelectorAll("li"));
  
  items.sort((a, b) => {
    return a.textContent.localeCompare(b.textContent);
  });
  
  items.forEach(item => {
    list.appendChild(item);
  });
});
```

</details>

---

### Exercise 18.2: Filter by price range
```html
<!-- HTML -->
<input id="minPrice" type="number" placeholder="Min price">
<input id="maxPrice" type="number" placeholder="Max price">
<button id="filterBtn">Filter</button>
<div id="products"></div>
```

**Task:** Filter products by price range based on input values.

**Hint:** Create an array of products, filter by price, then display

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const minPriceInput = document.getElementById("minPrice");
const maxPriceInput = document.getElementById("maxPrice");
const filterBtn = document.getElementById("filterBtn");
const productsDiv = document.getElementById("products");

const products = [
  { name: "Laptop", price: 800 },
  { name: "Phone", price: 500 },
  { name: "Tablet", price: 300 },
  { name: "Monitor", price: 200 }
];

filterBtn.addEventListener("click", function() {
  const minPrice = Number(minPriceInput.value) || 0;
  const maxPrice = Number(maxPriceInput.value) || Infinity;
  
  const filtered = products.filter(p => p.price >= minPrice && p.price <= maxPrice);
  
  productsDiv.innerHTML = filtered.map(p => `
    <div>
      <h3>${p.name}</h3>
      <p>$${p.price}</p>
    </div>
  `).join("");
});
```

</details>

---

## 📚 Level 19: Fetch and API

### Exercise 19.1: Fetch JSON data
```html
<!-- HTML -->
<button id="fetchBtn">Fetch Users</button>
<div id="output"></div>
```

**Task:** Fetch user data from a JSON file and display it.

**Hint:** Use `fetch()` and `.then()` to handle the response

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("fetchBtn");
const output = document.getElementById("output");

btn.addEventListener("click", function() {
  fetch("https://jsonplaceholder.typicode.com/users?_limit=3")
    .then(response => response.json())
    .then(data => {
      output.innerHTML = data.map(user => `
        <div>
          <h3>${user.name}</h3>
          <p>Email: ${user.email}</p>
        </div>
      `).join("");
    })
    .catch(error => console.error("Error:", error));
});
```

</details>

---

### Exercise 19.2: Display loading state
```html
<!-- HTML -->
<button id="fetchBtn">Load Posts</button>
<p id="loading" style="display:none;">Loading...</p>
<div id="posts"></div>
```

**Task:** Show a loading message while fetching data, then hide it when done.

**Hint:** Show the loading element before fetch, hide it after

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("fetchBtn");
const loading = document.getElementById("loading");
const posts = document.getElementById("posts");

btn.addEventListener("click", function() {
  loading.style.display = "block";
  posts.innerHTML = "";
  
  fetch("https://jsonplaceholder.typicode.com/posts?_limit=3")
    .then(response => response.json())
    .then(data => {
      posts.innerHTML = data.map(post => `
        <div style="border: 1px solid #ccc; padding: 10px; margin: 10px 0;">
          <h4>${post.title}</h4>
          <p>${post.body}</p>
        </div>
      `).join("");
    })
    .finally(() => {
      loading.style.display = "none";
    });
});
```

</details>

---

## 📚 Level 20: Search in Real-time

### Exercise 20.1: Live search
```html
<!-- HTML -->
<input id="searchBox" type="text" placeholder="Search for users...">
<ul id="results"></ul>
```

**Task:** As user types, filter and display matching items in real-time.

**Hint:** Use the `input` event and filter the array

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const searchBox = document.getElementById("searchBox");
const results = document.getElementById("results");

const users = [
  "Alice", "Bob", "Charlie", "Diana", "Eve", "Frank", "Grace", "Henry"
];

searchBox.addEventListener("input", function() {
  const query = searchBox.value.toLowerCase();
  
  const filtered = users.filter(user => user.toLowerCase().includes(query));
  
  results.innerHTML = filtered.map(user => `<li>${user}</li>`).join("");
});
```

</details>

---

### Exercise 20.2: Debounced search
```html
<!-- HTML -->
<input id="searchBox" type="text" placeholder="Search...">
<div id="results"></div>
<p id="status"></p>
```

**Task:** Wait 500ms after user stops typing before searching (debounce).

**Hint:** Use `setTimeout` and `clearTimeout`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const searchBox = document.getElementById("searchBox");
const results = document.getElementById("results");
const status = document.getElementById("status");

const items = ["Apple", "Apricot", "Banana", "Blueberry", "Cherry", "Citrus"];
let timeout;

searchBox.addEventListener("input", function() {
  clearTimeout(timeout);
  status.textContent = "Typing...";
  
  timeout = setTimeout(() => {
    const query = searchBox.value.toLowerCase();
    const filtered = items.filter(item => item.toLowerCase().includes(query));
    
    results.innerHTML = filtered.map(item => `<div>${item}</div>`).join("");
    status.textContent = `Found ${filtered.length} results`;
  }, 500);
});
```

</details>

---

## 🎓 Tips for Advanced Exercises

1. **Event Delegation** - Saves memory when handling many elements
2. **DOM Traversal** - More efficient than querySelectorAll sometimes
3. **localStorage** - Great for small data persistence
4. **Fetch API** - Modern way to get data from servers
5. **Debouncing** - Improves performance for frequent events

---

## 🔗 Resources

- [MDN Event Delegation](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_delegation)
- [MDN localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
