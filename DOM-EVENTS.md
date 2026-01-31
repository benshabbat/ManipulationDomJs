# DOM Events Deep Dive - Specialized Exercises

## 🎯 Project Overview
This file contains in-depth exercises focused on event handling and event patterns.

---

## 📚 Level A1: Event Types and Handling

### Exercise A1.1: Mouse Events Deep Dive
```html
<!-- HTML -->
<div id="box" style="width: 200px; height: 200px; background: lightblue; padding: 20px;">
  Hover over me
</div>
<div id="log"></div>
```

**Goal:** Understand different mouse events

**Hint:** Use `mouseover`, `mouseenter`, `mousedown`, `mouseup`, `click`, `dblclick`

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const box = document.getElementById("box");
const log = document.getElementById("log");

const events = [
  "mouseenter",    // First time cursor enters
  "mouseover",     // Every time cursor moves over
  "mousedown",     // Mouse button pressed
  "mouseup",       // Mouse button released
  "click",         // Full click (down + up)
  "dblclick",      // Double click
  "mouseleave",    // Cursor leaves element
  "mouseout",      // Similar to mouseleave
  "mousemove"      // Every pixel movement
];

events.forEach(eventName => {
  box.addEventListener(eventName, function() {
    log.textContent += `${eventName} | `;
    log.scrollLeft = log.scrollWidth; // Auto scroll
  });
});
```

</details>

---

### Exercise A1.2: Keyboard Events
```html
<!-- HTML -->
<input id="input" type="text" placeholder="Type something">
<div id="output"></div>
```

**Goal:** Handle keyboard input events

**Hint:** Use `keydown`, `keyup`, `keypress` (deprecated), and `input` events

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const input = document.getElementById("input");
const output = document.getElementById("output");

// keydown - key pressed
input.addEventListener("keydown", function(e) {
  console.log(`Key pressed: ${e.key}`);
});

// keyup - key released
input.addEventListener("keyup", function(e) {
  console.log(`Key released: ${e.key}`);
});

// input - text changed (best for real-time)
input.addEventListener("input", function(e) {
  output.textContent = `You typed: ${e.target.value}`;
});

// Check for specific keys
input.addEventListener("keydown", function(e) {
  if (e.key === "Enter") {
    console.log("Enter pressed");
  }
  if (e.ctrlKey && e.key === "s") {
    console.log("Ctrl+S pressed");
  }
});
```

</details>

---

## 📚 Level A2: Event Object and Properties

### Exercise A2.1: Understanding event.preventDefault()
```html
<!-- HTML -->
<form id="form">
  <input id="checkbox" type="checkbox">
  <label for="checkbox">I agree</label>
  <button type="submit">Submit</button>
</form>
<div id="message"></div>
```

**Goal:** Control default behavior with preventDefault()

**Hint:** Call `event.preventDefault()` to stop default action

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const form = document.getElementById("form");
const checkbox = document.getElementById("checkbox");
const message = document.getElementById("message");

form.addEventListener("submit", function(e) {
  e.preventDefault(); // Stop form submission
  
  if (!checkbox.checked) {
    message.textContent = "You must agree to continue!";
    message.style.color = "red";
    return;
  }
  
  message.textContent = "Form submitted!";
  message.style.color = "green";
  // form.submit(); // Manually submit if needed
});

// Prevent link default behavior
const link = document.createElement("a");
link.href = "#";
link.addEventListener("click", function(e) {
  e.preventDefault(); // Don't navigate
  console.log("Link clicked but navigation prevented");
});
```

</details>

---

### Exercise A2.2: Event Propagation - Bubbling vs Capturing
```html
<!-- HTML -->
<div id="outer" style="border: 2px solid red; padding: 20px;">
  Outer
  <div id="middle" style="border: 2px solid blue; padding: 20px;">
    Middle
    <div id="inner" style="border: 2px solid green; padding: 20px;">
      Inner
    </div>
  </div>
</div>
<div id="log"></div>
```

**Goal:** Understand event bubbling and capturing phases

**Hint:** Third parameter of addEventListener is useCapture (true = capture, false = bubble)

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const outer = document.getElementById("outer");
const middle = document.getElementById("middle");
const inner = document.getElementById("inner");
const log = document.getElementById("log");

// Bubbling phase (default, bottom-up)
inner.addEventListener("click", function() {
  log.innerHTML += "Inner (bubble) | ";
});
middle.addEventListener("click", function() {
  log.innerHTML += "Middle (bubble) | ";
});
outer.addEventListener("click", function() {
  log.innerHTML += "Outer (bubble) | ";
});

// Capturing phase (top-down)
outer.addEventListener("click", function() {
  log.innerHTML = "Outer (capture) -> " + log.innerHTML;
}, true); // true = capturing phase

// Stop propagation
inner.addEventListener("click", function(e) {
  // e.stopPropagation(); // Would stop bubbling
  // e.stopImmediatePropagation(); // Stop all listeners
});
```

</details>

---

## 📚 Level A3: Custom Events

### Exercise A3.1: Creating Custom Events
```html
<!-- HTML -->
<button id="btn">Trigger Event</button>
<div id="message"></div>
```

**Goal:** Create and dispatch custom events

**Hint:** Use `new CustomEvent()` to create, and `dispatchEvent()` to trigger

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("btn");
const messageDiv = document.getElementById("message");

// Create custom event
btn.addEventListener("click", function() {
  const customEvent = new CustomEvent("myEvent", {
    detail: { message: "Hello from custom event!" }
  });
  
  // Dispatch the event
  document.dispatchEvent(customEvent);
});

// Listen to custom event
document.addEventListener("myEvent", function(e) {
  messageDiv.textContent = e.detail.message;
  messageDiv.style.color = "blue";
});
```

</details>

---

### Exercise A3.2: Custom Events with Data
```html
<!-- HTML -->
<button id="btn">Send Data</button>
<div id="display"></div>
```

**Goal:** Pass complex data in custom events

**Hint:** Use the `detail` property to pass any data

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const btn = document.getElementById("btn");
const display = document.getElementById("display");

const userData = {
  name: "John",
  email: "john@example.com",
  role: "admin",
  timestamp: new Date()
};

btn.addEventListener("click", function() {
  const event = new CustomEvent("userAction", {
    detail: userData,
    bubbles: true,
    cancelable: true
  });
  
  btn.dispatchEvent(event);
});

btn.addEventListener("userAction", function(e) {
  const data = e.detail;
  display.innerHTML = `
    <h3>${data.name}</h3>
    <p>Email: ${data.email}</p>
    <p>Role: ${data.role}</p>
    <p>Time: ${data.timestamp.toLocaleTimeString()}</p>
  `;
});
```

</details>

---

## 📚 Level A4: Touch and Pointer Events

### Exercise A4.1: Touch Events (Mobile)
```html
<!-- HTML -->
<div id="touchArea" style="width: 300px; height: 300px; background: lightcyan; display: flex; align-items: center; justify-content: center; font-size: 24px; user-select: none;">
  Touch me!
</div>
<div id="log"></div>
```

**Goal:** Handle touch events on mobile devices

**Hint:** Use `touchstart`, `touchmove`, `touchend` events

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const touchArea = document.getElementById("touchArea");
const log = document.getElementById("log");

touchArea.addEventListener("touchstart", function(e) {
  const touch = e.touches[0]; // First touch point
  log.innerHTML = `Started at: (${touch.clientX}, ${touch.clientY})<br>`;
  touchArea.style.backgroundColor = "yellow";
});

touchArea.addEventListener("touchmove", function(e) {
  const touch = e.touches[0];
  log.innerHTML += `Moving: (${touch.clientX}, ${touch.clientY})<br>`;
});

touchArea.addEventListener("touchend", function(e) {
  log.innerHTML += "Touch ended!";
  touchArea.style.backgroundColor = "lightcyan";
});

// Multi-touch (multiple fingers)
touchArea.addEventListener("touchstart", function(e) {
  if (e.touches.length > 1) {
    console.log(`${e.touches.length} fingers detected!`);
  }
});
```

</details>

---

### Exercise A4.2: Pointer Events (Modern)
```html
<!-- HTML -->
<div id="pointerArea" style="width: 300px; height: 300px; background: lightpink; display: flex; align-items: center; justify-content: center;">
  Pointer area
</div>
```

**Goal:** Use modern Pointer Events API

**Hint:** `pointerdown`, `pointermove`, `pointerup` work for mouse, touch, and pen

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const pointerArea = document.getElementById("pointerArea");

pointerArea.addEventListener("pointerdown", function(e) {
  console.log(`Pointer ${e.pointerId} down`);
  console.log(`Type: ${e.pointerType}`); // mouse, touch, pen
  pointerArea.setPointerCapture(e.pointerId); // Capture future events
});

pointerArea.addEventListener("pointermove", function(e) {
  pointerArea.style.left = e.clientX + "px";
});

pointerArea.addEventListener("pointerup", function(e) {
  console.log(`Pointer ${e.pointerId} up`);
  pointerArea.releasePointerCapture(e.pointerId);
});

pointerArea.addEventListener("pointerover", function(e) {
  pointerArea.style.backgroundColor = "lightgreen";
});

pointerArea.addEventListener("pointerout", function(e) {
  pointerArea.style.backgroundColor = "lightpink";
});
```

</details>

---

## 📚 Level A5: Focus and Blur Events

### Exercise A5.1: Focus Management
```html
<!-- HTML -->
<input id="field1" type="text" placeholder="Field 1">
<input id="field2" type="text" placeholder="Field 2">
<input id="field3" type="text" placeholder="Field 3">
<div id="status"></div>
```

**Goal:** Track which field has focus

**Hint:** Use `focus` and `blur` events

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const fields = document.querySelectorAll("input");
const status = document.getElementById("status");

fields.forEach(field => {
  field.addEventListener("focus", function() {
    status.textContent = `Focused on: ${this.placeholder}`;
    this.style.borderColor = "blue";
  });
  
  field.addEventListener("blur", function() {
    status.textContent = "No field focused";
    this.style.borderColor = "gray";
  });
});
```

</details>

---

### Exercise A5.2: Focus Event Delegation
```html
<!-- HTML -->
<div id="form">
  <input type="text" placeholder="Name">
  <input type="email" placeholder="Email">
  <input type="password" placeholder="Password">
</div>
<div id="activeField"></div>
```

**Goal:** Use delegation with focus events

**Hint:** The `focus` event doesn't bubble, use `focusin` instead

<details>
<summary>🔍 Click to reveal solution</summary>

```javascript
const form = document.getElementById("form");
const activeField = document.getElementById("activeField");

// focusin bubbles, focus doesn't
form.addEventListener("focusin", function(e) {
  if (e.target.tagName === "INPUT") {
    activeField.textContent = `Editing: ${e.target.placeholder}`;
    e.target.style.backgroundColor = "lightyellow";
  }
});

form.addEventListener("focusout", function(e) {
  if (e.target.tagName === "INPUT") {
    e.target.style.backgroundColor = "white";
  }
});
```

</details>

---

## 🎓 Advanced Event Patterns

### Resource Events
```javascript
// Load event
window.addEventListener("load", function() {
  // Page completely loaded
});

// Unload event
window.addEventListener("beforeunload", function(e) {
  // Before leaving page
  e.preventDefault();
  e.returnValue = "Are you sure?";
});
```

### Scroll Events
```javascript
window.addEventListener("scroll", function() {
  console.log(`Scrolled to: ${window.scrollY}px`);
});
```

### Resize Events
```javascript
window.addEventListener("resize", function() {
  console.log(`Window size: ${window.innerWidth}x${window.innerHeight}`);
});
```

---

## 💡 Key Concepts

| Event Type | Best Use | Bubbles | Cancelable |
|-----------|----------|---------|-----------|
| click | Single clicks | Yes | Yes |
| dblclick | Double clicks | Yes | Yes |
| input | Real-time text input | Yes | No |
| change | Form field changed | Yes | No |
| focus | Element focused | No | No |
| focusin | Focus (bubbles) | Yes | No |
| keydown | Key pressed | Yes | Yes |
| keyup | Key released | Yes | Yes |
| mouseover | Cursor enters | Yes | Yes |
| mouseleave | Cursor leaves | No | Yes |
| touchstart | Touch begins | Yes | Yes |
| pointerdown | Pointer begins | Yes | Yes |

---

## 🔗 References

- [MDN Event Reference](https://developer.mozilla.org/en-US/docs/Web/Events)
- [Keyboard Events](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
- [Touch Events](https://developer.mozilla.org/en-US/docs/Web/API/Touch_events)
- [Pointer Events](https://developer.mozilla.org/en-US/docs/Web/API/PointerEvent)
- [Custom Events](https://developer.mozilla.org/en-US/docs/Web/Events/Creating_and_triggering_events)
