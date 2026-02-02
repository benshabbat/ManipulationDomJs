# Template Patterns and Advanced DOM Techniques

## 🎯 Project Overview
Advanced exercises focusing on template elements, reusable components, and modern DOM patterns.

---

## 📚 Level B1: Template Element Fundamentals

### Exercise B1.1: Template vs innerHTML
```html
<!-- HTML -->
<template id="template">
  <div class="item">
    <h3></h3>
    <p></p>
  </div>
</template>
<div id="output"></div>
```

**Goal:** Compare template elements vs innerHTML

**Hint:** Templates don't render until cloned; innerHTML is simpler but less efficient for reuse

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
// Method 1: innerHTML (simple but less reusable)
const htmlMethod = `
  <div class="item">
    <h3>Title</h3>
    <p>Description</p>
  </div>
`;

// Method 2: Template (reusable, better structure)
const template = document.getElementById("template");
const clone = template.content.cloneNode(true);
clone.querySelector("h3").textContent = "Title";
clone.querySelector("p").textContent = "Description";

document.getElementById("output").appendChild(clone);

// Benefits of templates:
// 1. HTML structure is separated and reusable
// 2. Can clone multiple times efficiently
// 3. Better for complex components
```

</details>

---

### Exercise B1.2: Template with Slots (Data Binding Pattern)
```html
<!-- HTML -->
<template id="userTemplate">
  <div class="user-card">
    <div class="avatar" style="width: 50px; height: 50px; background: #ddd;"></div>
    <div class="info">
      <h4 class="user-name"></h4>
      <p class="user-role"></p>
      <p class="user-email"></p>
    </div>
  </div>
</template>
<div id="users"></div>
```

**Goal:** Create reusable template with multiple data fields

**Hint:** Use class names to target specific data fields for assignment

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("userTemplate");
const usersContainer = document.getElementById("users");

const users = [
  { name: "Alice", role: "Developer", email: "alice@example.com" },
  { name: "Bob", role: "Designer", email: "bob@example.com" },
  { name: "Carol", role: "Manager", email: "carol@example.com" }
];

users.forEach(user => {
  const clone = template.content.cloneNode(true);
  
  // Bind data to template
  clone.querySelector(".user-name").textContent = user.name;
  clone.querySelector(".user-role").textContent = user.role;
  clone.querySelector(".user-email").textContent = user.email;
  
  usersContainer.appendChild(clone);
});
```

</details>

---

## 📚 Level B2: Component Patterns

### Exercise B2.1: Reusable Component Class
```html
<!-- HTML -->
<div id="container"></div>
```

**Goal:** Create a reusable component class

**Hint:** Use ES6 classes to create template-based components

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
class Card {
  constructor(data) {
    this.data = data;
    this.element = this.render();
  }
  
  render() {
    const div = document.createElement("div");
    div.className = "card";
    div.innerHTML = `
      <div class="card-header">
        <h3>${this.data.title}</h3>
      </div>
      <div class="card-body">
        <p>${this.data.description}</p>
      </div>
      <div class="card-footer">
        <button class="btn">Action</button>
      </div>
    `;
    
    // Add event listener
    div.querySelector(".btn").addEventListener("click", () => {
      this.onAction();
    });
    
    return div;
  }
  
  onAction() {
    console.log("Card action triggered:", this.data);
  }
  
  mount(selector) {
    document.querySelector(selector).appendChild(this.element);
  }
}

// Usage
const card1 = new Card({
  title: "Product 1",
  description: "Amazing product"
});

const card2 = new Card({
  title: "Product 2",
  description: "Excellent quality"
});

card1.mount("#container");
card2.mount("#container");
```

</details>

---

### Exercise B2.2: Component with State Management
```html
<!-- HTML -->
<button id="incrementBtn">Increment</button>
<div id="counter">Count: 0</div>
```

**Goal:** Create component with internal state

**Hint:** Use private properties and methods to manage state

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
class Counter {
  constructor(initialValue = 0) {
    this.state = { count: initialValue };
    this.element = document.createElement("div");
    this.render();
  }
  
  setState(newState) {
    this.state = { ...this.state, ...newState };
    this.render();
  }
  
  render() {
    this.element.innerHTML = `
      <div class="counter">
        <p>Count: ${this.state.count}</p>
        <button class="increment-btn">Increment</button>
        <button class="decrement-btn">Decrement</button>
        <button class="reset-btn">Reset</button>
      </div>
    `;
    
    this.attachListeners();
  }
  
  attachListeners() {
    this.element.querySelector(".increment-btn").addEventListener("click", () => {
      this.setState({ count: this.state.count + 1 });
    });
    
    this.element.querySelector(".decrement-btn").addEventListener("click", () => {
      this.setState({ count: this.state.count - 1 });
    });
    
    this.element.querySelector(".reset-btn").addEventListener("click", () => {
      this.setState({ count: 0 });
    });
  }
  
  mount(selector) {
    document.querySelector(selector).appendChild(this.element);
  }
}

// Usage
const counter = new Counter(10);
counter.mount("#counter");
```

</details>

---

## 📚 Level B3: Fragment and DocumentFragment

### Exercise B3.1: DocumentFragment for Batch Operations
```html
<!-- HTML -->
<ul id="list"></ul>
```

**Goal:** Create multiple elements efficiently with DocumentFragment

**Hint:** Add to DocumentFragment first, then add once to DOM (fewer reflows)

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const list = document.getElementById("list");
const items = ["Item 1", "Item 2", "Item 3", "Item 4", "Item 5"];

// INEFFICIENT: Add one by one
// items.forEach(item => {
//   const li = document.createElement("li");
//   li.textContent = item;
//   list.appendChild(li); // Reflow each time!
// });

// EFFICIENT: Use DocumentFragment
const fragment = document.createDocumentFragment();

items.forEach(item => {
  const li = document.createElement("li");
  li.textContent = item;
  fragment.appendChild(li); // No reflow yet
});

list.appendChild(fragment); // Single reflow!
```

</details>

---

### Exercise B3.2: Fragment with Cloning
```html
<!-- HTML -->
<template id="itemTemplate">
  <li class="list-item">
    <span></span>
    <button>Remove</button>
  </li>
</template>
<ul id="list"></ul>
```

**Goal:** Use fragment with template cloning for efficiency

**Hint:** Clone template multiple times into fragment, then append once

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const template = document.getElementById("itemTemplate");
const list = document.getElementById("list");
const items = ["Apple", "Banana", "Orange", "Grape", "Mango"];

const fragment = document.createDocumentFragment();

items.forEach((item, index) => {
  const clone = template.content.cloneNode(true);
  clone.querySelector("span").textContent = item;
  clone.querySelector("button").addEventListener("click", function() {
    this.parentElement.remove();
  });
  fragment.appendChild(clone); // Add to fragment, not DOM
});

list.appendChild(fragment); // Single operation adds all items
```

</details>

---

## 📚 Level B4: Shadow DOM (Web Components)

### Exercise B4.1: Basic Shadow DOM
```html
<!-- HTML -->
<div id="host"></div>
```

**Goal:** Create isolated DOM with Shadow DOM

**Hint:** Use `attachShadow()` to create shadow tree

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const host = document.getElementById("host");

// Attach shadow root
const shadow = host.attachShadow({ mode: "open" });

// Create styles (isolated to shadow DOM)
const style = document.createElement("style");
style.textContent = `
  :host {
    display: inline-block;
    padding: 10px;
    background: lightblue;
    border: 1px solid blue;
  }
  
  h2 {
    color: darkblue;
    margin: 0;
  }
  
  p {
    margin: 5px 0 0 0;
    font-size: 14px;
  }
`;

// Create shadow DOM content
const div = document.createElement("div");
div.innerHTML = `
  <h2>Shadow DOM Component</h2>
  <p>Styles are isolated!</p>
`;

// Add to shadow DOM
shadow.appendChild(style);
shadow.appendChild(div);

// Note: Light DOM is still in the main document
host.innerHTML += "This is in light DOM";
```

</details>

---

### Exercise B4.2: Shadow DOM with Slots
```html
<!-- HTML -->
<div id="component">
  <h3>Original Content</h3>
  <p>This will be slotted</p>
</div>
```

**Goal:** Use slots to allow content projection

**Hint:** Use `<slot>` element to project light DOM content

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const component = document.getElementById("component");
const shadow = component.attachShadow({ mode: "open" });

// Create template with slots
const template = document.createElement("template");
template.innerHTML = `
  <style>
    ::slotted(h3) { color: red; }
    ::slotted(p) { color: blue; }
    .wrapper { border: 2px solid green; padding: 10px; }
  </style>
  <div class="wrapper">
    <h2>Component Title</h2>
    <slot name="content"></slot>
    <p>After slot</p>
  </div>
`;

shadow.appendChild(template.content.cloneNode(true));

// Light DOM content must have slot attribute
// <div id="component">
//   <p slot="content">This goes in the slot</p>
// </div>
```

</details>

---

## 📚 Level B5: Advanced Data Binding

### Exercise B5.1: Simple Reactive Data Binding
```html
<!-- HTML -->
<input id="nameInput" type="text" value="John">
<div id="display">Name: John</div>
```

**Goal:** Create two-way data binding

**Hint:** Use Proxy or Object.defineProperty for reactivity

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const nameInput = document.getElementById("nameInput");
const display = document.getElementById("display");

// Create reactive object
const data = {
  name: "John"
};

// Create proxy for reactivity
const handler = {
  set(target, property, value) {
    target[property] = value;
    updateUI(); // Update UI when data changes
    return true;
  }
};

const reactive = new Proxy(data, handler);

function updateUI() {
  display.textContent = `Name: ${reactive.name}`;
  nameInput.value = reactive.name;
}

nameInput.addEventListener("input", function(e) {
  reactive.name = e.target.value; // Trigger proxy setter
});

// Initial update
updateUI();
```

</details>

---

### Exercise B5.2: Complex Object Data Binding
```html
<!-- HTML -->
<input id="firstNameInput" type="text" value="John">
<input id="lastNameInput" type="text" value="Doe">
<div id="fullNameDisplay">John Doe</div>
```

**Goal:** Bind multiple properties with computed values

**Hint:** Use computed properties or getters with data binding

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const firstNameInput = document.getElementById("firstNameInput");
const lastNameInput = document.getElementById("lastNameInput");
const display = document.getElementById("fullNameDisplay");

const person = {
  firstName: "John",
  lastName: "Doe",
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
};

const handler = {
  set(target, property, value) {
    target[property] = value;
    updateUI();
    return true;
  }
};

const reactive = new Proxy(person, handler);

function updateUI() {
  display.textContent = reactive.fullName;
  firstNameInput.value = reactive.firstName;
  lastNameInput.value = reactive.lastName;
}

firstNameInput.addEventListener("input", (e) => {
  reactive.firstName = e.target.value;
});

lastNameInput.addEventListener("input", (e) => {
  reactive.lastName = e.target.value;
});

updateUI();
```

</details>

---

## 📚 Level B6: Virtual DOM Concepts

### Exercise B6.1: Diff and Patch Pattern
```html
<!-- HTML -->
<div id="app"></div>
```

**Goal:** Implement basic virtual DOM pattern

**Hint:** Keep old structure, compare with new, update only differences

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
class VirtualNode {
  constructor(tag, props, children) {
    this.tag = tag;
    this.props = props;
    this.children = children || [];
  }
}

function h(tag, props, ...children) {
  return new VirtualNode(tag, props, children.flat());
}

function render(vnode) {
  if (typeof vnode === "string") {
    return document.createTextNode(vnode);
  }
  
  const el = document.createElement(vnode.tag);
  
  Object.keys(vnode.props || {}).forEach(key => {
    if (key.startsWith("on")) {
      const eventName = key.slice(2).toLowerCase();
      el.addEventListener(eventName, vnode.props[key]);
    } else {
      el.setAttribute(key, vnode.props[key]);
    }
  });
  
  vnode.children.forEach(child => {
    el.appendChild(render(child));
  });
  
  return el;
}

// Usage
const vdom = h("div", { class: "container" },
  h("h1", {}, "Hello"),
  h("button", { onClick: () => console.log("Clicked!") }, "Click me")
);

document.getElementById("app").appendChild(render(vdom));
```

</details>

---

### Exercise B6.2: Render Function Pattern
```html
<!-- HTML -->
<div id="app"></div>
```

**Goal:** Implement render function that updates efficiently

**Hint:** Use render function to return vnode, compare old and new

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
class App {
  constructor(initialState = {}) {
    this.state = initialState;
    this.vnode = null;
    this.dom = null;
  }
  
  setState(newState) {
    this.state = { ...this.state, ...newState };
    this.update();
  }
  
  render() {
    // Override in subclass
    return `<div></div>`;
  }
  
  update() {
    const newDOM = document.createElement("div");
    newDOM.innerHTML = this.render();
    
    // Simple replacement (real apps use diffing)
    if (this.dom) {
      this.dom.parentNode.replaceChild(newDOM.firstChild, this.dom);
    }
    
    this.dom = newDOM.firstChild;
  }
  
  mount(selector) {
    this.update();
    document.querySelector(selector).appendChild(this.dom);
  }
}

// Usage
class Counter extends App {
  render() {
    return `
      <div style="padding: 20px; border: 1px solid #ccc;">
        <h2>Count: ${this.state.count}</h2>
        <button onclick="counter.setState({count: ${this.state.count + 1}})">
          Increment
        </button>
      </div>
    `;
  }
}

const counter = new Counter({ count: 0 });
counter.mount("#app");
```

</details>

---

## 💡 Performance Tips

### 1. Batch DOM Updates
```javascript
// BAD: Multiple reflows
for (let i = 0; i < 1000; i++) {
  element.style.width = i + "px"; // Reflow each time
}

// GOOD: Single reflow
const fragment = document.createDocumentFragment();
fragment.appendChild(...); // Build in memory
document.body.appendChild(fragment); // Single reflow
```

### 2. Use classList Instead of style
```javascript
// BAD: Direct style manipulation
element.style.color = "red";
element.style.fontSize = "16px";
element.style.padding = "10px";

// GOOD: Use classes
element.classList.add("active");
```

### 3. Cache DOM References
```javascript
// BAD: Querying every time
for (let i = 0; i < 1000; i++) {
  document.querySelector(".item").textContent = i;
}

// GOOD: Cache reference
const item = document.querySelector(".item");
for (let i = 0; i < 1000; i++) {
  item.textContent = i;
}
```

### 4. Delegate Events
```javascript
// BAD: Listener on each item
items.forEach(item => {
  item.addEventListener("click", handler);
});

// GOOD: Single listener on parent
list.addEventListener("click", (e) => {
  if (e.target.matches(".item")) handler(e);
});
```

---

## 🔗 References

- [Template Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template)
- [Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)
- [DocumentFragment](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)
- [Proxy Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
- [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components)
