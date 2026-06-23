# JavaScript Session 03

---

## Template Literals

Template literals are a modern way (ES6+) to write strings that makes it much easier to combine text with variables.

### The Backtick ```

Instead of using single quotes `'` or double quotes `"`, template literals use backticks.

```jsx
let name = `Ahmed`;
```

### The Dollar Sign and Curly Braces `${}`

Inside a backtick string, you can insert any variable or expression directly using `${}`. This is called **string interpolation**.

```jsx
let name = "Ahmed";
let age = 25;

console.log(`My name is${name} and I am${age} years old`);
// Output: My name is Ahmed and I am 25 years old
```

### How They Work Together

The backtick opens a special type of string. Anywhere inside that string, `${}` acts like a “window” where JavaScript switches from text mode to code mode, runs whatever is inside the curly braces, and inserts the result directly into the string.

```jsx
let x = 10;
let y = 20;

console.log(`The sum of${x} and${y} is${x + y}`);
// Output: The sum of 10 and 20 is 30
```

Before template literals, the same thing required messy concatenation:

```jsx
// Old way
console.log("The sum of " + x + " and " + y + " is " + (x + y));

// New way — cleaner and easier to read
console.log(`The sum of${x} and${y} is${x + y}`);
```

Template literals also support multi-line strings directly, without any special characters:

```jsx
let message = `Line one
Line two
Line three`;
```

---

## Storing Data in the Browser

Sometimes we need to save data on the user’s device so it doesn’t get lost when the page reloads or the browser closes. JavaScript gives us a few ways to do this.

### Storage Types

| Storage Type | How long data lasts | Approx. Size Limit | Sent to server? |
| --- | --- | --- | --- |
| Local Storage | Forever, until manually cleared | ~5-10 MB | No |
| Session Storage | Until the browser tab is closed | ~5-10 MB | No |
| Cookies | Set by you (can expire automatically) | ~4 KB | Yes, sent with every request |

### Local Storage

Data saved here stays even if you close the browser completely and reopen it later. It’s the most commonly used option for things like saving user preferences or a shopping cart.

```jsx
// Save data — key and value must be strings
localStorage.setItem("username", "Diaa");

// Get data — returns the string, or null if not found
console.log(localStorage.getItem("username")); // "Diaa"

// Remove specific data
localStorage.removeItem("username");

// Clear everything
localStorage.clear();
```

### Session Storage

Works exactly like Local Storage, but the data is automatically deleted as soon as the browser tab is closed.

```jsx
sessionStorage.setItem("tempData", "Hello");
console.log(sessionStorage.getItem("tempData")); // "Hello"
```

### Cookies

Small pieces of data that are also sent to the server with every request. Mostly used for things like login sessions and tracking.

```jsx
document.cookie = "username=Diaa; expires=Fri, 31 Dec 2026 23:59:59 GMT; path=/";
console.log(document.cookie);
```

---

## Storing Objects and Arrays

`localStorage` and `sessionStorage` can only store **strings**. So if you want to store an array or object, you need to convert it first.

### JSON.stringify()

Converts an array or object into a string.

```jsx
const user = { name: "Diaa", age: 24 };
const userString = JSON.stringify(user);
console.log(userString); // '{"name":"Diaa","age":24}'
```

### JSON.parse()

Converts a string back into an object or array.

```jsx
const userString = '{"name":"Diaa","age":24}';
const user = JSON.parse(userString);
console.log(user.name); // "Diaa"
```

### Putting It All Together

```jsx
const user = { name: "Diaa", age: 24 };

// Store an object
localStorage.setItem("user", JSON.stringify(user));

// Get and display the object
const savedUser = JSON.parse(localStorage.getItem("user"));
console.log(savedUser.name); // "Diaa"
```

---

## String Methods

### string.length

Returns the number of characters in a string.

```jsx
let name = "Diaa";
console.log(name.length); // 4
```

### string.slice(start, end)

Extracts a part of a string and returns it as a new string.

```jsx
let text = "Hello World";
console.log(text.slice(0, 5)); // "Hello"
console.log(text.slice(6));    // "World"
```

### string.indexOf(character)

Returns the index of the first occurrence of the given character/substring.

```jsx
let text = "Hello World";
console.log(text.indexOf("o")); // 4
```

### string.lastIndexOf(character)

Returns the index of the last occurrence of the given character/substring.

```jsx
let text = "Hello World";
console.log(text.lastIndexOf("o")); // 7
```

### string.charAt(index)

Returns the character found at the given index.

```jsx
let text = "Hello";
console.log(text.charAt(1)); // "e"
```

### string.trim()

Removes any whitespace from the beginning and end of a string and returns the cleaned string.

```jsx
let text = "   Hello World   ";
console.log(text.trim()); // "Hello World"
```

### string.includes(searchString, position)

Searches the string and returns `true` or `false`.

```jsx
let text = "Hello World";
console.log(text.includes("World")); // true
console.log(text.includes("xyz"));   // false
```

### string.toLowerCase() / string.toUpperCase()

```jsx
let text = "Hello World";
console.log(text.toLowerCase()); // "hello world"
console.log(text.toUpperCase()); // "HELLO WORLD"
```

### string.replaceAll(oldValue, newValue)

```jsx
let text = "I like cats. Cats are great.";
console.log(text.replaceAll("Cats", "Dogs"));
// "I like cats. Dogs are great."
```

### string.split(separator)

Cuts the string at the given separator and returns an array of the pieces.

```jsx
let text = "apple,banana,orange";
console.log(text.split(","));
// ["apple", "banana", "orange"]
```

## parseInt, parseFloat, and toFixed

These are 3 common methods for converting and formatting numbers, especially useful when working with prices and user input.

### parseInt()

Converts a string into a whole number (integer), and stops reading as soon as it hits a character that isn’t a number. It also drops anything after a decimal point.

```jsx
console.log(parseInt("25px"));  // 25, stops at the first non-number character
console.log(parseInt("25.99")); // 25, drops everything after the decimal point
```

### parseFloat()

Same idea as `parseInt()`, but it keeps the decimal part instead of dropping it.

```jsx
console.log(parseFloat("25.99kg")); // 25.99
```

### toFixed()

Rounds a number to a fixed number of decimal places. The important thing to remember: it returns a **string**, not a number.

```jsx
let price = 49.999;
console.log(price.toFixed(2)); // "50.00", rounded to 2 decimal places, returned as a string
```

### Real Use Case: Displaying a Price

```jsx
let price = 49.9;
console.log("Price: $" + price.toFixed(2)); // "Price: $49.90"
```

Without `toFixed()`, `price` would just print as `49.9`, missing the second `0` that real prices usually need.

### array.join(separator)

The opposite of split — converts an array into a string, joined by the given separator.

```jsx
let fruits = ["apple", "banana", "orange"];
console.log(fruits.join(", "));
// "apple, banana, orange"
```

### Common Search Pattern

A very common real-world pattern when searching user input is to clean it up first, so the search isn’t case-sensitive or affected by extra spaces:

```jsx
let userInput = "  Hello  ";
let searchTerm = "hello";

console.log(userInput.trim().toLowerCase().includes(searchTerm.trim().toLowerCase()));
// true
```

This chains 3 methods together: `trim()` removes extra spaces, `toLowerCase()` makes it lowercase, and `includes()` checks if the search term exists.

---

## Ternary Operator

The ternary operator is a shorthand way of writing a simple `if/else` in a single line.

```jsx
condition ? expressionIfTrue : expressionIfFalse
```

```jsx
let userAge = 25;

userAge > 20 ? console.log("Enter") : console.log("Out");
// "Enter"
```

It can also be stored directly in a variable:

```jsx
let userAge = 16;
let message = userAge >= 18 ? "Adult" : "Minor";
console.log(message); // "Minor"
```

### Combining Ternary with Logical Operators

You can also use `&&` and `||` as lightweight alternatives to conditions in certain cases.

```jsx
let isLoggedIn = true;

isLoggedIn && console.log("Welcome back!");
// Runs console.log only if isLoggedIn is true

let username = "" || "Guest";
console.log(username); // "Guest" — because "" is falsy, falls back to "Guest"
```

### Nested Ternary

You can chain ternary operators, though it’s best to use this only for simple cases to keep code readable.

```jsx
let score = 75;

let grade = score >= 90 ? "A" : score >= 70 ? "B" : "C";
console.log(grade); // "B"
```

---

## Regular Expressions (Regex)

A regular expression is a **pattern** made of symbols and characters used to search, validate, or match text.

Useful tools for practicing regex:
- **regex101.com** — a website to build and test regex patterns
- **ihateregex.io** — a website with ready-made common regex patterns

### Basic Syntax

```jsx
var regex = /pattern/;
```

The `^` means “start of the string” and `$` means “end of the string.”

### Character Sets `[ ]`

Square brackets mean “match any one character from this list” — similar to an OR condition.

```jsx
[a-z]      // any lowercase letter from a to z
[a-zA-Z]   // any letter, lowercase OR uppercase
[0-9]      // any digit from 0 to 9
\w         // shorthand for [a-zA-Z0-9_-@] (word characters)
```

### Quantifiers — Controlling How Many

```jsx
?    // 0 or 1 occurrence
+    // 1 or more occurrences
*    // 0 or more occurrences
{5}  // exactly 5 occurrences
{2,9} // between 2 and 9 occurrences
```

### Special Characters

```jsx
.    // matches ANY character when used alone
\.   // matches an actual literal dot "." character
```

### Examples with Explanation

**Exactly 5 letters or digits:**

```jsx
/^[a-zA-Z0-9]{5}$/
```

Must be exactly 5 characters, only letters and numbers allowed.

**Between 2 and 9 characters:**

```jsx
/^[a-zA-Z0-9]{2,9}$/
```

Must be at least 2 characters and no more than 9.

**Matching specific words with optional space:**

```jsx
/^web?(designer|developer)$/
```

This matches: “webdesigner”, “web designer”, “webdeveloper”, or “web developer” — the `?` after the space makes the space optional, and `(designer|developer)` means either word is accepted.

### Common Real-World Patterns

**Email:**

```jsx
/^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/
```

Checks for: text before `@`, the `@` symbol itself, a domain name, a literal dot, then 2 or more letters for the extension (like `.com`).

**Password (at least 8 characters, 1 letter, 1 number):**

```jsx
/^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/
```

Requires at least one letter, at least one number, and a minimum total length of 8.

**Numbers Only:**

```jsx
/^[0-9]+$/
```

The entire string must contain only digits, one or more.

**Letters Only:**

```jsx
/^[a-zA-Z]+$/
```

The entire string must contain only letters, one or more.

**Egyptian Phone Number:**

```jsx
/^01[0125][0-9]{8}$/
```

Must start with `01`, followed by one of `0`, `1`, `2`, or `5` (covering Vodafone, Etisalat, Orange, WE), then exactly 8 more digits.

### Using Regex in JavaScript

```jsx
let regex = /^[0-9]+$/;
console.log(regex.test("12345")); // true
console.log(regex.test("abc123")); // false
```

`.test()` checks if the string matches the pattern and returns `true` or `false`.

---

## DOM — Document Object Model

The DOM is how JavaScript “sees” an HTML page — as a tree of connected objects called nodes, instead of plain text.

### The DOM Tree

When the browser loads an HTML page, it builds a tree structure where every tag, piece of text, comment, and attribute becomes a connected node, starting from `document` at the top and branching down through every nested element.

```
document
  └── html
       ├── head
       │    └── title
       └── body
            ├── h1
            └── p
```

Because it’s structured like a tree, JavaScript can move up, down, and sideways through the page to find and change anything.

### DOM Nodes

All DOM nodes are objects, and JavaScript represents each node as a JavaScript object you can interact with. There are 5 main types:

| Node Type | What it represents |
| --- | --- |
| Document Node | The entire HTML document (the root) |
| Element Node | An HTML tag, like `<div>` or `<button>` |
| Text Node | The actual text inside an element |
| Attribute Node | An attribute on an element, like `class="btn"` |
| Comment Node | An HTML comment `<!-- like this -->` |

---

## Iterable Objects: HTMLCollection vs NodeList

An **iterable object** is a special type of object that has a `length` property and can be looped over, even though it’s not a true array.

Since 2015 (ES6), some DOM methods return `HTMLCollection` and others return `NodeList` — and they behave differently.

|  | HTMLCollection | NodeList |
| --- | --- | --- |
| Type | Iterable Object | Iterable Object |
| Contains | Only Element Nodes | Any type of Node |
| Live or Static? | Live (auto-updates if DOM changes) | Static (except `childNodes`, which is live) |

“Live” means if you add or remove elements from the page afterward, the collection automatically reflects that change. “Static” means it stays frozen as a snapshot from the moment it was created.

### Converting to a Real Array

Since these aren’t true arrays, you don’t have access to array methods like `.map()` or `.filter()` directly. To convert them into a real array, use:

```jsx
let elements = document.getElementsByClassName("item");
let elementsArray = Array.from(elements);
```

---

## Selecting Elements from the DOM

```jsx
// Returns HTMLCollection (iterable object, live)
document.getElementsByClassName("box");

// Returns HTMLCollection (iterable object, live)
document.getElementsByTagName("div");

// Returns NodeList (iterable object)
document.getElementsByName("username");

// Returns a single element — works just like CSS selectors
document.querySelector(".box");
document.querySelector("#myId");
document.querySelector("button");

// Returns NodeList — use this when you want to select MORE than one element
document.querySelectorAll(".box");
```

### Other Useful document Properties

```jsx
document.title;   // gets/sets the page title
document.body;    // selects the <body> element
document.images;  // returns all <img> elements on the page
document.links;   // returns all <a> elements with an href
```

---

## classList

`classList` lets you add, remove, toggle, or check CSS classes on an element directly through JavaScript.

```jsx
let box = document.querySelector(".box");

box.classList.add("active");        // adds a class
box.classList.remove("active");     // removes a class
box.classList.toggle("active");     // adds it if missing, removes it if present
box.classList.contains("active");   // returns true or false
box.classList.replace("active", "hidden"); // replace exist "active" class with "hidden"
```

This is the standard, modern way to control styling dynamically through JavaScript instead of manually editing `style` properties.

---

## addEventListener

`addEventListener` is how we tell JavaScript “watch this element, and when a specific event happens to it, run this function.”

```jsx
myBtn.addEventListener("click", function () {
  console.log("Button clicked!");
});
```

The first argument is the event you want to listen for, and the second is the function you want to run — written **without** `()` at the end, because we don’t want to call it immediately, we want JavaScript to call it later when the event happens.

### Best Practice: Use an Anonymous Function

```jsx
myBtn.addEventListener("click", function () {
  console.log("Clicked!");
});
```

Writing the function directly inline like this (anonymous function) is the most common and recommended pattern.

### Using `this` Inside addEventListener

Inside a regular function passed to `addEventListener`, the keyword `this` refers to the element the listener was attached to.

```jsx
myBtn.addEventListener("click", function () {
  console.log(this); // refers to myBtn itself
  this.style.backgroundColor = "red";
});
```

This is useful when you want to change something about the exact element that triggered the event, without needing to reference its variable name again.

---

## Common Events

To override the default behavior of any event (like a form refreshing the page on submit), use `e.preventDefault()`.

### Mouse Events

```jsx
addEventListener("contextmenu"); // right-click menu
addEventListener("mouseenter");  // mouse enters the element
addEventListener("mouseleave");  // mouse leaves the element
addEventListener("mousedown");   // mouse button is pressed down
addEventListener("mouseup");     // mouse button is released after being pressed
```

### Focus Events

```jsx
addEventListener("focus"); // element becomes focused (e.g. clicking into an input)
addEventListener("blur");  // element loses focus — opposite of focus
```

### Keyboard and Input Events

```jsx
addEventListener("keydown");  // fires while a key is being pressed down
addEventListener("keyup");    // fires when a key is released
addEventListener("keypress"); // similar to keydown, but only fires for character keys (letters, numbers, symbols, space) — does NOT fire for backspace
addEventListener("input");    // the best option overall — fires on any input change, including backspace
addEventListener("change");   // fires when a change happens inside an input AND the input loses focus
addEventListener("submit");   // fires when a form is submitted
```

---

## Event Info: target vs currentTarget

```jsx
document.addEventListener("click", function (e) {
  console.log(e); // the full event object
});
```

```html
<div>
  <button>Click</button>
</div>
```

```jsx
button.addEventListener("click", function (e) {
  console.log(e.target);        // the element that was actually clicked
  console.log(e.currentTarget); // the element the listener was attached to (button)
});
```

In this example, since the listener is on the button itself, `target` and `currentTarget` are the same. The difference becomes important with **Event Bubbling**, explained next.

---

## Event Bubbling and Event Capturing

### Event Bubbling

When you click an element, the click event doesn’t just fire on that element — it also “bubbles up” and fires on every parent element above it, all the way up to `document`. This is the default browser behavior.

### Event Capturing

The opposite of bubbling. Instead of going from the clicked element upward, capturing goes from the top-level parent downward to the clicked element, before bubbling happens.

### Example

```html
<section class="bg-warning p-5">
  <div class="bg-danger text-center w-50 p-5 m-auto">
    <button class="btn btn-primary">Click Me</button>
  </div>
</section>
```

```jsx
var btns = document.querySelector(".btn");
var div = document.querySelector("div");
var section = document.querySelector("section");

section.addEventListener("click", function (e) {
  console.log("Target ====>", e.target);          // button
  console.log("Current Target ====>", e.currentTarget); // section
});

div.addEventListener("click", function (e) {
  console.log("Target ====>", e.target);          // button
  console.log("Current Target ====>", e.currentTarget); // div
});

btns.addEventListener("click", function (e) {
  console.log("Target ====>", e.target);          // button
  console.log("Current Target ====>", e.currentTarget); // button
});
```

When you click the button, all three listeners fire — button first, then div, then section — because the click event bubbles upward through every parent. In every case, `e.target` stays the same (the actual button you clicked), but `e.currentTarget` changes depending on which element’s listener is currently running.

### Stopping Event Bubbling

If you don’t want the event to bubble up to parent elements, use:

```jsx
btns.addEventListener("click", function (e) {
  e.stopPropagation();
});
```

### Using Event Capturing

To make an event listener fire during the capturing phase (top-down) instead of the bubbling phase (bottom-up), pass `true` as a third argument:

```jsx
section.addEventListener("click", function (e) {
  console.log(e.clientX);
}, true);
```

---

*End of Session 03*