# JavaScript Session 01

---

## Why JavaScript?

JavaScript is the programming language of the web. Without it, websites would be static and unable to respond to users.

Here is why we use JavaScript:

**1. Detect User Events**
JavaScript can detect everything the user does on the page, like clicking a button, typing in an input, or moving the mouse.

**2. Validation**
Before sending any data to the server, JavaScript checks it first. For example, making sure the email field is not empty or the password is long enough.

**3. Connection Between Frontend and Backend (API)**
JavaScript sends and receives data from the server without reloading the page. This is how modern apps like Gmail or Twitter work.

**4. Implement Any Logic**
Any feature you can think of on a website showing/hiding elements, calculating prices, filtering a list JavaScript is what makes it happen.

**5. DOM (Document Object Model)**
JavaScript can read and change anything in the HTML page. The DOM is the bridge between JavaScript and HTML.

**6. BOM (Browser Object Model)**
JavaScript can also control the browser itself. Things like the URL, the back button, screen size, and pop-up windows all of these are controlled through the BOM (`window` object).

---

## Where Do We Write JavaScript?

### 1. Internal

Write JavaScript directly inside the HTML file using a `<script>` tag, usually before the closing `</body>`.

```html
<script>
  console.log("Hello from internal JS");
</script>
```

### 2. External

Write JavaScript in a separate `.js` file and link it to the HTML file.

```html
<script src="app.js"></script>
```

This is the most common and recommended approach for real projects.

### 3. Inline

Write JavaScript directly inside an HTML element attribute.

```html
<button onclick="window.alert('Hello')">Click Me</button>
```

This is not recommended for real projects because it mixes HTML and JavaScript together.

---

## Basic JavaScript Commands

### window.alert()

Shows a pop-up message in the browser.

```jsx
window.alert("Hi");
```

### window.prompt()

Shows a pop-up that asks the user to enter something. The value the user types can be saved in a variable.

```jsx
var name = window.prompt("Enter Your Name");
```

### console.log()

Prints any value to the browser console. This is the most used tool for testing and debugging your code.

```jsx
console.log("Hi");
console.log(42);
```

**How to open the console in the browser:**
Right-click anywhere on the page → Inspect → Console tab.
Or press `F12` then go to the Console tab.

---

## Important Rules in JavaScript

**JavaScript is Case-Sensitive**`name` and `Name` and `NAME` are three completely different things in JavaScript.

**We Write in Camel Case**
When a variable name has more than one word, the first word is lowercase and every word after that starts with a capital letter.

```jsx
var firstName = "Ahmed";
var myAge = 25;
var totalPrice = 100;
```

---

## How to Write Comments in JavaScript

Comments are lines that JavaScript ignores completely. We use them to explain the code.

```jsx
// This is a single-line comment

/* This is a
   multi-line comment */
```

Shortcut: `Ctrl + /` to comment/uncomment any line quickly.

---

## Automatic Semicolon Insertion

In JavaScript, the semicolon `;` at the end of a line is optional. JavaScript adds it automatically if you forget it.

```jsx
console.log("Hello")   // works fine
console.log("Hello");  // also works fine
```

However, it is a good habit to write the semicolon yourself to avoid rare bugs.

---

## JavaScript Variables

### What is a Variable?

A variable is a container that stores a value in memory so we can use it later.

```jsx
var age = 25;
var name = "Ahmed";
```

### Reserved Words

Reserved words are words that JavaScript already uses for its own purposes. You cannot use them as variable names.

Examples of reserved words:

```
var     let     const     if      else
for     while   function  return  class
new     this    true      false   null
```

### Rules for Naming Variables

- Must start with a letter, `_`, or `$`  cannot start with a number
- Cannot contain spaces
- Cannot use reserved words
- Case-sensitive (`age` and `Age` are different)
- Use camelCase for multi-word names

```jsx
// Correct
var firstName = "Ahmed";
var _score = 100;
var $price = 50;

// Wrong
var 1name = "Ahmed";   // starts with a number
var my name = "Ahmed"; // has a space
var var = 10;          // reserved word
```

---

## JavaScript Data Types

### 1. Primitive Data Types

Simple values that hold one piece of data.

| Type | Example |
| --- | --- |
| String | `"Ahmed"` |
| Number | `25` or `3.14` |
| Boolean | `true` or `false` |
| Undefined | variable declared but has no value yet |
| Null | intentionally empty value |

In ES6+ there are 2 new primitive types:
- **Symbol** — creates a completely unique value
- **BigInt** — for very large numbers beyond normal number limits

```jsx
var name = "Ahmed";        // String
var age = 25;              // Number
var isStudent = true;      // Boolean
var score;                 // Undefined (declared but no value)
var data = null;           // Null (intentionally empty)
```

### 2. Non-Primitive Data Types

Complex values that can hold multiple pieces of data.

```jsx
// Object
var person = { name: "Ahmed", age: 25 };

// Array
var colors = ["red", "green", "blue"];

// Function
function greet() {
  console.log("Hello");
}
```

### null vs undefined — What is the Difference?

|  | undefined | null |
| --- | --- | --- |
| Meaning | The variable exists but has no value yet | The variable exists and its value is intentionally empty |
| Who sets it? | JavaScript sets it automatically | The developer sets it on purpose |

```jsx
var x;
console.log(x); // undefined — we declared x but gave it no value

var y = null;
console.log(y); // null — we intentionally said "this has no value"
```

---

## JavaScript is Loosely Typed

In some languages you have to tell the language what type a variable is before using it. In JavaScript, you do not. JavaScript figures out the type automatically based on the value you give it.

```jsx
var x = 25;        // JavaScript knows this is a Number
var y = "Ahmed";   // JavaScript knows this is a String
var z = true;      // JavaScript knows this is a Boolean
```

This is called **loosely typed** or **dynamically typed**.

---

## typeof Operator

`typeof` tells you what data type a value or variable is.

```jsx
console.log(typeof 25);        // "number"
console.log(typeof "Ahmed");   // "string"
console.log(typeof true);      // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof null);      // "object"  ← known quirk in JavaScript
console.log(typeof [1,2,3]);   // "object"
```

> **Note:** `typeof null` returns `"object"` — this is a well-known quirk in JavaScript that exists for historical reasons.
> 

---

## Remember

| Goal | What to use |
| --- | --- |
| Write output in the browser console | `console.log()` |
| Write output inside the HTML page | `document.` |
| Control the browser (alerts, URL, etc.) | `window.` |

---

## Arithmetic Operations

```jsx
var x = 10;
var y = 3;

console.log(x + y);  // 13  — Addition
console.log(x - y);  // 7   — Subtraction
console.log(x * y);  // 30  — Multiplication
console.log(x / y);  // 3.33 — Division
console.log(x % y);  // 1   — Modulus (remainder)
console.log(x ** y); // 1000 — Exponentiation (10 to the power of 3)
```

### Shorthand Assignment Operators

Instead of writing `x = x + 20` we can write `x += 20`.

```jsx
var x = 10;

x += 20;  // same as x = x + 20 → result: 30
x -= 5;   // same as x = x - 5  → result: 25
x *= 2;   // same as x = x * 2  → result: 50
x /= 5;   // same as x = x / 5  → result: 10
```

### PostFix vs Prefix (++ Operator)

**PostFix (x++)** — shows the current value first, then increases it.

```jsx
var x = 20;
console.log(x++); // shows 20 — displayed the original value first
console.log(x++); // shows 21 — now it shows the increased value
```

**Prefix (++x)** — increases the value first, then shows it.

```jsx
var x = 20;
console.log(++x); // shows 21 — increased first, then displayed
```

**Simple rule to remember:**
- PostFix: display → then increase
- Prefix: increase → then display

---

## NaN — Not a Number

When you try to do a math operation on something that is not a number, JavaScript returns `NaN`.

```jsx
var x = 10;
var y = "Diaa";
console.log(x * y); // NaN
```

---

## Type Conversion

### Implicit Conversion (JavaScript does it automatically)

JavaScript sometimes converts types on its own without you asking.

```jsx
console.log(6 + 18 + "Diaa" + 20 + 5);
// Result: "24Diaa205"
```

Why? JavaScript reads left to right:
- `6 + 18` = `24` (both numbers, normal math)
- `24 + "Diaa"` = `"24Diaa"` (number meets string → becomes string concatenation)
- `"24Diaa" + 20` = `"24Diaa20"` (still concatenation)
- `"24Diaa20" + 5` = `"24Diaa205"` (still concatenation)

Once JavaScript hits a string, everything after it becomes concatenation.

### Explicit Conversion (You do it manually)

When you want to control the conversion yourself.

```jsx
// Convert String to Number
var x = "25";
console.log(Number(x));  // 25
console.log(+x);         // 25 — shorthand way using +

// Convert Number to String
var y = 25;
console.log(String(y));  // "25"
console.log(y.toString()); // "25"
```

> **Remember:** To convert a String to a Number → use `Number(value)` or `+(value)`
> 

### Concatenation

Joining strings together using `+`.

```jsx
var firstName = "Ahmed";
var lastName = "Ali";
console.log(firstName + " " + lastName); // "Ahmed Ali"

var age = 25;
console.log("My age is " + age); // "My age is 25"
```

---

## Comparison Operators

### Assignment vs Comparison

```jsx
var x = 10;  // = is Assignment — puts 10 into x, does NOT compare
```

### == (Loose Equality) — Looks at Value Only

```jsx
console.log(5 == "5");  // true — same value, ignores type difference
console.log(0 == false); // true — both are "falsy"
```

### === (Strict Equality) — Looks at Value AND Type

```jsx
console.log(5 === "5");  // false — same value but different types
console.log(5 === 5);    // true — same value AND same type
```

### != and !==

```jsx
console.log(5 != "5");   // false — loose, only checks value, they match
console.log(5 !== "5");  // true  — strict, type is different so they do not match
```

### >= and <=

```jsx
console.log(10 >= 10); // true
console.log(10 <= 5);  // false
```

These use loose comparison like `==`.

---

## Logical Operators

### && (AND) — Returns First False, or Last True

All conditions must be true to get a truthy result. As soon as JavaScript finds a false value, it stops and returns it.

```jsx
console.log("" && "Ahmed" && true);  // "" — first false value
console.log("Ahmed" && true && 20);  // 20 — all truthy, returns last one
```

### || (OR) — Returns First True, or Last False

Only one condition needs to be true. As soon as JavaScript finds a true value, it stops and returns it.

```jsx
console.log("Ahmed" || 20 || 42);  // "Ahmed" — first truthy value
console.log("" || 0 || false);     // false — all falsy, returns last one
```

### ! (NOT) — Flips the Value

```jsx
console.log(!true);  // false
console.log(!false); // true
console.log(!"");    // true  — empty string is falsy, NOT flips it
```

### Falsy Values

These values are always treated as false in JavaScript:

```jsx
false
0
""         // empty string
null
undefined
NaN
```

### Truthy Values

Everything else is truthy:

```jsx
true
// any non-zero number
// any non-empty string
```

### Operator Priority — && runs before ||

```jsx
console.log("" && false || true && "ahmed" && "mohsen" || "diaa");

// JavaScript evaluates && first:
// "" && false  → ""
// true && "ahmed" && "mohsen" → "mohsen"

// Then evaluates ||:
// "" || "mohsen" || "diaa" → "mohsen"

// Result: "mohsen"
```

---

## Conditional Statements

### if / else Statement

Used to run different code depending on a condition.

```jsx
var age = 20;

if (age >= 18) {
  console.log("You are an adult");
} else {
  console.log("You are a minor");
}
```

### else if — Multiple Conditions

```jsx
var score = 75;

if (score >= 90) {
  console.log("Excellent");
} else if (score >= 70) {
  console.log("Good");
} else if (score >= 50) {
  console.log("Pass");
} else {
  console.log("Fail");
}
```

### Nested if — if Inside if

```jsx
var age = 20;
var hasID = true;

if (age >= 18) {
  if (hasID === true) {
    console.log("Welcome, you can enter");
  } else {
    console.log("You need an ID to enter");
  }
} else {
  console.log("You are too young to enter");
}
```

### switch Statement

Used when you have one variable and want to check it against many specific values.

```jsx
var day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of the work week");
    break;
  case "Friday":
    console.log("End of the work week");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend");
    break;
  default:
    console.log("Regular day");
}
```

> **Important:** Always add `break` after each case. Without it, JavaScript will continue running into the next case even if it does not match.
`default` is like the `else` — it runs when none of the cases match.
> 

### if vs switch — What is the Difference?

|  | if / else | switch |
| --- | --- | --- |
| Best used when | Checking ranges or complex conditions | Checking one variable against specific values |
| Example | `if (age >= 18)` | `switch(day)` checking “Monday”, “Tuesday”… |
| Supports ranges? | Yes | No — only exact values |

---

*End of Session 01*