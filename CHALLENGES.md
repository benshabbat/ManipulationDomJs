# DOM Challenges and Real-World Projects

## 🎯 Project Overview
Practical mini-projects and coding challenges to apply DOM manipulation skills in real-world scenarios.

---

## 🏆 Challenge 1: Build a Todo Application

### Project 1.1: Todo List with Full Features
```html
<!-- HTML -->
<div class="todo-app">
  <h1>My Todo List</h1>
  
  <div class="input-section">
    <input id="todoInput" type="text" placeholder="Add a new task...">
    <button id="addBtn">Add Todo</button>
  </div>
  
  <div class="filters">
    <button class="filter-btn active" data-filter="all">All</button>
    <button class="filter-btn" data-filter="active">Active</button>
    <button class="filter-btn" data-filter="completed">Completed</button>
  </div>
  
  <ul id="todoList" class="todo-list"></ul>
  
  <template id="todoTemplate">
    <li class="todo-item" data-id="">
      <input type="checkbox" class="complete-checkbox">
      <span class="todo-text"></span>
      <button class="delete-btn">×</button>
    </li>
  </template>
</div>

<style>
  .todo-app { max-width: 500px; margin: 20px auto; }
  .todo-item { display: flex; gap: 10px; padding: 10px; border-bottom: 1px solid #eee; align-items: center; }
  .todo-item.completed .todo-text { text-decoration: line-through; color: #999; }
  .delete-btn { background: red; color: white; border: none; cursor: pointer; padding: 5px 10px; }
</style>
```

**Goal:** Build a fully-functional todo app with persistence

**Challenge Requirements:**
- Add new todos
- Mark todos as complete/incomplete
- Delete todos
- Filter by status
- Save to localStorage
- Load from localStorage on page load

**Hint:** Use template cloning, event delegation, localStorage, and classList manipulation

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
class TodoApp {
  constructor() {
    this.todos = JSON.parse(localStorage.getItem("todos")) || [];
    this.currentFilter = "all";
    this.nextId = this.todos.length > 0 ? Math.max(...this.todos.map(t => t.id)) + 1 : 1;
    
    this.init();
  }
  
  init() {
    this.cacheElements();
    this.attachListeners();
    this.render();
  }
  
  cacheElements() {
    this.input = document.getElementById("todoInput");
    this.addBtn = document.getElementById("addBtn");
    this.list = document.getElementById("todoList");
    this.template = document.getElementById("todoTemplate");
    this.filterBtns = document.querySelectorAll(".filter-btn");
  }
  
  attachListeners() {
    this.addBtn.addEventListener("click", () => this.addTodo());
    this.input.addEventListener("keypress", (e) => {
      if (e.key === "Enter") this.addTodo();
    });
    
    this.list.addEventListener("click", (e) => {
      const li = e.target.closest(".todo-item");
      const id = parseInt(li.dataset.id);
      
      if (e.target.classList.contains("complete-checkbox")) {
        this.toggleTodo(id);
      } else if (e.target.classList.contains("delete-btn")) {
        this.deleteTodo(id);
      }
    });
    
    this.filterBtns.forEach(btn => {
      btn.addEventListener("click", (e) => {
        this.filterBtns.forEach(b => b.classList.remove("active"));
        e.target.classList.add("active");
        this.currentFilter = e.target.dataset.filter;
        this.render();
      });
    });
  }
  
  addTodo() {
    const text = this.input.value.trim();
    if (!text) return;
    
    const todo = {
      id: this.nextId++,
      text,
      completed: false
    };
    
    this.todos.push(todo);
    this.save();
    this.render();
    this.input.value = "";
  }
  
  deleteTodo(id) {
    this.todos = this.todos.filter(t => t.id !== id);
    this.save();
    this.render();
  }
  
  toggleTodo(id) {
    const todo = this.todos.find(t => t.id === id);
    if (todo) {
      todo.completed = !todo.completed;
      this.save();
      this.render();
    }
  }
  
  getFilteredTodos() {
    if (this.currentFilter === "all") return this.todos;
    if (this.currentFilter === "active") return this.todos.filter(t => !t.completed);
    if (this.currentFilter === "completed") return this.todos.filter(t => t.completed);
  }
  
  render() {
    this.list.innerHTML = "";
    const filtered = this.getFilteredTodos();
    
    filtered.forEach(todo => {
      const clone = this.template.content.cloneNode(true);
      const li = clone.querySelector(".todo-item");
      
      li.dataset.id = todo.id;
      clone.querySelector(".complete-checkbox").checked = todo.completed;
      clone.querySelector(".todo-text").textContent = todo.text;
      
      if (todo.completed) {
        li.classList.add("completed");
      }
      
      this.list.appendChild(clone);
    });
  }
  
  save() {
    localStorage.setItem("todos", JSON.stringify(this.todos));
  }
}

// Initialize app
new TodoApp();
```

</details>

---

## 🏆 Challenge 2: Build a Shopping Cart

### Project 2.1: E-commerce Shopping Cart
```html
<!-- HTML -->
<div class="shop">
  <h1>Product Store</h1>
  
  <div class="products" id="products"></div>
  
  <div class="cart-sidebar">
    <h2>Shopping Cart</h2>
    <div id="cartItems"></div>
    <div class="cart-total">Total: $<span id="total">0</span></div>
    <button id="checkout">Checkout</button>
  </div>
</div>

<template id="productTemplate">
  <div class="product">
    <div class="product-image"></div>
    <h3 class="product-name"></h3>
    <p class="product-price"></p>
    <button class="add-to-cart">Add to Cart</button>
  </div>
</template>

<template id="cartItemTemplate">
  <div class="cart-item" data-product-id="">
    <span class="item-name"></span>
    <span class="item-price"></span>
    <div class="quantity-controls">
      <button class="qty-minus">−</button>
      <span class="quantity">1</span>
      <button class="qty-plus">+</button>
    </div>
    <button class="remove-item">Remove</button>
  </div>
</template>

<style>
  .shop { display: flex; gap: 20px; padding: 20px; }
  .products { display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; flex: 1; }
  .product { border: 1px solid #ddd; padding: 10px; text-align: center; }
  .product-image { width: 100%; height: 200px; background: #f0f0f0; margin-bottom: 10px; }
  .add-to-cart { background: blue; color: white; border: none; cursor: pointer; padding: 8px 16px; }
  .cart-sidebar { width: 250px; border-left: 1px solid #ddd; padding-left: 20px; }
  .cart-item { display: flex; gap: 10px; padding: 10px; border-bottom: 1px solid #eee; align-items: center; }
  .cart-total { padding: 10px; font-weight: bold; font-size: 18px; }
</style>
```

**Goal:** Create a working shopping cart system

**Challenge Requirements:**
- Display products
- Add/remove from cart
- Update quantities
- Calculate total
- Save cart to localStorage
- Handle checkout

**Hint:** Use event delegation for product and cart operations, maintain cart state, recalculate total on changes

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
class ShoppingCart {
  constructor() {
    this.products = [
      { id: 1, name: "Laptop", price: 999 },
      { id: 2, name: "Mouse", price: 29 },
      { id: 3, name: "Keyboard", price: 79 },
      { id: 4, name: "Monitor", price: 299 },
      { id: 5, name: "Headphones", price: 149 },
      { id: 6, name: "Webcam", price: 79 }
    ];
    
    this.cart = JSON.parse(localStorage.getItem("cart")) || [];
    this.init();
  }
  
  init() {
    this.renderProducts();
    this.renderCart();
    this.attachListeners();
  }
  
  renderProducts() {
    const container = document.getElementById("products");
    const template = document.getElementById("productTemplate");
    container.innerHTML = "";
    
    this.products.forEach(product => {
      const clone = template.content.cloneNode(true);
      const el = clone.querySelector(".product");
      
      el.dataset.productId = product.id;
      clone.querySelector(".product-name").textContent = product.name;
      clone.querySelector(".product-price").textContent = `$${product.price}`;
      
      container.appendChild(clone);
    });
  }
  
  renderCart() {
    const container = document.getElementById("cartItems");
    const template = document.getElementById("cartItemTemplate");
    container.innerHTML = "";
    
    let total = 0;
    
    this.cart.forEach(item => {
      const clone = template.content.cloneNode(true);
      const el = clone.querySelector(".cart-item");
      
      el.dataset.productId = item.id;
      clone.querySelector(".item-name").textContent = item.name;
      clone.querySelector(".item-price").textContent = `$${item.price}`;
      clone.querySelector(".quantity").textContent = item.quantity;
      
      container.appendChild(clone);
      total += item.price * item.quantity;
    });
    
    document.getElementById("total").textContent = total.toFixed(2);
  }
  
  attachListeners() {
    document.getElementById("products").addEventListener("click", (e) => {
      if (e.target.classList.contains("add-to-cart")) {
        const productId = parseInt(e.target.closest(".product").dataset.productId);
        this.addToCart(productId);
      }
    });
    
    document.getElementById("cartItems").addEventListener("click", (e) => {
      const item = e.target.closest(".cart-item");
      const productId = parseInt(item.dataset.productId);
      
      if (e.target.classList.contains("qty-plus")) {
        this.updateQuantity(productId, 1);
      } else if (e.target.classList.contains("qty-minus")) {
        this.updateQuantity(productId, -1);
      } else if (e.target.classList.contains("remove-item")) {
        this.removeFromCart(productId);
      }
    });
    
    document.getElementById("checkout").addEventListener("click", () => {
      alert("Order placed! Total: $" + this.getTotal());
      this.cart = [];
      this.save();
      this.renderCart();
    });
  }
  
  addToCart(productId) {
    const product = this.products.find(p => p.id === productId);
    const cartItem = this.cart.find(c => c.id === productId);
    
    if (cartItem) {
      cartItem.quantity++;
    } else {
      this.cart.push({
        id: product.id,
        name: product.name,
        price: product.price,
        quantity: 1
      });
    }
    
    this.save();
    this.renderCart();
  }
  
  removeFromCart(productId) {
    this.cart = this.cart.filter(c => c.id !== productId);
    this.save();
    this.renderCart();
  }
  
  updateQuantity(productId, delta) {
    const item = this.cart.find(c => c.id === productId);
    if (item) {
      item.quantity += delta;
      if (item.quantity <= 0) {
        this.removeFromCart(productId);
      } else {
        this.save();
        this.renderCart();
      }
    }
  }
  
  getTotal() {
    return this.cart.reduce((sum, item) => sum + (item.price * item.quantity), 0).toFixed(2);
  }
  
  save() {
    localStorage.setItem("cart", JSON.stringify(this.cart));
  }
}

new ShoppingCart();
```

</details>

---

## 🏆 Challenge 3: Build a Notes Application

### Project 3.1: Note-taking App with Categories
```html
<!-- HTML -->
<div class="notes-app">
  <h1>My Notes</h1>
  
  <div class="input-section">
    <input id="categoryInput" type="text" placeholder="Category">
    <input id="noteInput" type="text" placeholder="Note title">
    <textarea id="noteContent" placeholder="Note content"></textarea>
    <button id="addNoteBtn">Add Note</button>
  </div>
  
  <div class="notes-container">
    <div class="categories">
      <h3>Categories</h3>
      <ul id="categoryList"></ul>
    </div>
    
    <div class="notes">
      <div id="notesList"></div>
    </div>
  </div>
</div>

<template id="noteTemplate">
  <div class="note-card" data-id="">
    <div class="note-header">
      <h4 class="note-title"></h4>
      <button class="delete-note">×</button>
    </div>
    <p class="note-content"></p>
    <small class="note-category"></small>
  </div>
</template>

<style>
  .notes-app { max-width: 900px; margin: 20px auto; }
  .input-section { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }
  .input-section input, .input-section textarea { padding: 10px; border: 1px solid #ddd; }
  .notes-container { display: grid; grid-template-columns: 200px 1fr; gap: 20px; }
  .note-card { border: 1px solid #ddd; padding: 15px; margin-bottom: 10px; background: #f9f9f9; }
  .note-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
  .note-category { color: #999; font-size: 12px; }
  .delete-note { background: red; color: white; border: none; cursor: pointer; }
  #categoryList { list-style: none; padding: 0; }
  #categoryList li { padding: 8px; cursor: pointer; background: #f0f0f0; margin-bottom: 5px; }
  #categoryList li.active { background: #007bff; color: white; }
</style>
```

**Goal:** Create a notes app with categories and filtering

**Challenge Requirements:**
- Add notes with title, content, and category
- Display notes in cards
- Filter by category
- Delete notes
- Save to localStorage
- Display category list

**Hint:** Use templates, event delegation, localStorage, and dynamic filtering

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
class NotesApp {
  constructor() {
    this.notes = JSON.parse(localStorage.getItem("notes")) || [];
    this.selectedCategory = null;
    this.nextId = this.notes.length > 0 ? Math.max(...this.notes.map(n => n.id)) + 1 : 1;
    
    this.init();
  }
  
  init() {
    this.cacheElements();
    this.attachListeners();
    this.render();
  }
  
  cacheElements() {
    this.categoryInput = document.getElementById("categoryInput");
    this.noteInput = document.getElementById("noteInput");
    this.noteContent = document.getElementById("noteContent");
    this.addNoteBtn = document.getElementById("addNoteBtn");
    this.notesList = document.getElementById("notesList");
    this.categoryList = document.getElementById("categoryList");
    this.template = document.getElementById("noteTemplate");
  }
  
  attachListeners() {
    this.addNoteBtn.addEventListener("click", () => this.addNote());
    this.noteInput.addEventListener("keypress", (e) => {
      if (e.key === "Enter") this.addNote();
    });
    
    this.notesList.addEventListener("click", (e) => {
      if (e.target.classList.contains("delete-note")) {
        const noteId = parseInt(e.target.closest(".note-card").dataset.id);
        this.deleteNote(noteId);
      }
    });
    
    this.categoryList.addEventListener("click", (e) => {
      document.querySelectorAll("#categoryList li").forEach(li => li.classList.remove("active"));
      if (e.target.tagName === "LI") {
        e.target.classList.add("active");
        this.selectedCategory = e.target.textContent;
        this.renderNotes();
      }
    });
  }
  
  addNote() {
    const category = this.categoryInput.value.trim();
    const title = this.noteInput.value.trim();
    const content = this.noteContent.value.trim();
    
    if (!category || !title || !content) {
      alert("All fields required!");
      return;
    }
    
    this.notes.push({
      id: this.nextId++,
      category,
      title,
      content,
      date: new Date().toLocaleDateString()
    });
    
    this.save();
    this.categoryInput.value = "";
    this.noteInput.value = "";
    this.noteContent.value = "";
    this.render();
  }
  
  deleteNote(id) {
    this.notes = this.notes.filter(n => n.id !== id);
    this.save();
    this.render();
  }
  
  render() {
    this.renderCategories();
    this.renderNotes();
  }
  
  renderCategories() {
    const categories = [...new Set(this.notes.map(n => n.category))];
    this.categoryList.innerHTML = categories.map(cat =>
      `<li class="${cat === this.selectedCategory ? 'active' : ''}">${cat}</li>`
    ).join("");
  }
  
  renderNotes() {
    this.notesList.innerHTML = "";
    let filtered = this.notes;
    
    if (this.selectedCategory) {
      filtered = this.notes.filter(n => n.category === this.selectedCategory);
    }
    
    filtered.forEach(note => {
      const clone = this.template.content.cloneNode(true);
      const card = clone.querySelector(".note-card");
      
      card.dataset.id = note.id;
      clone.querySelector(".note-title").textContent = note.title;
      clone.querySelector(".note-content").textContent = note.content;
      clone.querySelector(".note-category").textContent = `${note.category} • ${note.date}`;
      
      this.notesList.appendChild(clone);
    });
  }
  
  save() {
    localStorage.setItem("notes", JSON.stringify(this.notes));
  }
}

new NotesApp();
```

</details>

---

## 🏆 Challenge 4: Build a Weather Dashboard

### Project 4.1: Weather App with API Integration
```html
<!-- HTML -->
<div class="weather-app">
  <h1>Weather Dashboard</h1>
  
  <div class="search">
    <input id="cityInput" type="text" placeholder="Enter city name">
    <button id="searchBtn">Search</button>
  </div>
  
  <div id="weatherContainer"></div>
</div>

<template id="weatherTemplate">
  <div class="weather-card">
    <h2 class="city-name"></h2>
    <div class="current-weather">
      <div class="temp"></div>
      <div class="description"></div>
    </div>
    <div class="weather-details">
      <div>Humidity: <span class="humidity"></span>%</div>
      <div>Wind: <span class="wind"></span> m/s</div>
      <div>Pressure: <span class="pressure"></span> mb</div>
    </div>
  </div>
</template>

<style>
  .weather-app { max-width: 600px; margin: 20px auto; }
  .search { display: flex; gap: 10px; margin-bottom: 20px; }
  .search input { flex: 1; padding: 10px; border: 1px solid #ddd; }
  .search button { padding: 10px 20px; background: blue; color: white; border: none; cursor: pointer; }
  .weather-card { border: 1px solid #ddd; padding: 20px; background: #f9f9f9; border-radius: 8px; }
  .current-weather { display: flex; gap: 20px; align-items: center; margin: 15px 0; }
  .temp { font-size: 48px; font-weight: bold; }
  .weather-details { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px; margin-top: 15px; }
</style>
```

**Goal:** Build a weather app using OpenWeather API

**Challenge Requirements:**
- Search for cities
- Display current weather
- Show temperature, humidity, wind, pressure
- Handle API errors
- Cache searches (optional)

**Hint:** Use fetch API, template cloning, and async/await

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
class WeatherApp {
  constructor() {
    this.apiKey = "d6d27df89b3cc16e6b50a8ae3bedd63c"; // Free API key
    this.init();
  }
  
  init() {
    this.cacheElements();
    this.attachListeners();
  }
  
  cacheElements() {
    this.cityInput = document.getElementById("cityInput");
    this.searchBtn = document.getElementById("searchBtn");
    this.container = document.getElementById("weatherContainer");
    this.template = document.getElementById("weatherTemplate");
  }
  
  attachListeners() {
    this.searchBtn.addEventListener("click", () => this.search());
    this.cityInput.addEventListener("keypress", (e) => {
      if (e.key === "Enter") this.search();
    });
  }
  
  async search() {
    const city = this.cityInput.value.trim();
    if (!city) return;
    
    try {
      this.container.innerHTML = "Loading...";
      
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${this.apiKey}&units=metric`
      );
      
      if (!response.ok) {
        throw new Error("City not found");
      }
      
      const data = await response.json();
      this.displayWeather(data);
      this.cityInput.value = "";
    } catch (error) {
      this.container.innerHTML = `<p style="color: red;">Error: ${error.message}</p>`;
    }
  }
  
  displayWeather(data) {
    const clone = this.template.content.cloneNode(true);
    
    clone.querySelector(".city-name").textContent = `${data.name}, ${data.sys.country}`;
    clone.querySelector(".temp").textContent = Math.round(data.main.temp) + "°C";
    clone.querySelector(".description").textContent = data.weather[0].main;
    clone.querySelector(".humidity").textContent = data.main.humidity;
    clone.querySelector(".wind").textContent = data.wind.speed;
    clone.querySelector(".pressure").textContent = data.main.pressure;
    
    this.container.innerHTML = "";
    this.container.appendChild(clone);
  }
}

new WeatherApp();
```

</details>

---

## 💡 Project Ideas for Practice

1. **Expense Tracker** - Add expenses, categorize, calculate totals
2. **Recipe App** - Display recipes, filter by ingredients
3. **Movie Database** - Search movies, display details from API
4. **Task Management Board** - Kanban-style board with drag-and-drop
5. **Quote Generator** - Display random quotes, save favorites
6. **Pomodoro Timer** - Timer with notifications
7. **Calculator** - Build a working calculator
8. **Flashcard App** - Create and study flashcards
9. **Blog Platform** - Create, edit, delete posts
10. **Photo Gallery** - Display and filter images

---

## 🎓 Common Pitfalls to Avoid

### 1. Not Managing State Properly
```javascript
// BAD: State in DOM
let count = document.getElementById("count").textContent;

// GOOD: State in JavaScript
let count = 0;
```

### 2. Forgetting to Save
```javascript
// BAD: Data lost on refresh
this.data.push(newItem);

// GOOD: Save to storage
this.data.push(newItem);
localStorage.setItem("data", JSON.stringify(this.data));
```

### 3. Memory Leaks
```javascript
// BAD: Never removing listeners
for (let i = 0; i < 1000; i++) {
  const btn = document.createElement("button");
  btn.addEventListener("click", someHandler);
  // What if we delete the button later?
}

// GOOD: Cleanup or use delegation
container.addEventListener("click", (e) => {
  if (e.target.tagName === "BUTTON") {
    someHandler(e);
  }
});
```

### 4. Performance Issues
```javascript
// BAD: Many DOM reflows
for (let i = 0; i < 1000; i++) {
  const div = document.createElement("div");
  document.body.appendChild(div);
}

// GOOD: Single reflow
const fragment = document.createDocumentFragment();
for (let i = 0; i < 1000; i++) {
  const div = document.createElement("div");
  fragment.appendChild(div);
}
document.body.appendChild(fragment);
```

---

## 🔗 Useful APIs for Projects

- **OpenWeatherMap API** - Weather data
- **JSONPlaceholder API** - Fake REST API for testing
- **REST Countries API** - Country information
- **Quote API** - Random quotes
- **TMDb API** - Movie and TV data
- **GitHub API** - Repository information
- **Unsplash API** - Free high-quality images
- **CoinGecko API** - Cryptocurrency data

---

## 📚 Learning Resources

- [MDN DOM Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
- [JavaScript.info](https://javascript.info/)
- [Web.dev](https://web.dev/)
- [Eloquent JavaScript](https://eloquentjavascript.net/)
- [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)

Happy Coding! 🚀
