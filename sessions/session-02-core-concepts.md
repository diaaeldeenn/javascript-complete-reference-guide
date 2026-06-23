# JavaScript Session 02

---

## Loops

Loops let us repeat a block of code multiple times instead of writing it over and over manually.

### For Loop

Used when you know exactly how many times you want to repeat something.

```jsx
for (let i = 0; i < 5; i++) {
  console.log(i);
}
// Output: 0 1 2 3 4
```

It has 3 parts:
- `let i = 0` → starting point
- `i < 5` → condition, the loop keeps running while this is true
- `i++` → what happens after each loop run

### While Loop

Used when you don’t know exactly how many times you’ll loop, but you have a condition to check before each run.

```jsx
let i = 0;

while (i < 5) {
  console.log(i);
  i++;
}
// Output: 0 1 2 3 4
```

The condition is checked **before** running the code. If the condition is false from the start, the loop never runs.

### Do While Loop

Same as while loop, but it runs the code **at least once** before checking the condition.

```jsx
let i = 10;

do {
  console.log(i);
  i++;
} while (i < 5);
// Output: 10
// Runs once even though 10 < 5 is false
```

### For Of (ES6+)

Used to loop through the **values** of an array or any iterable.

```jsx
const colors = ["red", "green", "blue"];

for (const color of colors) {
  console.log(color);
}
// Output: red green blue
```

### For In (ES6+)

Used to loop through the **keys** of an object.

```jsx
const person = { name: "Ahmed", age: 25 };

for (const key in person) {
  console.log(key, person[key]);
}
// Output: name Ahmed
//         age 25
```

### Which One Should I Use?

| Loop | Best used when |
| --- | --- |
| for | You know the exact number of repetitions |
| while | You have a condition, but don’t know the exact count |
| do while | You need the code to run at least once regardless of condition |
| for of | You want to loop through array values |
| for in | You want to loop through object keys |

---

## DRY — Don’t Repeat Yourself

DRY is a core programming principle that means: if you find yourself writing the same code more than once, you should turn it into something reusable instead — like a loop or a function.

```jsx
// Without DRY — repeating the same line
console.log("Hello");
console.log("Hello");
console.log("Hello");

// With DRY — using a loop instead
for (let i = 0; i < 3; i++) {
  console.log("Hello");
}
```

This keeps your code shorter, easier to maintain, and less likely to have bugs.

---

## Useful Math Methods

### Math.trunc()

Removes any decimal part of a number and keeps only the whole number — without rounding.

```jsx
console.log(Math.trunc(4.9));  // 4
console.log(Math.trunc(4.1));  // 4
console.log(Math.trunc(-4.7)); // -4
```

### Math.random()

Generates a random decimal number between 0 (included) and 1 (excluded).

```jsx
console.log(Math.random()); // example: 0.4839201...
```

If you want a bigger range, multiply by the number you want.

```jsx
console.log(Math.random() * 10); // random number between 0 and 10
```

### Generating a Random Number in a Specific Range (like a Dice)

If you want a range from 1 to 6, like a dice:

```jsx
var dice = Math.trunc(Math.random() * 6 + 1);
```

How this works:
- `Math.random()` gives a number between 0 and 1
- `* 6` stretches the range to be from 0 to 5 (almost 6)
- `+ 1` shifts the range to be from 1 to 6
- `Math.trunc()` removes the decimal part so we get a clean whole number

---

## Building HTML with a Loop

This is a common real-world pattern: using a loop to build repeated HTML content, then injecting it into the page.

```jsx
let cartona = "";

for (let i = 0; i < 10; i++) {
  cartona += `<h2>Hello</h2>`;
}

document.getElementById("demo").innerHTML = cartona;
```

What’s happening here:
- `cartona` starts as an empty string
- The loop runs 10 times, and each time it adds another `<h2>Hello</h2>` to the string using `+=`
- After the loop finishes, `cartona` contains 10 `<h2>` tags joined together
- `document.getElementById("demo")` finds the HTML element with `id="demo"`
- `.innerHTML = cartona` injects all that generated HTML into the page

This is exactly how dynamic content (like product lists or comment sections) gets built in real websites.

---

## Functions

A function is a reusable block of code that runs only when you call it.

### Function Naming Rules

- First letter must be small (lowercase)
- The name must be a verb, since a function performs an action

```jsx
function calcAvg(x, y) {
  let avg = (x + y) / 2;
  return avg;
}

let avg = calcAvg(10, 20);
console.log(avg); // 15   ← this is called "calling" or "invoking" the function
```

### Function Vocabulary

| Term | Meaning |
| --- | --- |
| Parameters | The names written inside the parentheses when you **define** the function (`x`, `y`) |
| Arguments | The actual values you pass in when you **call/invoke** the function (`10`, `20`) |
| Scope | Everything written inside the function’s curly braces `{ }` — these variables only exist inside the function |

```jsx
function calcAvg(x, y) {     // x and y are parameters
  let avg = (x + y) / 2;     // avg only exists in this scope
  return avg;
}

calcAvg(10, 20);              // 10 and 20 are arguments
```

---

## Types of Functions

### 1. Declaration Function

Starts with the word `function` and has a name. It can be called even before it’s written in the code (more on this in Hoisting below).

```jsx
function getAvg(x, y) {
  var sum = x + y;
  var avg = sum / 2;
  console.log(avg);
}

getAvg(10, 20); // 15
```

### 2. Expression Function

A function stored inside a variable. It has no name of its own (anonymous), and it cannot be called before it’s defined.

```jsx
var sayHello = function () {
  console.log("hello");
};

sayHello(); // hello
```

### 3. Self-Invoked Function (IIFE)

Short for “Immediately Invoked Function Expression.” This function runs immediately, one time only, without needing to be called separately. It’s a type of expression function, also called an anonymous function.

```jsx
(function () {
  console.log("hello");
})();
```

The extra `()` at the end is what makes it run immediately.

---

## The return Statement

To actually use the result of a function elsewhere in your code, you must use `return`, and store the result in a variable.

```jsx
function calcAvg(x, y) {
  return (x + y) / 2;
}

let result = calcAvg(10, 20); // result now holds 15
```

### Why return Matters

`return` does two things: it sends a value back out of the function, and it immediately stops the function from running. Anything written **after** `return` inside the function will never run.

```jsx
function test() {
  return "Hello";
  console.log("This will never run"); // unreachable code
}

console.log(test()); // "Hello"
```

If a function has no `return`, it gives back `undefined` by default.

---

## Hoisting

Hoisting is a JavaScript behavior where function and variable **declarations** are moved to the top of their scope in memory before the code actually runs. This means some declarations can be used before they appear in the code.

The key rule to understand hoisting is this: JavaScript hoists **declarations**, but it does not hoist the **assignment/value**. This works differently for declaration functions vs expression functions vs variables.

### Example 1 — Two Declaration Functions with the Same Name

```jsx
function foo() {
  function bar() {
    return 3;
  }
  return bar();
  function bar() {
    return 8;
  }
}

alert(foo()); // 8
```

**Why 8?** Both `bar` functions are full declaration functions, so both get hoisted completely (with their full code) to the top of `foo`. Since they have the same name, the second one **overwrites** the first one in memory before any code runs. So by the time `bar()` is called, only the second version (returning 8) exists.

### Example 2 — Two Expression Functions with the Same Name (using var)

```jsx
function foo() {
  var bar = function () {
    return 3;
  };
  return bar();
  var bar = function () {
    return 8;
  };
}

alert(foo()); // 3
```

**Why 3?** With `var`, only the **declaration** (`var bar`) is hoisted to the top — not the value/function assigned to it. So at the top of `foo`, JavaScript only knows `bar` exists, but it’s `undefined` until the actual line of code runs. The code runs top to bottom: `bar` gets assigned the function that returns 3, then `bar()` is called and returns 3, and the function exits with `return` before ever reaching the second assignment.

### Example 3 — Mixing Declaration Function and Expression Function

```jsx
function foo() {
  return bar();
  function bar() {
    return 3;
  }
  var bar = function () {
    return 8;
  };
}

alert(foo()); // 3
```

**Why 3?** The declaration function `function bar() { return 3; }` is fully hoisted to the top, value included. The expression function (`var bar = function() {...}`) only has its `var bar` part hoisted, not its value. So at the moment `bar()` is called on the very first line, only the fully-hoisted declaration function exists and runs — returning 3. The expression assignment never even gets the chance to run because `return` already exited the function.

### Simple Takeaway for Interviews

- **Declaration functions** are hoisted completely — name AND code.
- **var variables** (including expression functions stored in `var`) only have their name hoisted, not their value — the value is only assigned when that line actually executes.
- If two declaration functions share a name, the **last one defined wins**, because hoisting overwrites earlier ones.

---

## Objects

An object is a way to simulate reality in code — representing something with multiple related characteristics in one place.

Anything written inside an object is called a **property**.

### Creating an Object

```jsx
const person = {
  name: "Ahmed",
  age: 25,
  isStudent: true
};
```

### Functions Inside Objects (Methods)

A function is actually a special type of object. When a function is written inside an object, it’s called a **method**.

There are two ways to write a method inside an object:

```jsx
// Way 1: key : value, where value is a function
const person = {
  name: "Ahmed",
  eat: function () {
    console.log("Eating...");
  }
};

// Way 2: writing the function directly (shorthand, ES6+)
const person = {
  name: "Ahmed",
  eat() {
    console.log("Eating...");
  }
};
```

### The window Object

`window` is the super global object in the browser — every other global object (like `document`) actually lives inside it.

`window` is the only object where you can call its methods and properties **without writing its name**.

```jsx
window.alert("Hi");  // normal way
alert("Hi");          // also works — window is implied automatically

window.console.log("Hi"); // normal way
console.log("Hi");         // also works — window is implied automatically
```

### How to Get Data From an Object

There are 2 ways:

```jsx
const person = {
  name: "Ahmed",
  age: 25,
  eat: function () {
    console.log("Eating...");
  }
};

// Dot Notation
console.log(person.name); // "Ahmed"

// Bracket Notation
console.log(person["age"]); // 25

// Calling a method with bracket notation
console.log(person["eat"]()); // "Eating..."
```

### How to Add/Update Data in an Object

```jsx
person.age = 30;       // Dot notation
person["age"] = 30;    // Bracket notation
```

Both add a new property if it doesn’t exist, or update it if it already does.

---

## Arrays

An array is a **linear data structure** — meaning it is ordered, unlike an object which is unordered.

An array is also a special type of object.

Anything written inside an array is called an **element**.

```jsx
const fruits = ["apple", "banana", "orange"];
```

### How to Get Data From an Array

You access elements using their index number. Indexes start from 0.

```jsx
console.log(fruits[0]); // "apple"
console.log(fruits[1]); // "banana"
console.log(fruits[2]); // "orange"
```

---

## Array Methods

### 1. array.sort()

Sorts the elements of the array.

```jsx
const numbers = [3, 1, 4, 1, 5];
numbers.sort();
console.log(numbers); // [1, 1, 3, 4, 5]
```

### 2. array.reverse()

Reverses the order of the array.

```jsx
const numbers = [1, 2, 3];
numbers.reverse();
console.log(numbers); // [3, 2, 1]
```

### 3. array.push()

Adds elements to the **end** of the array, and returns the new real length of the array (not the index).

```jsx
const fruits = ["apple", "banana"];
const newLength = fruits.push("orange");
console.log(fruits);     // ["apple", "banana", "orange"]
console.log(newLength);  // 3
```

### 4. array.unshift()

Adds elements to the **start** of the array, and returns the new length.

```jsx
const fruits = ["banana", "orange"];
const newLength = fruits.unshift("apple");
console.log(fruits);     // ["apple", "banana", "orange"]
console.log(newLength);  // 3
```

### 5. array.pop()

Removes the **last** element from the array and returns it.

```jsx
const fruits = ["apple", "banana", "orange"];
const removed = fruits.pop();
console.log(fruits);   // ["apple", "banana"]
console.log(removed);  // "orange"
```

### 6. array.shift()

Removes the **first** element from the array and returns it.

```jsx
const fruits = ["apple", "banana", "orange"];
const removed = fruits.shift();
console.log(fruits);   // ["banana", "orange"]
console.log(removed);  // "apple"
```

### 7. array.splice(startIndex, deleteCount)

You choose where to start deleting from (`startIndex`) and how many elements to delete (`deleteCount`). It returns the elements it removed, and it **modifies the original array**.

```jsx
const fruits = ["apple", "banana", "orange", "mango"];
const removed = fruits.splice(1, 2);
console.log(fruits);   // ["apple", "mango"]
console.log(removed);  // ["banana", "orange"]
```

### 8. array.slice(startIndex, endIndex)

You choose where to start (`startIndex`) and where to stop (`endIndex`, not included). It returns the selected elements, but unlike `splice`, it does **not** modify the original array — it returns a new copy.

```jsx
const fruits = ["apple", "banana", "orange", "mango"];
const sliced = fruits.slice(1, 3);
console.log(fruits);  // ["apple", "banana", "orange", "mango"] — unchanged
console.log(sliced);  // ["banana", "orange"]
```

> **Important:** Apart from `push`, `unshift`, `pop`, `shift`, and `splice`, most other array methods (like `slice`) do **not** edit the original array — they return a copy instead.
> 

### 9. array.includes(value, startIndex)

Searches the array and returns `true` or `false`.

```jsx
const fruits = ["apple", "banana", "orange"];
console.log(fruits.includes("banana")); // true
console.log(fruits.includes("grape"));  // false
```

### 10. array.indexOf(value, startIndex)

Searches the array and returns the index of the value. If not found, returns `-1`. If there are duplicates, it returns the index of the **first** match.

```jsx
const numbers = [10, 20, 30, 20];
console.log(numbers.indexOf(20)); // 1 — first match
console.log(numbers.indexOf(99)); // -1 — not found
```

### 11. array.lastIndexOf(value, startIndex)

Same as `indexOf`, but if there are duplicates, it returns the index of the **last** match.

```jsx
const numbers = [10, 20, 30, 20];
console.log(numbers.lastIndexOf(20)); // 3 — last match
```

### 12. array.with(index, newValue)

Replaces the value at a specific index and returns a new array (does not modify the original).

```jsx
const numbers = [10, 20, 30];
const updated = numbers.with(1, 99);
console.log(numbers); // [10, 20, 30] — unchanged
console.log(updated); // [10, 99, 30]
```

### Pushing a New Element to Any Specific Position

To insert an element at any position you want (not just start or end), use `splice` with `0` as the delete count:

```jsx
const fruits = ["apple", "orange"];
fruits.splice(1, 0, "banana");
console.log(fruits); // ["apple", "banana", "orange"]
```

- First argument → index where you want to insert
- Second argument → `0` means delete nothing
- Third argument → the new element to insert

---

## Array vs Object

|  | Array | Object |
| --- | --- | --- |
| Based on | Index | Key |
| Order | Ordered | Unordered |
| Best for | Related or unrelated elements in a sequence | Related properties or methods describing one thing |

---

*End of Session 02*