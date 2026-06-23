# JavaScript Session 04

---

## getAttribute / setAttribute

These two methods let you read or change the value of an HTML attribute directly through JavaScript.

### getAttribute

Reads the current value of an attribute on an element.

```jsx
let link = document.querySelector("a");
console.log(link.getAttribute("href")); // "https://example.com"
```

### setAttribute

Sets or changes the value of an attribute on an element.

```jsx
let link = document.querySelector("a");
link.setAttribute("href", "https://google.com");
link.setAttribute("target", "_blank");
```

This works on any attribute — `src`, `alt`, `class`, `id`, custom data attributes, and more.

```jsx
let img = document.querySelector("img");
img.setAttribute("src", "newImage.jpg");
img.setAttribute("alt", "A new image");
```

---

## Relative Path vs Absolute Path

When linking to a file (image, page, script), the path can be written in two ways.

### Relative Path

Points to a file based on the location of the **current file**. It changes depending on where you are in the folder structure.

*(Short Path)

```html
<img src="images/logo.png">      <!-- inside a folder called "images" -->
<img src="../images/logo.png">   <!-- go up one folder, then into "images" -->
```

### Absolute Path

Points to the exact full address of a file, starting from the root of the website or even the full URL — it does not depend on where the current file is located.

*(Full Path)

```html
<img src="/images/logo.png">                    <!-- starts from website root -->
<img src="https://example.com/images/logo.png"> <!-- full URL -->
```

**Rule of thumb:** Relative paths are common for files within your own project. Absolute paths are common when linking to external websites or resources.

---

## Writing CSS Inside JavaScript

You can apply CSS styles directly to an element using JavaScript in two ways.

### Way 1: style.cssText

Write multiple CSS properties at once as a single string, exactly like writing CSS.

```jsx
let box = document.querySelector(".box");
box.style.cssText = "background-color: red; padding: 20px; border-radius: 10px;";
```

### Way 2: Individual Style Properties

Set one CSS property at a time. Note that CSS properties with a dash (like `background-color`) are written in camelCase in JavaScript.

```jsx
let box = document.querySelector(".box");
box.style.backgroundColor = "red";
box.style.padding = "20px";
box.style.borderRadius = "10px";
```

---

## DOM Traversing

DOM Traversing means moving between elements based on their relationship to each other in the DOM tree — like moving to a parent, a child, or a sibling element — without needing to select them directly with a new query.

### Moving to Children

```jsx
let div = document.querySelector("div");

div.children;             // returns all direct children (HTMLCollection, elements only)
div.children[3].classList.add("bg-danger"); // adds a class to the 4th child (index 3)

div.childNodes;            // returns ALL child nodes, including text nodes (NodeList)

div.firstElementChild;     // grabs the FIRST child element
div.lastElementChild;      // grabs the LAST child element
```

### Moving to Parent

```jsx
let child = document.querySelector(".child");

child.parentElement;       // grabs the direct parent element
```

### Moving to Siblings

A sibling is an element at the same level in the tree (same parent).

```jsx
let div = document.querySelector("div");

div.nextElementSibling;     // grabs the element directly AFTER it (like CSS: div + h1)
div.previousElementSibling; // grabs the element directly BEFORE it
```

### Quick Summary Table

| Property | What it grabs |
| --- | --- |
| `.children` | All direct child elements |
| `.childNodes` | All child nodes (elements + text) |
| `.firstElementChild` | First child element |
| `.lastElementChild` | Last child element |
| `.parentElement` | The direct parent element |
| `.nextElementSibling` | The next element at the same level |
| `.previousElementSibling` | The previous element at the same level |

---

## Creating Elements Dynamically

Sometimes you need to build a brand-new HTML element from scratch using JavaScript, instead of just editing an existing one.

### document.createElement()

Creates a new HTML element in memory (not yet visible on the page).

```jsx
let newDiv = document.createElement("div");
```

### document.createTextNode()

Creates a text node that can be placed inside an element.

```jsx
let text = document.createTextNode("Hello World");
```

### appendChild()

Attaches a new element (or text node) as the last child inside another element, making it actually appear on the page.

```jsx
let newDiv = document.createElement("div");
let text = document.createTextNode("Hello World");

newDiv.appendChild(text);              // put the text inside the new div
document.body.appendChild(newDiv);     // put the new div inside the body
```

This is the manual, step-by-step way to build HTML elements completely through JavaScript.

---

## setInterval and setTimeout

Both are used to run code after a delay, but they behave differently.

### setInterval

Runs a piece of code repeatedly, again and again, after every X milliseconds, until you manually stop it.

```jsx
setInterval(function () {
  console.log("Hello");
}, 2000);
// Runs every 2000ms (2 seconds), forever
```

**Example — counting and stopping after a limit:**

```jsx
var count = 0;
var counter = setInterval(function () {
  count++;
  console.log(count);
  if (count >= 20) {
    clearInterval(counter); // stops the interval once count reaches 20
  }
}, 1000);
```

`clearInterval()` is how you manually stop a `setInterval` from continuing to run. You must pass it the variable that stored the interval.

### setTimeout

Works the same way as `setInterval`, but it only runs the code **one single time** after the delay — it does not repeat.

```jsx
setTimeout(function () {
  console.log("This runs once after 2 seconds");
}, 2000);
```

### Real Example: Auto-Hiding Error Message (Pauses on Hover)

This is a common real-world UI pattern: show an error message that disappears automatically after a few seconds, but pause the timer while the user is hovering over it.

```html
<p class="alert alert-danger">
  Lorem ipsum dolor sit amet consectetur adipisicing elit. Magni, ullam?
</p>
```

```jsx
var errorAlert = document.querySelector(".alert");

var errorCount = setTimeout(function () {
  errorAlert.classList.add("d-none");
}, 1000);

// When the mouse enters the alert, stop the countdown
errorAlert.addEventListener("mouseenter", function () {
  clearTimeout(errorCount);
});

// When the mouse leaves, restart the countdown again
errorAlert.addEventListener("mouseleave", function () {
  setTimeout(function () {
    errorAlert.classList.add("d-none");
  }, 1000);
});
```

How it works: a `setTimeout` is set to hide the alert after 1 second. If the user hovers over it before the time runs out, `clearTimeout()` cancels that countdown so the message doesn’t disappear while they’re reading it. Once they move the mouse away, a brand-new `setTimeout` starts the countdown again.

---

## Try…Catch Statement

`try...catch` lets you run code that might fail, and handle the error gracefully instead of crashing the whole program.

```jsx
try {
  // code that might cause an error
  let result = someUndefinedFunction();
} catch (error) {
  // this runs only if an error happens above
  console.log("Something went wrong:", error.message);
}
```

```jsx
try {
  let user = JSON.parse("this is not valid JSON");
} catch (error) {
  console.log("Failed to parse data:", error.message);
}
console.log("The program keeps running normally after this");
```

Without `try...catch`, an error would stop the entire script from running any further. With it, JavaScript “catches” the error, runs your fallback code, and continues normally.

---

## API — Application Programming Interface

An API is a bridge that allows different applications to communicate with each other and exchange data — usually between a frontend application and a backend server connected to a database.

### How It Works

![preview.webp](preview.webp)

Looking at the diagram: no matter what type of application you’re building — Web (HTML, CSS, JS), Android (Java), iOS (Swift), or Desktop (C++, JS) — they all need a way to talk to the same database. That’s exactly what the API does. The frontend sends data (like name, email, password) to the API, the API forwards it to the database, and the database sends back a response (like “Success” or “Registered”) that travels back through the API to the app.

This means the same API can serve completely different types of applications at once, because they all just need to know how to talk to it — they don’t need to know how the database itself works internally.

### Backend Documentation

The backend team creates something called **documentation** — a guide that explains exactly how to use their API: what links are available, what data to send, and what data you’ll get back. As a frontend developer, this documentation is your main reference for connecting to any API.

Example of API documentation:
[https://documenter.getpostman.com/view/49715513/2sBXqGqM2x](https://documenter.getpostman.com/view/49715513/2sBXqGqM2x)

### JSON Formatter (Chrome Extension)

When you open an API link directly in the browser, the response usually appears as a messy, unformatted block of text. The **JSON Formatter** extension automatically organizes it into a clean, readable, color-coded, collapsible structure — making it much easier to read and explore.

Link
[https://chromewebstore.google.com/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa?hl=en](https://chromewebstore.google.com/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa?hl=en)

---

### API Responses: Array of Objects or Object

An API typically returns data in one of two shapes:

```jsx
// Array of Objects — multiple items, like a list of products
[
  { "id": 1, "name": "Cat Food" },
  { "id": 2, "name": "Dog Toy" }
]

// Single Object — one item, like one user's profile
{ "id": 1, "name": "Diaa" "email": "diaa@example.com" }
```

Example API to explore this live:
[https://my-pet-store-api.vercel.app/products](https://my-pet-store-api.vercel.app/products)

---

## API Link Structure: baseURL + Endpoint

```
baseURL / Endpoint
```

The **baseURL** is the root address of the API and it never changes. The **Endpoint** is the specific path that points to a specific resource or action.

```
https://my-pet-store-api.vercel.app   ← baseURL (never changes)
/products                              ← Endpoint (changes based on what you need)
```

---

## 4 Ways to Send Data to the Backend

### 1. Path Parameter

Data sent as part of the URL path itself, usually to identify a specific item.

```jsx
https://my-pet-store-api.vercel.app/products/5
```

Here, `5` is a path parameter — likely the ID of a specific product.

### 2. Query Parameters

Extra data added to the end of the URL after a `?`, used for filtering, searching, or sorting. Multiple query parameters are joined with `&`.

```jsx
https://my-pet-store-api.vercel.app/products/filter?category=cats&name=royal
```

Breaking this down:
- `baseURL` → `https://my-pet-store-api.vercel.app`
- `Endpoint` → `/products/filter`
- `Query Parameters` → `?category=cats&name=royal`

### 3. Headers

Extra information sent along with the request, but not visible in the URL — commonly used for things like authentication tokens or specifying the data format.

```jsx
headers: {
  "Authorization": "Bearer your-token-here",
  "Content-Type": "application/json"
}
```

### 4. Body

The actual data payload sent with the request, typically used with `POST`, `PUT`, or `PATCH` to create or update something.

```jsx
body: JSON.stringify({
  name: "New Cat Food",
  category: "cats",
  price: 50
})
```

---

## API Methods (HTTP Methods)

| Method | Purpose |
| --- | --- |
| GET | Retrieve/read data 
*Default Browser Method |
| POST | Send/create new data |
| PUT | Update existing data (usually replaces the whole item) |
| PATCH | Update only a specific part of existing data |
| DELETE | Delete data |

```jsx
GET    https://my-pet-store-api.vercel.app/products       → get all products
GET    https://my-pet-store-api.vercel.app/products/5     → get one specific product
POST   https://my-pet-store-api.vercel.app/products       → create a new product
PUT    https://my-pet-store-api.vercel.app/products/5     → fully update product 5
PATCH  https://my-pet-store-api.vercel.app/products/5     → partially update product 5
DELETE https://my-pet-store-api.vercel.app/products/5     → delete product 5
```

---

## Postman

Postman is a tool used to test APIs without needing to build a frontend interface first. It lets you send requests (GET, POST, PUT, DELETE, etc.) directly to an API and see the response immediately.

### Why Frontend Developers Use Postman

- Test if an API endpoint works correctly before connecting it to actual frontend code
- See the exact shape and structure of the data the API returns
- Try sending data (body, headers, query parameters) and check the response without writing any JavaScript
- Read and explore shared API documentation that backend teams build using Postman
- Save time debugging — if something breaks, you can check directly in Postman whether the problem is in the API itself or in your frontend code

---

## AJAX — Asynchronous JavaScript and XML

AJAX is a technique that allows JavaScript to send and receive data from a server **without reloading the entire page**. Despite the name mentioning XML, today it’s almost always used with JSON instead.

### XMLHttpRequest()

This is the original built-in JavaScript object used to make AJAX requests. The steps to use it are:

1. Create a new request object: `new XMLHttpRequest()`
2. Open a connection: `.open("Method", "API Link")`
3. Send the request: `.send()`
4. Set the response type: `.responseType = "json"`
5. Listen for the response using an event — either:
    - `readystatechange` → check `if (readyState == 4 && status == 200)`
    - OR `load` → check `if (status == 200)`
6. Optionally listen for `error` to catch failed requests

### Step-by-Step Example

```jsx
var api = new XMLHttpRequest();
api.open("GET", "Api Link");
api.send();

api.addEventListener("readystatechange", function () {
  if (api.readyState == 4) {
    console.log(JSON.parse(api.response));
  }
});
```

Explanation: `readyState` tracks the current stage of the request (0 = not started, all the way to 4 = request fully complete and response is ready). We check for `4` to make sure the entire response has actually arrived before trying to use it. Since the raw response comes back as a string, `JSON.parse()` converts it into a usable object or array.

### Best Way: Using “load” Instead

```jsx
var api = new XMLHttpRequest();
api.open("GET", "api link");
api.send();

api.addEventListener("load", function () {
  if (api.status == 200) {
    console.log(JSON.parse(api.response));
  }
});
```

The `load` event is simpler and more commonly used because it only fires once the request is fully complete — so you don’t need to manually check `readyState`. You only need to check `status == 200` to confirm the request succeeded.

---

## HTTP Status Codes (Simplified)

Status codes tell you whether a request succeeded or failed, and roughly why.

| Range | Meaning |
| --- | --- |
| 200s | Success — everything worked (e.g. `200 OK`, `201 Created`) |
| 300s | Redirection — the resource moved somewhere else |
| 400s | Client Error — something wrong with your request (e.g. `404 Not Found`, `400 Bad Request`, `401 Unauthorized`) |
| 500s | Server Error — something wrong on the server’s side (e.g. `500 Internal Server Error`) |

**Simple rule:** 200s = good, 400s = you made a mistake, 500s = the server made a mistake.

---

## Full Practical Example: Fetching and Displaying API Data

```html
<section>
  <div class="container">
    <div class="row"></div>
  </div>
</section>
```

```jsx
function apis() {
  var api = new XMLHttpRequest();
  api.open("GET", "https://forkify-api.herokuapp.com/api/search?q=pizza");
  api.send();

  api.addEventListener("load", function () {
    display(JSON.parse(api.response).recipes);
  });
}

apis();

function display(array) {
  var cards = "";
  for (var i = 0; i < array.length; i++) {
    cards += `<div class="col-md-3">
            <div class="inner position-relative">
              <img src="${array[i].image_url}" alt="" class="img-fluid" />
              <div class="text position-absolute bottom-0 text-center text-white start-50 translate-middle-x">
                <p class="m-0">${array[i].publisher}</p>
                <p class="m-0">${array[i].title}</p>
              </div>
            </div>
          </div>`;
  }
  document.querySelector(".row").innerHTML = cards;
}
```

### Breaking This Example Down

1. `apis()` creates a request to search for “pizza” recipes using a query parameter (`?q=pizza`)
2. Once the data loads successfully, it calls `display()` and passes in the array of recipes from the response
3. `display()` loops through every recipe in the array using a `for` loop
4. On each loop, it builds an HTML card using a template literal, inserting the recipe’s image, publisher, and title directly using `${}`
5. All the generated cards get joined together into one big string (`cards`)
6. After the loop finishes, that entire string is injected into the page at once using `innerHTML`

This is the complete real-world cycle: request data from an API → wait for it to load → loop through the results → dynamically build and inject HTML based on that data.

---

*End of Session 04*