# JavaScript Session 06 (ES6+)

---

## Strict Mode

Before ES5, JavaScript ran in what’s called **Sloppy Mode**. In Sloppy Mode, JavaScript allowed a lot of messy and unsafe behavior without throwing any errors, like creating a variable without declaring it, or silently ignoring certain mistakes.

After ES5, JavaScript introduced **Strict Mode**, which blocks that messy behavior and forces you to write cleaner, safer code. It catches common mistakes early by throwing actual errors instead of silently failing.

### How to Enable Strict Mode

Add this line at the very first line of your JavaScript file (or function):

```jsx
"use strict";

x = 10; // Error in strict mode: x is not defined
```

In Sloppy Mode, the line above would have silently created a global variable `x`. In Strict Mode, it throws an error instead, forcing you to declare it properly with `let`, `const`, or `var`.

---

## var vs let vs const

These are the 3 ways to declare a variable in JavaScript, and each one behaves differently in terms of scope, re-declaration, re-assignment, and hoisting. This is one of the most common interview topics in JavaScript.

|  | var | let | const |
| --- | --- | --- | --- |
| Scope | Always Global, except inside Functions | Block Scope (Local) | Block Scope (Local) |
| Re-declaration | Allowed | Not Allowed | Not Allowed |
| Re-assignment | Allowed | Allowed | Not Allowed |
| Hoisting | Yes (initialized as `undefined`) | Yes (Temporal Dead Zone) | Yes (Temporal Dead Zone) |

### Scope Difference

```jsx
if (true) {
  var x = 10;
  let y = 20;
}

console.log(x); // 10, var leaks outside the block
console.log(y); // Error, let is trapped inside the block { }
```

### Re-declaration

```jsx
var name = "Diaa";
var name = "sh"; // allowed, no error

let age = 24;
let age = 22; // Error: Identifier 'age' has already been declared

const salary = 2000;
const salary = 3000; // Error: Identifier 'salary' has already been declared
```

### Re-assignment

```jsx
let age = 24;
age = 25; // allowed

const email = "diaaelseady@gmail.com";
email = "test@gmail.com"; // Error: Assignment to constant variable
```

### Hoisting and the Temporal Dead Zone

All 3 (`var`, `let`, `const`) are hoisted, meaning JavaScript knows they exist before the code runs. But they’re hoisted differently:

- `var` is hoisted and automatically initialized with the value `undefined`. So if you use it before its actual line, you get `undefined`, not an error.
- `let` and `const` are hoisted too, but they are NOT initialized with any value. They stay in a state called the **Temporal Dead Zone (TDZ)**, between the start of the scope and the actual line of declaration. Trying to use them inside the TDZ throws an error.

```jsx
console.log(a); // undefined, var is hoisted and initialized as undefined
var a = 10;

console.log(b); // Error: Cannot access 'b' before initialization (TDZ)
let b = 20;
```

**Why this matters in interviews:** the TDZ is exactly why `let` and `const` are considered safer than `var`. They force you to declare a variable before using it, catching bugs early instead of silently giving you `undefined` like `var` does. This is also why modern code avoids `var` almost completely:

- `var` has scope issues and can cause real bugs, avoid it in modern code
- `let` is for variables whose value will change
- `const` is for variables whose value will not change, and it’s the most commonly used of the three

---

## Default Parameters

You can give a function parameter a default value, which is used automatically if no argument is passed for it.

```jsx
function test(age, salary = 2000) {
  console.log("Hello My Age Is " + age + " My Salary Is " + salary);
}

test(24, 1000); // Hello My Age Is 24 My Salary Is 1000
test(24);       // Hello My Age Is 24 My Salary Is 2000, salary falls back to default
```

The default value is only used if the argument is missing or `undefined`. If you explicitly pass any other value, it overrides the default.

---

## Spread Operator (…)

The spread operator (`...`) takes a collection of values (like an array or object) and spreads them out individually, instead of treating them as one single value.

### Spreading an Array into Function Arguments

```jsx
let arr = [5, 20, 5];

function calcSum(x, y, z) {
  return x + y + z;
}

console.log(calcSum(...arr)); // 30, same as calcSum(5, 20, 5)
// Without spread, calcSum(arr) would pass the whole array as ONE argument
// With spread, it's passed as 3 separate arguments: 5, 20, 5
```

### Spreading an Array Inside Another Array

```jsx
let arr = [5, 20, 5];
let arrr = [1, 2, ...arr, 30];
console.log(arrr); // [1, 2, 5, 20, 5, 30]
```

### Spreading Objects to Merge Them

```jsx
let obj1 = {
  name: "Diaa",
  age: 24,
};

let obj2 = {
  wifeName: "sh",
  wifeAge: 22,
};

let person = {
  ...obj1,
  ...obj2,
};

console.log(person);
// { name: "Diaa", age: 24, wifeName: "sh", wifeAge: 22 }
```

If two spread objects have the same key, the **last one spread wins** and overwrites the earlier value.

---

## Rest Parameter (…)

The Rest Parameter looks exactly like the spread operator (`...`), but it does the opposite job. Instead of spreading values out, it collects multiple arguments into a single array parameter.

If a function has one parameter but you pass two or more arguments, the rest parameter gathers all of them into one array.

```jsx
function calcSum(...x) {
  console.log(x);
}

calcSum(20, 30, 40, 50); // [20, 30, 40, 50]
```

### Important Rule: Rest Must Be Last

Nothing is allowed to come **after** the rest parameter, but other normal parameters can come **before** it.

```jsx
// Wrong, rest parameter must be the last parameter
function calcSum(...x, y) {
  console.log(x);
}
// Error: Rest parameter must be last formal parameter

// Correct
function calcSum(y, ...x) {
  console.log(y, x);
}
calcSum(10, 20, 30, 40); // 10  [20, 30, 40]
```

### Spread vs Rest, the Key Difference

|  | Spread | Rest |
| --- | --- | --- |
| Job | Breaks a collection apart into individual values | Gathers individual values into one array |
| Used where | When calling a function, or building an array/object | When defining a function’s parameters |

---

## Destructuring

Destructuring lets you pull values out of an array or object and store them directly into separate variables, without accessing them one by one manually.

### Array Destructuring

```jsx
let arr = ["Diaa", 24, "diaaelseady@gmail.com"];

let [name, age, email] = arr;

console.log(name);  // "Diaa"
console.log(age);   // 24
console.log(email); // "diaaelseady@gmail.com"
```

**Important:** in array destructuring, the **order matters**, since values are matched by position, not by name.

### Skipping Elements in Array Destructuring

If you don’t need a specific element, you can skip it by leaving an empty slot between the commas.

```jsx
let colors = ["red", "green", "blue"];
let [first, , third] = colors;

console.log(first); // "red"
console.log(third); // "blue", "green" was skipped
```

### Object Destructuring

```jsx
let person = {
  name: "Diaa",
  age: 24,
  salary: 2000,
};

let { name, age, salary } = person;

console.log(name);   // "Diaa"
console.log(age);    // 24
console.log(salary); // 2000
```

**Important:** in object destructuring, the **order doesn’t matter**, since values are matched by property name, not position. This is the opposite of array destructuring.

### Renaming with an Alias

Sometimes the property name in the object isn’t the name you want to use in your code. You can rename it using a colon `:`.

```jsx
let person = {
  name: "Diaa",
  age: 24,
};

let { name: fullName, age: personAge } = person;

console.log(fullName);  // "Diaa"
console.log(personAge); // 24
```

This is especially useful when the object comes from an API and its property names don’t match the naming style you want to use in your project.

## Destructuring with Default Values

You can give a destructured value a fallback default, used only if the value is missing or `undefined`.

```jsx
let [a = 10, b = 20] = [5];

console.log(a); // 5, a value was provided
console.log(b); // 20, no value was provided, so the default was used
```

Same idea works with objects.

```jsx
let person = {};

let { age = 18 } = person;
console.log(age); // 18, default used since "person" has no age property
```

You can also combine a default value with an alias at the same time.

```jsx
let person = {};

let { age: userAge = 18 } = person;
console.log(userAge); // 18
```

## Array Rest in Destructuring

Just like the Rest Parameter collects leftover function arguments, the rest pattern can collect leftover array or object values during destructuring.

### With Arrays

```jsx
let numbers = [10, 20, 30, 40, 50];

let [first, second, ...rest] = numbers;

console.log(first);  // 10
console.log(second); // 20
console.log(rest);   // [30, 40, 50]
```

`first` and `second` take the first two values, and `...rest` collects everything that’s left into a new array.

### With Objects

```jsx
let person = {
  name: "Diaa",
  age: 24,
  salary: 2000,
  email: "diaaelseady@gmail.com",
};

let { name, ...otherInfo } = person;

console.log(name);      // "Diaa"
console.log(otherInfo); // { age: 24, salary: 2000, email: "diaaelseady@gmail.com" }
```

`name` is pulled out on its own, and `...otherInfo` collects every remaining property into a new object.

## Object Property Shorthand

When you have a variable with the exact same name as the property you want to create, you can skip writing the name twice.

```jsx
let name = "Diaa";
let age = 24;

// Old way
let person = { name: name, age: age };

// Shorthand
let person = { name, age };

console.log(person); // { name: "Diaa", age: 24 }
```

This only works when the variable’s name matches the property’s name exactly. It’s used constantly in modern JavaScript and React code.

---

## The “this” Keyword

`this` is one of the most confusing topics in JavaScript for a lot of people, but the core idea is simple:

`this` refers to an object, or `undefined`, depending on **who owns the function** or **who calls the function**, not where the function was written. The question to always ask yourself is: **who is calling this function?**

### this Inside an Object Method

```jsx
let person = {
  name: "Diaa",
  greet: function () {
    console.log(this.name); // "Diaa"
  },
};

person.greet();
```

Here, `this` refers to `person`, because `person` is the object that called `greet()`.

### this Inside a Regular Function

```jsx
function showThis() {
  console.log(this);
}

showThis();
```

- In **Sloppy Mode**, `this` refers to `window` (the global object), because no specific object called the function.
- In **Strict Mode**, `this` refers to `undefined`, because Strict Mode refuses to silently fall back to `window`.

```jsx
function test() {
  console.log(this);
}

test(); // window object (Sloppy Mode)

"use strict";
test(); // undefined (Strict Mode)
```

### this Inside addEventListener

```jsx
let btn = document.querySelector("button");

btn.addEventListener("click", function () {
  console.log(this); // refers to the button element the listener is attached to
});
```

This works because `addEventListener` calls your function as if the button itself called it, so `this` refers to the button.

## call, apply, and bind

These 3 methods let you manually control what `this` refers to inside a function, connecting directly back to the `this` keyword explained earlier in this session.

### call()

Runs the function immediately, and sets `this` to whatever object you pass in.

```jsx
let person1 = { name: "Diaa" };
let person2 = { name: "sh" };

function greet() {
  console.log("Hello, " + this.name);
}

greet.call(person1); // "Hello, Diaa"
greet.call(person2); // "Hello, sh"
```

You can also pass extra arguments after the object, separated by commas.

```jsx
function greet(greeting, age) {
  console.log(greeting + ", " + this.name + ". Age: " + age);
}

greet.call(person1, "Welcome", 24); // "Welcome, Diaa. Age: 24"
```

### apply()

Works exactly like `call()`, with one difference: extra arguments are passed as a single array instead of separately.

```jsx
greet.apply(person1, ["Welcome", 24]); // "Welcome, Diaa. Age: 24"
```

### bind()

Unlike `call()` and `apply()`, `bind()` does NOT run the function immediately. Instead, it returns a brand new function with `this` permanently locked to the object you passed, ready to be called whenever you want, even much later.

```jsx
let greetDiaa = greet.bind(person1);

greetDiaa("Hello", 24); // "Hello, Diaa. Age: 24"
greetDiaa("Welcome", 24); // "Welcome, Diaa. Age: 24", works again, this stays locked to person1
```

### Quick Comparison

|  | call | apply | bind |
| --- | --- | --- | --- |
| Runs immediately? | Yes | Yes | No, returns a new function |
| Extra arguments | Passed separately | Passed as an array | Passed separately |
| Common use case | One-time call with a specific `this` | Same as call, but args already in an array | Saving a function for later with `this` fixed |

---

## Arrow Functions

Arrow functions are a shorter way to write functions, introduced in ES6.

```jsx
let sayHello = () => {
  return "hello";
};

console.log(sayHello()); // "hello"
```

### Implicit Return

If an arrow function only returns one single value with no other logic, you can remove the curly braces `{ }` and the `return` keyword, and write it on one line.

```jsx
let sayHello = () => "hello";

console.log(sayHello()); // "hello"
```

### With a Parameter

```jsx
let sayHello = (userName) => {
  return "hello " + userName;
};

console.log(sayHello("Diaa")); // "hello Diaa"
```

Shortened with implicit return:

```jsx
let sayHello = (userName) => "hello " + userName;

console.log(sayHello("Diaa")); // "hello Diaa"
```

### this Inside an Arrow Function

This is the most important difference between arrow functions and regular functions: an arrow function does **not** create its own `this`. Instead, it looks outward and borrows the `this` value from the nearest regular function around it (this is called **lexical this**).

```jsx
let person = {
  name: "Diaa",
  greet: function () {
    // "this" here refers to person

    let arrowFn = () => {
      console.log(this.name); // "Diaa", borrows "this" from greet()
    };

    arrowFn();
  },
};

person.greet(); // "Diaa"
```

If `greet` itself had been written as an arrow function, `this` inside it would not refer to `person` at all, since there would be no regular function around it to borrow `this` from, and it would end up pointing to `window` or `undefined` instead. This is exactly why object methods should be written as regular functions, not arrow functions, while arrow functions are perfect for callbacks written **inside** a regular method.

---

## For Of Loop

`for...of` loops through the **values** of any iterable object (arrays, strings, Maps, Sets, etc).

A key limitation: `for...of` does not accept a condition like a normal `for` loop does (you can’t write `if` logic as part of the loop’s setup).

### Old Way vs for…of

```jsx
let array = [10, 20, 30];

// Old way
for (let i = 0; i < array.length; i++) {
  console.log(array[i]);
}

// for...of, cleaner, no need for index or .length
for (let numbers of array) {
  console.log(numbers); // 10, 20, 30
}
```

### for…of Can Use const

Even though `const` normally can’t be reassigned, `for...of` works fine with `const` because each loop run creates a **brand new variable**, it’s not reassigning the same one over and over.

```jsx
let myArray = [
  { name: "Diaa", age: 24, salary: 2000 },
  { name: "sh", age: 22, salary: 2000 },
];

for (const object of myArray) {
  console.log(object);
}
```

Each time the loop runs, `object` is treated as a fresh `const` for that specific run, not a re-assignment of the previous one, so there’s no conflict.

---

## For In Loop

`for...in` is specifically used to loop through the **keys (property names)** of an object.

### Looping Through Keys Only

```jsx
let person = {
  name: "Diaa",
  age: 24,
  salary: 2000,
};

for (const property in person) {
  console.log(property); // name, age, salary
}
```

### Looping Through Values Only

```jsx
for (const property in person) {
  console.log(person[property]); // Diaa, 24, 2000
}
```

### Looping Through Both Key and Value

```jsx
for (const property in person) {
  console.log(property + ": " + person[property]);
}
// name: Diaa
// age: 24
// salary: 2000
```

---

## Higher Order Functions

A Higher Order Function is any function that either:
- Takes another function as a parameter (a callback), or
- Returns a function

```jsx
function doSomething(callback) {
  console.log("Doing something...");
  callback();
}

doSomething(() => console.log("Done!"));
// Doing something...
// Done!
```

It’s standard practice to use arrow functions when working with higher order functions, since the syntax stays short and readable, especially when passing them as callbacks (like with `map`, `filter`, and the other array methods below).

---

## Array Methods: map, filter, find, reduce, some, every

These are the most important array methods in modern JavaScript. All of them take a callback function and run it on every element of the array, but each one returns something different.

### map(): Transforms Every Element, Returns a New Array of the Same Length

```jsx
let array = [10, 20, 30];
let newArray = array.map((num) => num * 10);
console.log(newArray); // [100, 200, 300]
```

A more realistic example, transforming an array of objects:

```jsx
let users = [
  { name: "Diaa", age: 24 },
  { name: "sh", age: 22 },
];

let names = users.map((user) => user.name);
console.log(names); // ["Diaa", "sh"]
```

### filter(): Keeps Only the Elements That Pass a Condition

```jsx
let users = [
  { name: "Diaa", age: 24 },
  { name: "sh", age: 22 },
];

let adults = users.filter((user) => user.age >= 23);
console.log(adults); // [{ name: "Diaa", age: 24 }]
```

`filter()` always returns an array, even if only one element matches, or none at all (in which case it returns an empty array).

### find(): Returns Only the First Matching Element

```jsx
let users = [
  { name: "Diaa", age: 24 },
  { name: "sh", age: 22 },
];

let user = users.find((user) => user.name === "Diaa");
console.log(user); // { name: "Diaa", age: 24 }
```

Unlike `filter()`, `find()` stops searching as soon as it finds one match, and returns that single value directly, not wrapped in an array. If nothing matches, it returns `undefined`.

### reduce(): Combines All Elements Into One Single Value

`reduce()` is the most powerful but also the most confusing of the group. It takes every element and “reduces” them down into one final result, like a running total.

```jsx
let prices = [50, 120, 30];
let totalPrice = prices.reduce((accumulator, current) => accumulator + current, 0);
console.log(totalPrice); // 200
```

How it works step by step:
- The second argument (`0`) is the **starting value** of the accumulator
- On each loop, `accumulator` holds the running result so far, and `current` is the current element
- Whatever the callback returns becomes the new `accumulator` for the next round

### some(): Checks if AT LEAST ONE Element Passes a Condition

```jsx
let array = [10, 20, 30];
console.log(array.some((item) => item > 25));  // true, 30 passes
console.log(array.some((item) => item > 100)); // false, none pass
```

### every(): Checks if ALL Elements Pass a Condition

```jsx
let array = [10, 20, 30];
console.log(array.every((item) => item > 5));  // true, all pass
console.log(array.every((item) => item > 15)); // false, 10 fails
```

### Quick Comparison

| Method | Returns | Use it when |
| --- | --- | --- |
| map | New array, same length | You want to transform every element |
| filter | New array, filtered length | You want to keep only matching elements |
| find | One value (or undefined) | You only need the first match |
| reduce | One single value | You want to combine all elements into one result |
| some | true / false | You want to know if any element matches |
| every | true / false | You want to know if all elements match |

---

## Map() Object

`Map()` is an enhanced, more powerful version of a iterable object, used to store key-value pairs. Unlike a normal object, the key in a `Map` doesn’t have to be a string, it can be any type.

```jsx
let person = new Map();
person.set("name", "Diaa");
person.set("age", 24);
person.set("salary", 2000);

console.log(person);
```

### Map() Methods

```jsx
person.set("name", "Diaa"); // adds or updates a key-value pair
person.get("name");         // returns the value for that key
person.has("name");         // returns true or false
person.delete("name");      // removes a key-value pair
person.clear();             // removes everything
person.keys();              // returns only the keys (property names)
person.values();            // returns only the values
person.size;                 // returns how many key-value pairs exist
```

### You Cannot Loop a Map() with a Normal for Loop

A `Map()` is not a normal array, so a regular `for` loop with an index won’t work on it. You must use `for...of` instead.

```jsx
let person = new Map();
person.set("name", "Diaa");
person.set("age", 24);
person.set("salary", 2000);

for (const entry of person) {
  console.log(entry);
}
// ["name", "Diaa"]
// ["age", 24]
// ["salary", 2000]
```

Each `entry` here is itself a small array of `[key, value]`.

### Looping Through Only Keys

```jsx
for (const entry of person) {
  console.log(entry[0]); // name, age, salary
}

// OR, cleaner with destructuring
for (const [key, value] of person) {
  console.log(key); // name, age, salary
}
```

### Looping Through Only Values

```jsx
for (const entry of person) {
  console.log(entry[1]); // Diaa, 24, 2000
}

// OR, cleaner with destructuring
for (const [key, value] of person) {
  console.log(value); // Diaa, 24, 2000
}
```

### Converting Map to a Normal Object

```jsx
let newPerson = Object.fromEntries(person);
console.log(newPerson); // { name: "Diaa", age: 24, salary: 2000 }
```

### Converting a Normal Object to Map

```jsx
let person = {
  name: "Diaa",
  age: 24,
  salary: 2000,
};

let newPerson = new Map(Object.entries(person));
console.log(newPerson);
```

### The in Operator

`in` checks if a property exists inside an object directly, without needing to convert it to anything, and returns a boolean.

```jsx
let person = {
  name: "Diaa",
  age: 24,
  salary: 2000,
};

console.log("salary" in person); // true
console.log("email" in person);  // false
```

### delete

Removes a property from an object.

```jsx
delete person.name;
console.log(person); // { age: 24, salary: 2000 }
```

### Famous Interview Question: Checking if a Property Exists

This is a common interview question: you get an object back from the backend, and you want to check if a specific property exists inside it before using it. There are 2 main ways to do it.

**Method 1: using the `in` operator directly**

```jsx
let user = {
  name: "Diaa",
  age: 24,
  email: "diaaelseady@gmail.com",
};

console.log("email" in user);  // true
console.log("salary" in user); // false
```

**Method 2: convert the object to a Map, then use `.has()`**

```jsx
let user = {
  name: "Diaa",
  age: 24,
  email: "diaaelseady@gmail.com",
};

let userMap = new Map(Object.entries(user));

console.log(userMap.has("email"));  // true
console.log(userMap.has("salary")); // false
```

The `in` operator is simpler and faster for this specific check, since it works directly on the object without needing to convert it to a `Map` first.

---

## Set() Object

`Set()` is an enhanced, more powerful version of a regular array. Its most important feature: it automatically removes any duplicate values.

```jsx
let numbers = new Set();
numbers.add(10);
numbers.add(10);
numbers.add(10);
numbers.add(20);
numbers.add(20);
numbers.add(30);
numbers.add(40);
numbers.add(50);

console.log(numbers); // Set(5) {10, 20, 30, 40, 50}
```

Even though `.add(10)` was called 3 times, it only appears once in the final `Set`, because duplicates are ignored automatically.

### Set() Methods

```jsx
numbers.add(60);       // adds a new value
numbers.has(10);       // returns true or false
numbers.delete(10);    // removes a specific value
numbers.clear();       // removes everything
numbers.values();      // returns the values
numbers.size;          // returns how many values exist
```

### Converting a Normal Array to a Set

```jsx
let array = [10, 10, 20, 30, 30, 40, 50];
let newArray = new Set(array);
console.log(newArray); // Set(5) {10, 20, 30, 40, 50}
```

### Removing Duplicates From an Array (Real-World Use Case)

This is the most common reason developers use `Set()`: a fast, one-line way to remove duplicate values from an array.

```jsx
let array = [10, 10, 20, 30, 30, 40, 50];
let newArray = Array.from(new Set(array));
console.log(newArray); // [10, 20, 30, 40, 50]
```

`new Set(array)` removes the duplicates, and `Array.from()` converts the result back into a normal array, since `Set` itself is not a true array.

---

## Object.freeze() and Object.seal()

Both methods restrict changes to an object, but to a different degree.

### Object.freeze()

Completely locks the object. You cannot add new properties, remove existing ones, or change any existing value.

```jsx
let person = { name: "Diaa", age: 24 };
Object.freeze(person);

person.age = 30;                        // fails silently, no change happens
person.email = "diaaelseady@gmail.com"; // fails silently, nothing gets added

console.log(person); // { name: "Diaa", age: 24 }, completely unchanged
```

### Object.seal()

Less strict than `freeze()`. You can still update the values of existing properties, but you cannot add new properties or remove existing ones.

```jsx
let person = { name: "Diaa", age: 24 };
Object.seal(person);

person.age = 25;                        // works, updating an existing property is allowed
person.email = "diaaelseady@gmail.com"; // fails, can't add a new property
delete person.name;                     // fails, can't remove a property

console.log(person); // { name: "Diaa", age: 25 }
```

### Quick Difference

|  | Object.freeze() | Object.seal() |
| --- | --- | --- |
| Add new properties | Not allowed | Not allowed |
| Remove properties | Not allowed | Not allowed |
| Change existing values | Not allowed | Allowed |

---

## Memory Management in JavaScript

JavaScript stores data in 2 different places in memory, depending on the data type.

### Stack: Primitive Data Types

Primitive data types (`String`, `Number`, `Boolean`, `undefined`, `null`, `Symbol`, `BigInt`) are stored in the **Stack**, **by value**. Each variable gets its own separate space with its own copy of the value.

```jsx
let age1 = 24;
let age2 = age1; // a full COPY of the value is made

age2 = 30;

console.log(age1); // 24, unaffected
console.log(age2); // 30
```

### Heap: Non-Primitive Data Types

Non-primitive data types (`Object`, `Array`, `Function`) are stored in the **Heap**, **by reference**. The variable doesn’t hold the actual data, it holds an address (a reference) pointing to where the data lives in the Heap.

```jsx
let person1 = { name: "Diaa" };
let person2 = person1; // copies the REFERENCE, not the object itself

person2.name = "sh";

console.log(person1.name); // "sh", also changed, because both point to the same object in the Heap
console.log(person2.name); // "sh"
```

This is the core reason why copying objects and arrays behaves differently from copying primitive values, and it’s exactly what the next section is about.

---

## 4 Ways to Copy Data

Because objects and arrays are stored by reference (in the Heap), copying them isn’t as simple as copying a number or a string. There are 4 main ways to do it, each with a different result.

### 1. Reference Copy

```jsx
var nums1 = [10, 20, 30];
var nums2 = nums1;

nums2.push(40);

console.log(nums1); // [10, 20, 30, 40], changed too!
console.log(nums2); // [10, 20, 30, 40]
```

Both variables point to the exact same array in the Heap, so editing one affects the other.

### 2. Shallow Copy

```jsx
var nums1 = [10, 20, 30];
var nums2 = [...nums1];

nums2.push(40);

console.log(nums1); // [10, 20, 30], unaffected
console.log(nums2); // [10, 20, 30, 40]
```

A shallow copy creates a new array/object with a different reference, so editing one no longer affects the other, at the top level.

**The catch:** if you have **nested objects** inside the array/object, a shallow copy only copies the outer layer. The nested objects inside are still shared by reference, so you’ll face the exact same problem as a Reference Copy, but one level deeper.

```jsx
let user1 = {
  name: "Diaa",
  wife: {
    name: "sh",
    age: 22,
  },
};

let user2 = { ...user1 };
user2.wife.name = "Sara";

console.log(user1.wife.name); // "Sara", changed too, because "wife" is still shared by reference
```

### 3. Deep Copy (JSON method)

```jsx
var nums1 = [10, 20, 30];
var nums2 = JSON.parse(JSON.stringify(nums1));
```

This converts the data to a string and immediately parses it back into a brand new object/array, completely disconnected from the original, including any nested objects inside it. It solves the nested object problem from shallow copy.

The downside: this method loses certain data types during the conversion, like functions, `undefined` values, and `Date` objects don’t survive the round trip correctly.

### 4. structuredClone(): The Best Way (2022+)

`structuredClone()` is a newer built-in browser function that creates a true, complete deep copy, including nested objects, and without the data-loss issues of the JSON method.

```jsx
var nums1 = [10, 20, 30];
var nums2 = structuredClone(nums1);
```

### All 4 Side by Side

```jsx
let user1 = {
  name: "Diaa",
  age: 24,
  wife: {
    name: "sh",
    age: 22,
  },
};

// Reference Copy
let user2 = user1;
user1.name = "Diaa Ahmed";
console.log({ user1, user2 }); // both objects change

// Shallow Copy
let user3 = { ...user1 };
user1.wife.name = "Sara"; // still shared, since "wife" is nested
console.log({ user1, user3 }); // both wife objects change

// Deep Copy
let user4 = structuredClone(user1);
user1.wife.name = "Mona";
console.log({ user1, user4 }); // user4 stays fully independent
```

### Summary Table

| Method | New Reference | Nested Objects Safe | Handles Functions | Available Since |
| --- | --- | --- | --- | --- |
| Reference Copy | No | No | Yes | Always |
| Shallow Copy | Yes | No | Yes | Always |
| Deep Copy (JSON) | Yes | Yes | No | Always |
| structuredClone() | Yes | Yes | No | 2022 |

---

## Famous Interview Question: [] == []

```jsx
if ([] == []) {
  console.log("hello");
} else {
  console.log("bye");
}
```

**Answer: it prints `"bye"`.**

**Why?** Every time you write `[]`, JavaScript creates a brand new array in the Heap with its own unique reference. So `[] == []` is really asking “does this empty array’s reference equal that completely different empty array’s reference?”, and the answer is no, even though both arrays look empty and identical. Objects and arrays are only equal to each other if they point to the exact same reference in memory, not if their content looks the same.

---

## BOM: Browser Object Model

The BOM is everything JavaScript can control about the **browser itself**, not the page content. It’s accessed through the `window` object.

### Opening and Closing Windows

```jsx
window.open("https://example.com"); // opens a new browser tab/window
window.close();                      // closes the current window/tab (only works on windows opened by script)
```

### Screen and Viewport Size

```jsx
window.innerHeight; // height of the visible browser viewport
window.innerWidth;  // width of the visible browser viewport

window.screen.height;      // full height of the user's screen/monitor
window.screen.availHeight; // available screen height (excluding taskbars, etc)
window.screen.width;       // full width of the screen
window.screen.availWidth;  // available screen width
```

### Browser History

```jsx
window.history;           // the browser's session history object
window.history.forward(); // moves forward one page, like clicking the forward button
window.history.back();    // moves back one page, like clicking the back button
window.history.go(-2);    // moves back 2 steps at once
```

### Location (Current URL Info)

```jsx
window.location;          // the full location object
window.location.href;     // the full current URL as a string
window.location.hostname; // just the domain, e.g. "example.com"
window.location.pathname; // just the path, e.g. "/products/5"

window.location.href = "https://example.com"; // redirects to a different page
window.location.reload();                      // reloads the current page
```

### One More Important BOM Object: navigator

`window.navigator` gives you information about the user’s browser and device, like the browser name, operating system, and language settings. It’s commonly used to detect browser/device info or to check things like whether the user is online.

```jsx
console.log(window.navigator.userAgent); // info about the browser and OS
console.log(window.navigator.language);  // the browser's language setting, e.g. "en-US"
console.log(window.navigator.onLine);    // true or false, is the device connected to the internet?
```

---

## Modules

A module is simply a separate JavaScript file whose code can be shared with other files, by explicitly exporting and importing what’s needed.

### Why Use Modules

- **Modularity**: break a big project into small, manageable files
- **Reusability**: write a piece of logic once and use it anywhere
- **Organization**: keep related code grouped together in its own file
- **Separation of Concerns**: each file focuses on one specific job

### Using Modules in HTML

To actually use `import`/`export` in the browser, the `<script>` tag must be marked as a module:

```html
<script src="js/main.js" type="module"></script>
```

**Important:** `type="module"` only works when the page is served through a local server (like Live Server in VS Code), not when opened directly as a file from your computer (`file://`). Opening it directly will block the imports due to browser security restrictions.

### Named Export

You can export multiple specific things from a file by name.

```jsx
// file: helpers.js
export function calcAvg(x, y) {
  return (x + y) / 2;
}

export const userName = "Diaa";
```

```jsx
// file: main.js
import { calcAvg, userName } from "./helpers.js";

console.log(calcAvg(10, 20)); // 15
console.log(userName);        // "Diaa"
```

### Default Export

Each file can only have **one** default export. It’s used when a file’s whole purpose is to export one main thing.

```jsx
// file: user.js
const user = {
  name: "Diaa",
  age: 24,
};

export default user;
```

```jsx
// file: main.js
import user from "./user.js"; // no curly braces needed for a default export

console.log(user.name); // "Diaa"
```

### Importing Everything at Once

```jsx
import * as diaa from "./diaa.js";

console.log(diaa.calcAvg(10, 20));
```

This imports all named exports from the file and groups them under one object (`diaa` in this example), so you access each one through dot notation.

### Named vs Default Export

|  | Named Export | Default Export |
| --- | --- | --- |
| Per file | Multiple allowed | Only one allowed |
| Import syntax | Requires `{ }` | No `{ }` needed |
| Name on import | Must match the export name | Can be any name you want |

---

## NPM: Node Package Manager

NPM is a tool that lets you install and manage external packages (pre-written code/libraries) inside your project, instead of writing everything from scratch.

### Dependencies

Any package you install might rely on other smaller packages to work. These required packages are called **dependencies**. NPM automatically installs all of a package’s dependencies for you when you install it.

### Before You Can Use NPM

You must install **Node.js** on your computer first, since NPM comes bundled together with Node. After installing, you can verify it worked:

```bash
node -v
npm -v
```

There are also alternative package managers that do a similar job:

| Package Manager | Who made it |
| --- | --- |
| npm | Node.js (the original) |
| pnpm | Developer community (faster, less disk space) |
| yarn | Meta (Facebook) |
| homebrew | Apple (macOS only, used for general software too) |

### How to Write NPM Commands

You need a terminal open inside your project’s folder. You have 2 common options:

1. **CMD / Terminal**, just make sure you’re standing inside the project’s folder before running any command
2. **VS Code’s Built-in Terminal**, open it with the shortcut `Ctrl + `` directly inside your editor

### Most Important Commands

```bash
npm init             # creates a new package.json file
npm install          # installs all packages listed in package.json
npm install axios    # installs a specific package
npm uninstall axios  # removes a package
```

### package.json: Why It Matters

When you deploy or share a project, the `node_modules` folder (where all installed packages physically live) is usually ignored and not uploaded, since it can be huge and is rebuilt automatically.

Instead, the server or any other developer relies on `package.json`, which lists exactly which packages and versions your project depends on. Running `npm install` reads that file and re-downloads everything needed to recreate the same `node_modules` folder from scratch.

```json
{
  "name": "my-pet-store",
  "version": "1.0.0",
  "dependencies": {
    "axios": "^1.4.0"
  }
}
```

This is why `package.json` is considered the real source of truth for a project’s dependencies, not the `node_modules` folder itself.

---

## Closures

A closure happens when a function “remembers” the variables from the scope it was created in, even after that outer function has already finished running.

![image.png](image.png)

```jsx
function outer() {
  let count = 0;

  return function () {
    count++;
    console.log(count);
  };
}

let counter = outer();
counter(); // 1
counter(); // 2
counter(); // 3
```

`outer()` runs once and finishes immediately, but the function it returns still has access to `count`. Normally, once a function finishes, its local variables should disappear, but because the returned function “closes over” `count`, it stays alive in memory as long as that returned function exists.

### Another Example

```jsx
function greet(name) {
  return function (message) {
    console.log(message + ", " + name);
  };
}

let greetDiaa = greet("Diaa");

greetDiaa("Hello");   // "Hello, Diaa"
greetDiaa("Welcome"); // "Welcome, Diaa"
```

`greetDiaa` permanently remembers `name = "Diaa"`, even though `greet()` already finished running a long time ago.

### **The Most Famous Interview Question**

```jsx
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
// What does this print?
```

### **Answer: `3, 3, 3`**

### **Why?**

1. `var` has **Function Scope**, NOT Block Scope
2. All callbacks share the **SAME** `i` variable
3. Loop finishes → `i = 3`
4. After 1 second, each callback prints `3`

**Timeline:**

```
t=0ms:   i=0, i=1, i=2 (3 setTimeout scheduled)
t=0ms:   Loop ends → i=3
t=1000ms: Callback 1 → logs 3
t=1000ms: Callback 2 → logs 3
t=1000ms: Callback 3 → logs 3
```

---

## **Solutions**

### **Solution 1: Use `let`**

```jsx
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
// Output: 0, 1, 2
```

**Why?** `let` has **Block Scope** → each iteration gets its OWN `i`.

**Visual:**

```
Iteration 1: i = 0 (in its own block)
Iteration 2: i = 1 (in its own block)
Iteration 3: i = 2 (in its own block)

Each callback closes over its own i
```

### **Solution 2: IIFE (Legacy)**

```jsx
for (var i = 0; i < 3; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j);
    }, 1000);
  })(i);
}
// Output: 0, 1, 2
```

**How it works:** IIFE creates a new scope for each iteration, capturing the current `i` as `j`.

### **Solution 3: `bind()`**

```jsx
for (var i = 0; i < 3; i++) {
  setTimeout(console.log.bind(console, i), 1000);
}
// Output: 0, 1, 2
```

---

## **5. var vs let in Closures**

| **Feature** | **`var`** | **`let`** |
| --- | --- | --- |
| Scope | Function Scope | Block Scope |
| Hoisting | Yes (initialized as `undefined`) | Yes (TDZ) |
| In `for` loop | One shared variable | New variable per iteration |
| `setTimeout` output | `3,3,3` | `0,1,2` |

---

## Debouncing:

This is a very common real-world pattern, and a very common interview question. You don’t want a function (like a search request) to run on every single keystroke, you only want it to run once the user actually stops typing for a short moment.

```jsx
let searchBtn = document.querySelector(".search");
let count = 0;
let timeSearch;

let search = () => {
  clearTimeout(timeSearch);

  timeSearch = setTimeout(() => {
    count++;
    console.log(count, searchBtn.value);
  }, 800);
};

searchBtn.addEventListener("input", search);
```

### How This Code Works

### 1. Store the Timer ID

```jsx
lettimeSearch;
```

`setTimeout()` returns a timer ID, which is stored in `timeSearch` so it can be canceled later.

---

### 2. Cancel the Previous Timer

```jsx
clearTimeout(timeSearch);
```

Every time the user types, the previous timer is cleared.

This prevents the function from executing too early.

---

### 3. Create a New Timer

```jsx
timeSearch=setTimeout(() => {
count++;
console.log(count,searchBtn.value);
},800);
```

A new timer starts and waits for **800ms**.

If the user types again before 800ms passes, the timer is canceled and restarted.

---

### 4. Execute Only After Typing Stops

The callback function runs only when the user stops typing for at least **800ms**.

Example:

```
User types: h
User types: he
User types: hel
User types: hell
User types: hello
```

Only one execution occurs:

```
1 hello
```

Instead of:

```
1 h
2 he
3 hel
4 hell
5 hello
```

---

### Interview Definition

> **Debounce is a technique that delays a function's execution until a specified time has passed since the last event occurred. It helps improve performance by reducing unnecessary function calls.**
> 

### Common Use Cases

- Search inputs
- API requests
- Window resize events
- Scroll events
- Auto-save features
- Form validation while typing

---

## Optional Chaining (?.) and Nullish Coalescing (??)

These are 2 modern operators (ES2020) that make working with data from APIs much safer, since API data is often incomplete or missing certain fields.

### Optional Chaining (?.)

Normally, trying to access a property on something that’s `undefined` or `null` throws an error and breaks your code.

```jsx
let user = {
  name: "Diaa",
  address: {
    city: "Cairo",
  },
};

console.log(user.address.city);  // "Cairo"
console.log(user.contact.phone); // Error: Cannot read properties of undefined
```

`?.` checks if the thing right before it exists. If it’s `null` or `undefined`, it stops immediately and safely returns `undefined`, instead of throwing an error.

```jsx
console.log(user.contact?.phone); // undefined, no error this time
```

It also works with function calls, only calling the function if it actually exists.

```jsx
user.greet?.(); // does nothing if "greet" doesn't exist on user, instead of throwing an error
```

### Nullish Coalescing (??)

Returns the value on the right side ONLY if the value on the left is `null` or `undefined`. This is different from `||`, which also falls back on any falsy value like `0`, `""`, or `false`, which can cause bugs with real data.

```jsx
let age = 0;

console.log(age || 18); // 18, wrong! 0 is a valid age, but || treats it as falsy
console.log(age ?? 18); // 0, correct, age is not null/undefined so ?? keeps it as is
```

### Real Use Case: Combining Both

```jsx
let salary = user.salary ?? 0;
let city = user?.address?.city ?? "Unknown";

console.log(salary); // 0 if salary is missing, otherwise the real value
console.log(city);   // the real city, or "Unknown" if address or city is missing
```

---

## The Date Object

`new Date()` creates an object representing a specific point in time. With no arguments, it represents the current date and time.

```jsx
let now = new Date();
console.log(now);
```

### Common Methods

```jsx
now.getFullYear(); // the full year, e.g. 2026
now.getMonth();    // the month, but 0-indexed (0 = January, 11 = December)
now.getDate();     // the day of the month, 1 to 31
now.getDay();      // the day of the week, 0-indexed (0 = Sunday, 6 = Saturday)
now.getHours();    // the hour, 0 to 23
now.getMinutes();  // the minutes, 0 to 59
now.getSeconds();  // the seconds, 0 to 59
```

### Creating a Specific Date

You can also create a `Date` object for any specific date you want, instead of the current one.

```jsx
let graduationDate = new Date("2026-06-15");
console.log(graduationDate.getFullYear()); // 2026
console.log(graduationDate.getMonth());    // 5, since months are 0-indexed (5 = June)
```

**Important note:** `getMonth()` being 0-indexed is a very common source of bugs, always remember to add 1 if you want to display the month in its normal human-readable form (1 to 12).

---

## JS Expression vs Statement

### Expression

An expression is any piece of code that **returns a value**. It’s something that can be calculated/evaluated, and the result is always a value.

```jsx
5 + 10          // expression, evaluates to 15
"Diaa"          // expression, evaluates to the string itself
calcAvg(10, 20) // expression, calling a function is an expression
```

### Statement

A statement is a line of code that **performs an action**, but doesn’t necessarily return a value itself.

```jsx
let age = 24;       // statement
if (age > 18) { }   // statement
function greet() {} // statement (this is the function's definition)
```

### So What About a Function With No return?

```jsx
function greet() {
  console.log("hello");
}

let result = greet();
console.log(result); // undefined
```

Any function that doesn’t explicitly use `return` automatically returns `undefined`.

The important distinction here: defining a function (`function greet() {}`) is a **Statement**, while calling that function (`greet()`) is an **Expression**, because calling it always produces a value, even if that value is just `undefined`.

---

## Declarative vs Imperative Programming

These are 2 different overall styles of writing code.

### Imperative (the HOW)

You tell the computer exactly **how** to do something, step by step, controlling every detail yourself.

```jsx
const el = document.querySelector("h1");
el.innerText = "Hello";
el.style.color = "red";
el.classList.add("active");
// You are telling the computer: go do this, then this, then this
```

You manually selected the element, then manually updated it, step by step. You’re in full control of every step.

### Declarative (the WHAT)

You describe **what** result you want, without writing the steps to get there. The underlying system or framework figures out how to achieve it.

```jsx
// Declarative, React
// You only describe what the UI should look like based on state
// React takes care of all the DOM updates internally
```

### The Difference in One Line

```
Imperative   ->  Describe the steps (the HOW)
Declarative  ->  Describe the result (the WHAT)
```

### Where React Fits In

React is built around the **Declarative** style. React only accepts Expressions inside JSX, not Statements, which is directly connected to everything explained above.

```jsx
// Will NOT work in JSX, it's a Statement
{if (isLoggedIn) { return <p>Hello</p> }}

// Will work in JSX, it's an Expression
{isLoggedIn ? <p>Hello</p> : <p>Login</p>}
```

**Common interview question: “Which one is React, and why?”**

> React is declarative because the developer only describes what the UI should look like based on the current state, and React itself handles how to update the DOM efficiently behind the scenes, using the Virtual DOM.
> 

In short: imperative programming focuses on describing how to build something step by step, while declarative programming focuses on describing what you want the end result to be. React is declarative because the UI is written based on state, and React takes care of updating the DOM internally.

---

*End of Session 06*