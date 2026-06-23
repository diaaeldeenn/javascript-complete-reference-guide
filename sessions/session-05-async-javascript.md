# JavaScript Session 05

---

## Sync vs Async Code

### Sync Code (Synchronous)

Sync code runs **line by line**, and each line must finish completely before the next one starts. If one line takes a long time, everything else waits. This is called **blocking code**.

```jsx
console.log("First");
console.log("Second");
console.log("Third");
// Output: First, Second, Third — always in this exact order
```

### Async Code (Asynchronous)

Async code is **non-blocking**. Instead of waiting for a slow operation to finish, JavaScript moves on to the next line and comes back to the async operation later when it’s done.

```jsx
console.log("First");

setTimeout(function () {
  console.log("Second"); // this is async — runs later
}, 2000);

console.log("Third");

// Output: First, Third, Second
// "Third" prints before "Second" even though "Second" appears first in the code
```

This is essential for things like fetching data from an API — you don’t want the entire page to freeze while waiting for a server response.

---

## JS V8 Runtime Environment (Chrome)

JavaScript by itself is just a language — it needs an **engine** to actually run it. In Chrome (and Node.js), that engine is called the **V8 Engine**. The full picture of how JavaScript runs is called the **Runtime Environment**, and it’s made of 4 main parts working together.

> The diagram below shows the full flow visually.
> 

![JS V8 Engine.JPG](JS_V8_Engine.jpg)

---

### Part 1: Execution Stack (Call Stack) — LIFO

This is where all **sync code** runs. Every time a function is called, it gets pushed onto the stack. When it finishes, it gets popped off.

It works as **LIFO** (Last In, First Out) — the last function that entered is the first one to leave.

```jsx
function getSum(a, b) {
  return a + b;
}

console.log("one");
getSum(5, 10); // pushed to stack, runs, then popped off
console.log("three");
```

If the Call Stack is busy running sync code, nothing else can run — this is what “blocking” means.

---

### Part 2: Web APIs

Any **async code** does NOT run in the Call Stack. Instead, it gets sent immediately to the **Web APIs** environment, which lives outside of JavaScript itself — it’s provided by the browser.

Web APIs handle the waiting so the Call Stack stays free to keep running other code.

Things that go to Web APIs:

```
AJAX
DOM Events
setTimeout
setInterval
Promise
fetch
```

---

### Part 3: Callback Queue (Message Queue)

Once an async operation finishes in Web APIs, it doesn’t go straight back to the Call Stack. It first moves to the **Callback Queue**, waiting in line.

The Callback Queue is split into two priority levels:

**MicroTask Queue (High Priority — runs FIRST) — FIFO**

```
AJAX
Promise
fetch
```

**MacroTask Queue (Low Priority — runs SECOND) — FIFO**

```
setTimeout
setInterval
DOM Events
```

Both queues follow **FIFO** (First In, First Out) — the first one that arrived is the first one to run.

The Event Loop always empties the entire MicroTask Queue before even looking at the MacroTask Queue.

---

### Part 4: Event Loop

The **Event Loop** is the piece that ties everything together. Think of it as a security guard standing between the Callback Queue and the Call Stack — constantly watching and deciding what goes in next.

Its job is simple but critical:

1. Watch the Call Stack — is it empty?
2. If yes, check the MicroTask Queue first — is there anything waiting?
3. Push the first MicroTask into the Call Stack and let it run
4. Repeat until the MicroTask Queue is completely empty
5. Only then, look at the MacroTask Queue and do the same

The Event Loop never pushes anything into the Call Stack while it’s still busy. It only moves things in when the stack is completely clear.

---

### The Full Flow — Step by Step

```
Your Code Runs
     |
     |---> Sync Code ---------> Call Stack (runs immediately)
     |
     |---> Async Code --------> Web APIs (waits here)
                                    |
                                    | (when finished)
                                    v
                             Callback Queue
                          /                  \
                   MicroTask               MacroTask
                (Promise/AJAX/fetch)   (setTimeout/DOM Events)
                   runs FIRST              runs SECOND
                        \                  /
                         \                /
                          v              v
                          Event Loop watches
                               |
                               v
                          Call Stack (empty?)
                               |
                               v
                          Runs the next task
```

---

### Test Your Understanding — What is the Output?

```jsx
console.log("one");      // sync

setTimeout(() => {
  console.log("two");   // async - MacroTask
}, 0);

Promise.resolve().then(() => {
  console.log("three"); // async - MicroTask
});

console.log("four");    // sync
```

Work out the answer yourself before looking below.

**Answer and explanation:**

```
one    → sync, runs immediately in the Call Stack
four   → sync, runs immediately after "one"
three  → MicroTask (Promise), runs before MacroTask
two    → MacroTask (setTimeout), runs last even though delay is 0ms
```

Even though `setTimeout` has a delay of `0`, it still goes through Web APIs and MacroTask Queue. A Promise always wins over setTimeout because MicroTasks always run before MacroTasks — no exceptions.

---

## JavaScript is Single Thread

JavaScript runs on a **single thread** — meaning it can only do one thing at a time, line by line. It has one Call Stack, and only one piece of code can be running in it at any given moment.

### But Wait — How Does Async Work Then?

This is the question that confuses a lot of people: if JavaScript is single-threaded, how can it handle async operations without freezing?

The answer is: **JavaScript itself is single-threaded, but the browser is not.**

When JavaScript hits an async operation (like `setTimeout` or `fetch`), it hands it off to the browser’s Web APIs — which run on separate threads provided by the browser, not by JavaScript. JavaScript then keeps running normally on its single thread. When the browser finishes the async operation, it puts the result in the Callback Queue, and the Event Loop brings it back into JavaScript when the Call Stack is free.

So JavaScript never actually does two things at once — it just delegates the waiting to the browser, stays free, and picks up the result later.

```
Multi-Thread  → multiple things happening at the exact same time
Single-Thread → one thing at a time, BUT delegates waiting to the browser
```

This is why JavaScript can feel like it’s doing multiple things at once — it’s not, the browser is handling the waiting part while JavaScript keeps moving.

---

## How to Control Async Code

This is one of the most important topics for interviews. JavaScript has gone through 3 generations of solving the same problem: “how do I use the result of an async operation?”

---

### 1. Callback Function

A callback is a function passed as a parameter to another function, to be called when that function finishes its work.

```jsx
function calculate(num1, num2, callback) {
  callback(num1, num2);
}

function add(x, y) {
  console.log(x + y);
}

calculate(5, 3, add);
```

### The Problem: Callback Hell (Pyramid of Doom)

When you need to chain multiple async operations — where each one depends on the result of the previous one — callbacks get nested inside each other deeper and deeper, creating what’s known as **Callback Hell**.

> The image below shows what this looks like in real code — it forms a pyramid shape that gets wider and deeper with every level, making it nearly impossible to read, debug, or maintain.
> 

![1747521846876.png](1747521846876.png)

```jsx
getUser(1, function (user) {
  getOrders(user.id, function (orders) {
    getOrderDetails(orders[0].id, function (details) {
      getPaymentInfo(details.paymentId, function (payment) {
        // deeper and deeper...
      });
    });
  });
});
```

This is the “Pyramid of Doom” — every callback adds another level of indentation. This is the problem that Promises were created to solve.

---

### 2. Promise (ES6 — 2015)

A Promise is a container for async code. Instead of nesting callbacks, you chain `.then()` calls in a flat, readable way.

A Promise has 2 possible statuses:
1- **Pending** — still running, result not ready yet

2- **Settled** — finished, either:
A) **Resolved** (Success) — operation succeeded
B) **Rejected** (Failed) — operation failed

```jsx
function checkResult(degree) {
  return new Promise(function (resolve, reject) {
    if (degree >= 50) {
      resolve("Success");
    } else {
      reject("Failed");
    }
  });
}

checkResult(80)
  .then(function (message) {
    console.log(message);
  })
  .catch(function (error) {
    console.log(error);
  });
```

### The Problem: Then Chain

Even though Promises are much better than callbacks, chaining many `.then()` calls can still become long and hard to follow, especially for complex operations.

```jsx
fetch(url)
  .then(function (res) { return res.json(); })
  .then(function (data) { return processData(data); })
  .then(function (result) { return saveResult(result); })
  .then(function (saved) { console.log(saved); })
  .catch(function (err) { console.log(err); });
```

This is the “then chain” problem — it’s better than callback hell, but it’s still a chain of callbacks in disguise. This is the problem that async/await was created to solve.

---

### 3. Async / Await (ES8 — 2017)

`async/await` is the modern and most recommended way to handle async code. It makes async code look and read like normal sync code — no nesting, no chains.

**Rules:**
- `await` can only be used inside an `async` function
- `await` pauses execution inside the function until the Promise resolves
- The function always returns a Promise

```jsx
async function getUserData() {
  let response = await fetch("https://api.example.com/user/1");
  let data = await response.json();
  console.log(data);
}

getUserData();
```

### Always Use with try…catch

Since async operations can fail, always wrap `async/await` code in `try...catch` to handle errors cleanly.

```jsx
async function getUserData() {
  try {
    let response = await fetch("https://api.example.com/user/1");
    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.log("Something went wrong:", error);
  }
}

getUserData();
```

## throw new Error()

Instead of only reacting to errors JavaScript creates on its own, you can create and throw your own custom error, which is especially useful for validating input before running any logic.

```jsx
function checkAge(age) {
  if (age < 18) {
    throw new Error("Age must be 18 or older");
  }

  return "Access granted";
}

try {
  console.log(checkAge(15));
} catch (error) {
  console.log(error.message);
}
```

`throw` immediately stops the function from running any further, exactly like `return`. This pairs perfectly with `try...catch`, the `catch` block receives your custom `Error` object, and `error.message` holds the exact text you wrote when creating it.

---

### 4. fetch (ES8 — 2017)

`fetch` is the modern built-in way to make API requests in JavaScript. It returns a Promise, so it works perfectly with `async/await`.

### The .json() Method

When `fetch` gets a response back from the server, the response body is not immediately available as usable data — it’s a raw stream. Calling `.json()` reads that stream and converts it into a JavaScript object or array. Since this is also async, it needs its own `await`.

```jsx
let response = await fetch("https://api.example.com/data");
let data = await response.json(); // converts the raw response to usable JS object
```

### fetch with All Options

```jsx
async function sendData() {
  try {
    let response = await fetch("https://api.example.com/products", {
      method: "POST",          // GET, POST, PUT, PATCH, DELETE
      headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer your-token-here"
      },
      body: JSON.stringify({   // data you want to send (POST/PUT/PATCH only)
        name: "Cat Food",
        price: 50,
        category: "cats"
      })
    });

    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.log("Error:", error);
  }
}

sendData();
```

### Full Practical Example with fetch and async/await

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <link rel="stylesheet" href="css/bootstrap.min.css"/>
    <link rel="stylesheet" href="css/style.css"/>
    <title>Fetch Session</title>
  </head>
  <body>
    <section>
      <div class="container">
        <div class="row"></div>
      </div>
    </section>
    <script src="js/bootstrap.bundle.min.js"></script>
    <script src="js/main.js"></script>
  </body>
</html>
```

```jsx
async function getPizza() {
  try {
    var data = await fetch(
      "https://forkify-api.herokuapp.com/api/search?q=pizza",
      {
        method: "GET",
      }
    );
    var finalResponse = await data.json();
    displayPizza(finalResponse.recipes);
  } catch (error) {
    console.log(error);
  }
}

getPizza();

function displayPizza(array) {
  let cards = ``;
  for (let i = 0; i < array.length; i++) {
    cards += `<div class="col-4">
            <div class="card">
              <img src=${array[i].image_url} class="img-fluid" alt="" />
              <p>${array[i].publisher}</p>
              <p>${array[i].title}</p>
            </div>
          </div>`;
  }
  document.querySelector(".row").innerHTML = cards;
}
```

### Breaking This Example Down

1. `getPizza()` is an `async` function, so it can use `await` inside
2. `await fetch(...)` sends the GET request and waits for the server to respond before moving to the next line
3. `await data.json()` waits for the response body to be fully read and converted to a JavaScript object
4. `finalResponse.recipes` is the array of pizza recipes from the API
5. That array is passed to `displayPizza()` which loops through it and builds HTML cards using template literals
6. If anything goes wrong at any point (network error, bad response, etc.), the `catch` block handles it cleanly

---

### Callback vs Promise vs Async/Await — Summary

|  | Callback | Promise | Async/Await |
| --- | --- | --- | --- |
| Introduced | Always existed | ES6 (2015) | ES8 (2017) |
| Readability | Hard (pyramid shape) | Medium (then chains) | Best (reads like sync code) |
| Error handling | Scattered | .catch() | try…catch |
| Recommended? | No (legacy) | Sometimes | Yes, this is the standard |

---

*End of Session 05*