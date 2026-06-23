# JavaScript Session 07 (OOP)

---

## JavaScript is Multi Paradigm

JavaScript supports more than one programming style, known as a “paradigm”. The 2 main ones used in JavaScript are:

1. **Functional Programming**: writing code mainly using functions (this is what was used through most of the previous 6 sessions, things like `map`, `filter`, `reduce`, closures, and higher order functions).
2. **OOP (Object Oriented Programming)**: organizing code around objects that bundle data and behavior together, which is the focus of this entire session.

---

## What is OOP?

OOP is a way of structuring code around **objects**, where each object bundles together its **data** (properties) and the **functions** that operate on that data (methods), instead of keeping data and logic as two completely separate things scattered around the code.

---

## Why We Use OOP

### 1. Simulate Reality

This is the most important reason OOP exists. Anything in real life has 2 things: properties (what it IS) and behaviors (what it DOES). A real person has a name and an age (properties), and can eat, walk, and talk (behaviors). OOP lets you model code the exact same way, by representing real entities as objects that hold both their data and their behavior together.

```jsx
let user = {
  name: "Diaa",
  age: 24,
  login() {
    console.log(this.name + " logged in");
  },
};
```

This `user` object mirrors a real user almost exactly: it has the same properties a real person has, and it can perform the same kind of actions. This makes the code’s structure match how anyone, even a non-developer, would naturally describe the system, which makes the whole project easier to understand, discuss, and maintain.

### 2. Reusable

With OOP, you write a blueprint (a class) once, and then create as many objects as you want from it, or extend it to build more specific versions, without ever rewriting the same logic again.

### 3. Scalable

As a project grows bigger, keeping related data and logic bundled together inside well-defined classes makes it much easier to add new features safely, since changes mostly stay contained inside the relevant class instead of spreading out and breaking unrelated parts of the app.

### 4. Testable

Since a class has clear, well-defined boundaries and a specific responsibility, it becomes much easier to test it on its own. You know exactly what input it expects, and exactly what behavior or output to check for, without needing to run the entire application around it.

---

## 2 Types of OOP

### 1. Class Based (C#, C++, Java)

A **Class** is just a container, a “plan on paper” that defines a set of properties and methods. The class itself is NOT tangible, it’s just a description. The actual tangible thing is the **Object** created from that class.

> The image below shows this visually: the `Table` class is just a written plan describing what a table should have (`capacity`, `color`, `price`), and real, tangible table objects are created from that plan.
> 

![class.JPG](class.jpg)

```jsx
class Table {
  constructor(capacity, color, price) {
    this.capacity = capacity;
    this.color = color;
    this.price = price;
  }
}

let table1 = new Table(4, "Brown", 1500);
let table2 = new Table(2, "White", 800);
```

`Table` itself is just a description, you can’t sit on a class. `table1` and `table2` are the real, tangible objects created from that description.

### 2. Prototype Based (JS, Self)

There’s no “plan on paper” step here. Instead, you start with an actual, already-existing object, and you create new objects directly from that one through a **constructor** (explained in detail later in this session).

> The image below shows this visually: an actual table object already exists, and other objects (a smaller coffee table, a side table) are created directly from that original table object, inheriting its general shape and features.
> 

![prototype.JPG](prototype.jpg)

```jsx
function Table(capacity, color, price) {
  this.capacity = capacity;
  this.color = color;
  this.price = price;
}

let table1 = new Table(4, "Brown", 1500);
let table2 = new Table(2, "White", 800);
```

The code looks almost identical to the class-based example, and that’s exactly the point: JavaScript’s OOP is built on objects and prototypes under the hood, even when it visually looks like a class-based language.

### Quick Comparison

|  | Class Based | Prototype Based |
| --- | --- | --- |
| Languages | C#, C++, Java | JavaScript, Self |
| Starting point | A class (not tangible, just a plan) | An existing object (already tangible) |
| How new objects are made | From a class, using `new` | From another object, through a constructor |

---

## Prototype Inheritance

`Object.setPrototypeOf(who, inheritFrom)` was introduced in ES6 (2015) and is used to make one object inherit properties and methods from another object.

```jsx
let human = {
  isAlive: true,
  canEat() {
    console.log("Yes I Can Eat");
  },
};

let person = {
  name: "Diaa",
  age: 24,
};

Object.setPrototypeOf(person, human);

person.canEat();             // "Yes I Can Eat"
console.log(person.isAlive); // true
```

`person` doesn’t actually have `canEat` or `isAlive` defined directly on itself. Instead, JavaScript walks up the **prototype chain**, and since `person`’s prototype was set to `human`, it finds both of them there and uses them as if they belonged to `person` all along.

### Object.setPrototypeOf Doesn’t Support Multi Inheritance

An object can only have ONE direct prototype at a time, you cannot make one object inherit from 2 different objects directly.

**The Workaround:** chain inheritance through multiple objects, so that inheritance “passes through” indirectly.

```jsx
let object1 = {
  skill1() {
    console.log("Skill 1");
  },
};

let object2 = {
  skill2() {
    console.log("Skill 2");
  },
};

let object3 = {
  skill3() {
    console.log("Skill 3");
  },
};

Object.setPrototypeOf(object2, object1);
Object.setPrototypeOf(object3, object2);

object3.skill1(); // "Skill 1", inherited through object2
object3.skill2(); // "Skill 2", inherited directly from object2
object3.skill3(); // "Skill 3", its own method
```

`object3`’s prototype is `object2`, and `object2`’s prototype is `object1`. So `object3` ends up indirectly inheriting from `object1` too, simply by walking up the chain step by step. This is how JavaScript fakes multi-level inheritance, even though any single object can only have one direct prototype.

### Cyclic Prototype Error

You cannot make `objectA` inherit from `objectB`, while `objectB` also inherits from `objectA` at the same time. This creates a cycle, and JavaScript throws an error, since walking up an endless loop is impossible.

```jsx
let objA = {};
let objB = {};

Object.setPrototypeOf(objA, objB);
Object.setPrototypeOf(objB, objA); // Error: Cyclic __proto__ value
```

In simple terms: a parent cannot inherit from its own child.

### The Old Way: **proto**

```jsx
let human = {
  isAlive: true,
};

let person = {
  name: "Diaa",
};

person.__proto__ = human;

console.log(person.isAlive); // true
```

`__proto__` does the exact same job as `Object.setPrototypeOf()`, but it’s the older, original way of doing it. Modern code generally avoids using `__proto__` directly and prefers `Object.setPrototypeOf()` or `Object.create()` instead.

### **proto** vs prototype (The Famous Confusion)

These two names look almost identical, but they are completely different things. This mix-up is one of the most common sources of confusion for developers learning JavaScript.

- **`prototype`**: only exists on **Functions and Classes**, not on regular objects. Think of it as the “manufacturing catalog” that a function hands down to every object created from it.
- **`__proto__`**: exists on **every single Object**, with no exceptions. Think of it as the “live wire” that an object uses to instantly reach its own prototype, one step up the chain.

```jsx
function Animal(name) {
  this.name = name;
}

console.log(typeof Animal.prototype); // "object", the catalog Animal hands down

let dog = new Animal("Rex");

console.log(Animal.prototype === dog.__proto__); // true, dog's live wire points to Animal's catalog
console.log(dog.prototype);                       // undefined, plain objects don't have a "prototype" property
```

`Animal.prototype` is the catalog that gets handed down to every object `Animal` creates. `dog.__proto__` is the actual wire `dog` uses to reach that exact same catalog. That’s why `Animal.prototype` and `dog.__proto__` end up pointing to the exact same thing.

---

### Every Object Has a Default Prototype

Whenever you create any object in JavaScript, it automatically gets a default prototype set to JavaScript’s built-in `Object`, even if you never wrote anything about prototypes yourself.

```jsx
let person = { name: "Diaa" };

console.log(person.toString()); // works! "toString" wasn't written by you
```

This is exactly why every single object in JavaScript can use methods like `toString()` or `hasOwnProperty()` for free, they’re inherited automatically from this default built-in prototype sitting at the very top of the prototype chain.

### Checking if a Property is Own or Inherited

Since an object can use properties and methods it doesn’t actually have on itself (they come from the prototype chain instead), `hasOwnProperty()` is used to check if a property truly belongs to the object itself, or if it’s just inherited from somewhere up the chain.

This is a very common interview question, since it directly tests if you understand the difference between an object’s own properties and the ones it only has access to through its prototype.

```jsx
function Animal(name) {
  this.name = name;
}

Animal.prototype.sayName = function () {
  console.log(this.name);
};

const dog = new Animal("Rex");

console.log(dog.hasOwnProperty("name"));    // true, "name" was set directly inside the constructor
console.log(dog.hasOwnProperty("sayName")); // false, "sayName" lives on the prototype, not on dog itself
```

`dog` can still call `dog.sayName()` normally and it works fine, since JavaScript walks up the prototype chain to find it. But `hasOwnProperty()` tells you the truth about WHERE that property actually lives: `name` belongs to `dog` directly, while `sayName` is only borrowed from `Animal.prototype`.

---

## Why is typeof null “object”?

This connects directly to the prototype chain concept above. `null` represents the complete absence of any object, the very end of the chain, nothing exists above it to inherit from. Historically, JavaScript marked this by giving `null` the type `"object"`, since `null` is specifically used to **stop the prototype chaining**.

```jsx
console.log(typeof null); // "object"
```

This is widely considered a long standing quirk (some even call it a bug) in JavaScript that was never fixed, because too much existing code across the web already depends on this exact behavior.

---

## Auto Boxing

This explains how primitive values like strings and numbers, despite NOT actually being objects, are still able to use methods like `.toUpperCase()` or `.toFixed()`.

```jsx
let name = "diaa";
console.log(name.toUpperCase()); // "DIAA"
```

`name` is a primitive string, not an object, so it shouldn’t have any methods at all. But the moment you try to call a method on it, JavaScript automatically and temporarily wraps it inside a hidden, invisible `String` object behind the scenes, runs the method using that wrapper, returns the result back to you, then immediately throws the temporary wrapper away. This automatic, invisible wrapping process is called **Auto Boxing**.

```jsx
let age = 24.567;
console.log(age.toFixed(1)); // "24.6"
```

`age` is a primitive `Number`, yet it still has access to `.toFixed()`, for the exact same reason: Auto Boxing temporarily treats it like an object just long enough to run the method.

---

## Constructor Function

A Constructor Function is the original, pre-ES6 way JavaScript creates multiple similar objects from one shared template.

### Rules

1. The function’s name must start with a **Capital letter** (a naming convention that signals “this is a constructor”)
2. Use `this` inside the function to define its properties
3. Always call it using the `new` keyword

### What Does `new` Actually Do?

When you call a function with `new`, 4 things happen automatically, one after another:

1. A brand new, empty object is created
2. `this` inside the function is made to refer to that new object
3. All the code inside the constructor function runs normally
4. The new object is automatically returned, you never need to write `return` yourself

```jsx
function User(userName, userAge, userSalary) {
  this.userName = userName;
  this.userAge = userAge;
  this.userSalary = userSalary;
}

let user1 = new User("Diaa", 24, 15000);
let user2 = new User("sh", 22, 8000);

console.log(user1, user2);
```

### Adding a Method to a Constructor Function

**Important rule:** you are not allowed to write any declaration (`let`, `function`, `const`, etc) directly inside a constructor function. The only thing allowed inside it is `this.something = ...` assignments.

To add a method, you add it to the constructor’s `prototype` instead, outside the function itself.

```jsx
User.prototype.canEat = function () {
  console.log("Can Eat");
};

user1.canEat(); // "Can Eat", inherited through the prototype
```

If the method were written directly inside the constructor function instead, a brand new copy of that exact same function would be created and stored separately for every single object, wasting memory. By placing it on the `prototype` instead, the method is created only ONCE, and every object created from `User` shares that same single copy through the prototype chain.

---

## ES6 Class

Developers coming from other languages (like Java or C#) were not comfortable working directly with constructor functions and prototypes, so in ES6, JavaScript introduced the `class` keyword to give OOP a more familiar, readable syntax.

**Important:** JavaScript still does NOT have “real” classes under the hood. `class` is just new, cleaner syntax sitting directly on top of the exact same old constructor function and prototype system explained above. Nothing fundamental changed internally, only the way it’s written.

### Rules

- The class name must start with a Capital letter
- It must contain a `constructor()` method

```jsx
class User {
  constructor(userName, userAge, userSalary) {
    this.userName = userName;
    this.userAge = userAge;
    this.userSalary = userSalary;
  }

  canEat() {
    console.log("Can Eat");
  }
}

let user1 = new User("Diaa", 24, 15000);
let user2 = new User("sh", 22, 8000);

console.log(user1, user2);
```

Any method written directly inside the class body, like `canEat()` above, is automatically placed onto the class’s prototype behind the scenes, exactly like writing `User.prototype.canEat = function () {}` manually with the old constructor function syntax.

---

## The 4 Pillars of OOP

### 1. Encapsulation

Bundling related data and the methods that operate on that data together inside one single unit (an object or class), and controlling how that data can be accessed or changed from the outside.

```jsx
class BankAccount {
  constructor(balance) {
    this.balance = balance;
  }

  deposit(amount) {
    this.balance += amount;
  }
}

let account = new BankAccount(1000);
account.deposit(500);
console.log(account.balance); // 1500
```

The `balance` lives inside the same object as the logic that’s allowed to change it (`deposit`), instead of being a separate, disconnected variable floating somewhere else in the code.

### 2. Abstraction

Hiding the complex internal details of how something works, and only exposing the simple, necessary parts that the outside world actually needs to interact with.

```jsx
class Car {
  startEngine() {
    this.checkFuel();
    this.igniteSpark();
    console.log("Engine Started");
  }

  checkFuel() {
    // internal detail, hidden from the user
  }

  igniteSpark() {
    // internal detail, hidden from the user
  }
}

let car = new Car();
car.startEngine(); // you only ever need to call this one simple method
```

You don’t need to know anything about `checkFuel()` or `igniteSpark()` to start the car, you simply call `startEngine()`. This is exactly like driving a real car: you press a button, you don’t need to understand what’s happening inside the engine.

### 3. Inheritance

A class can inherit properties and methods from another class, instead of rewriting the exact same code again. This is covered in full detail in the **Class Inheritance** section below.

### 4. Polymorphism (Override & Overload)

Polymorphism has 2 different forms:

**Override:** a child class redefining a method that already exists in its parent class, keeping the same method name but giving it different, more specific behavior. This IS fully supported in JavaScript, through class inheritance (shown in detail in the Class Inheritance section below).

**Overload:** writing the same function/method in more than one version, where each version behaves differently depending on the number or type of arguments passed to it.

**Important note:** JavaScript does NOT truly support Overload, unlike languages such as Java or C#. JavaScript doesn’t check argument types at all, and if you define the same function name twice, only the LAST defined version is ever kept, the earlier one is simply overwritten and lost.

```jsx
function greet() {
  console.log("Hello");
}

function greet(name) {
  console.log("Hello " + name);
}

greet("Diaa"); // "Hello Diaa", only the second version exists at all now
```

### Which Pillars Exist Natively in JavaScript?

| Pillar | Exists in JS? |
| --- | --- |
| Encapsulation | Yes (with true Private support via `#`) |
| Abstraction | Yes (by convention, exposing only what’s needed) |
| Inheritance | Yes (via `extends` and prototypes) |
| Polymorphism (Override) | Yes |
| Polymorphism (Overload) | No, true overloading does not exist in JS |

---

## Access Modifiers

Access Modifiers control who is allowed to access a property or method.

- **Public**: accessible from anywhere. This is the default behavior for everything in JavaScript, unless stated otherwise.
- **Private**: only accessible from inside the class itself, not from outside, and not even from a class that inherits from it. In JavaScript, you mark something as private by putting a `#` directly before its name.
- **Protected**: accessible from inside the class itself AND from any class that inherits from it, but not from outside. JavaScript does not have a real, built-in keyword for Protected like it does for Private (`#`). It’s more of a naming convention developers agree on (often a single underscore `_` prefix) rather than something JavaScript actually enforces.

```jsx
class User {
  #email; // private property

  constructor(name, email) {
    this.name = name;
    this.#email = email;
  }

  #validateEmail() {
    // private method
    console.log("Validating email...");
  }

  showEmail() {
    this.#validateEmail();
    console.log(this.#email);
  }
}

let user = new User("Diaa", "diaaelseady@gmail.com");

user.showEmail();          // works fine, called from inside the class through a public method
console.log(user.#email);  // Error, #email is private and cannot be accessed from outside
```

---

## Class Inheritance

**Important:** just like `Object.setPrototypeOf()` earlier, JavaScript classes do NOT support multi inheritance. A class can only `extend` ONE other class at a time.

Use `extends` to inherit from a parent class, and `super()` to run that parent class’s constructor.

```jsx
class Human {
  constructor(isAlive, breathing) {
    this.isAlive = isAlive;
    this.breathing = breathing;
  }

  canEat() {
    console.log("I Can Eat");
  }
}

class Person extends Human {
  constructor(name, age, salary, isAlive, breathing) {
    super(isAlive, breathing);
    this.name = name;
    this.age = age;
    this.salary = salary;
  }
}

let user = new Person("Diaa", 24, 15000, true, true);
console.log(user);
```

### What Does super() Actually Do?

Calling `super(isAlive, breathing)` runs the constructor of `Human`, the class being extended, exactly as if you had manually written `this.isAlive = isAlive` and `this.breathing = breathing` yourself inside `Person`. `super()` must always be called before using `this` anywhere inside a child class’s constructor, otherwise JavaScript throws an error.

### Overriding a Method (Polymorphism in Action)

```jsx
class Person extends Human {
  constructor(isAlive, breathing) {
    super(isAlive, breathing);
  }

  canEat() {
    console.log("I Can Eat Pizza Specifically");
  }
}

let person = new Person(true, true);
person.canEat(); // "I Can Eat Pizza Specifically", the overridden version runs, not the parent's version
```

`Person` redefines `canEat()` with the exact same name as the one in `Human`, but with completely different behavior. This is a real, practical example of **Override**, one of the 2 forms of Polymorphism explained earlier.

### Private Fields and Inheritance (Shadowing)

A smart question that often comes up: if a child class inherits from a parent, and both define a private field (`#`) with the exact same name, does that cause an error?

```jsx
class Parent {
  #email = "parent@mail.com";
}

class Child extends Parent {
  #email = "child@mail.com"; // does this cause an Error here?
}
```

**The answer: No, this does NOT cause an error.** Unlike some other languages, JavaScript treats `Child`’s `#email` as completely separate and unrelated to `Parent`’s `#email`. This is called **Strict Encapsulation**: each class has its own fully private copy, and `Child` can never reach or touch `Parent`’s `#email`, even though they share the exact same name.

```jsx
class Parent {
  #email = "parent@mail.com";
  showParentEmail() {
    console.log(this.#email);
  }
}

class Child extends Parent {
  #email = "child@mail.com";
  showChildEmail() {
    console.log(this.#email);
  }
}

let child = new Child();

child.showParentEmail(); // "parent@mail.com", Parent's own private copy
child.showChildEmail();  // "child@mail.com", Child's own separate private copy
```

Both private fields exist side by side inside the same object, fully isolated from each other. This is different from a normal (public) property with the same name, where the child’s value would simply override the parent’s.

---

## Getters and Setters

A **Getter** is a special method that runs automatically when you simply read a property, no parentheses needed. A **Setter** is a special method that runs automatically when you assign a value to a property.

Why use them? Because they let you add logic (like validation, or calculating something) while still letting the outside code use simple, normal-looking property syntax (`user.age`) instead of calling a method (`user.getAge()`).

```jsx
class Person {
  constructor(name, age) {
    this.name = name;
    this._age = age; // the real value is stored with an underscore
  }

  // Getter: runs when you READ person.age
  get age() {
    return this._age;
  }

  // Setter: runs when you SET person.age = something
  set age(value) {
    if (value < 0) {
      console.log("Age cannot be negative");
      return;
    }
    this._age = value;
  }
}

let person = new Person("Diaa", 24);

console.log(person.age); // 24, this calls the getter, no () needed
person.age = 22;          // this calls the setter
console.log(person.age);  // 22
person.age = -5;          // "Age cannot be negative", the setter blocks it
```

We stored the real value in `_age` (with an underscore) instead of `age`, because if the property and the getter/setter had the exact same name, JavaScript would create an infinite loop, the getter would keep calling itself forever.

Getters and setters are a clean, simple form of **Encapsulation**: the outside world still writes `person.age`, but internally we fully control what happens when that value is read or changed.

---

## Static Properties and Methods

Normally, every property and method defined in a class belongs to each individual object (each instance) created from it. A **static** member instead belongs to the **class itself**, not to any object made from it. You call it directly on the class name, never on an instance.

Static members are useful for things that don’t need a specific object to make sense, like a counter for how many objects were created, or a small helper function related to the class.

```jsx
class Person {
  static totalPeople = 0; // static property

  constructor(name) {
    this.name = name;
    Person.totalPeople++; // every time a new Person is made, increase the static counter
  }

  static greet() {
    // static method
    console.log("Hello from the Person class");
  }
}

let person1 = new Person("Diaa");
let person2 = new Person("Sh");

console.log(Person.totalPeople); // 2, called on the class itself
Person.greet();                  // "Hello from the Person class"

console.log(person1.totalPeople); // undefined, instances cannot access static members
```

`totalPeople` is shared by the whole class, not copied for every object. That’s why it correctly tracks the total count across all instances, instead of each object having its own separate counter stuck at 1.

---

## instanceof Operator

`instanceof` checks whether an object was created from a specific class (or, more precisely, whether that class appears anywhere in the object’s prototype chain). It returns `true` or `false`.

```jsx
class Human {}
class Person extends Human {}

let person = new Person();

console.log(person instanceof Person); // true, made directly from Person
console.log(person instanceof Human);  // true, Person extends Human, so it's true too
console.log(person instanceof Array);  // false, not related at all
```

This is commonly used to check what type of object you’re dealing with before deciding what to do with it, especially useful when working with inheritance, since `instanceof` checks the entire chain, not just the direct class.

---

## Object.create()

`Object.create(proto)` creates a brand new, empty object and directly sets its prototype to whatever object you pass in. It does the same job as `Object.setPrototypeOf()`, but in one single step instead of two (create the object, then set its prototype separately).

```jsx
let human = {
  isAlive: true,
  canEat() {
    console.log("Yes I Can Eat");
  },
};

let person = Object.create(human);
person.name = "Diaa";

console.log(person.name);    // "Diaa", set directly on person
console.log(person.isAlive); // true, inherited from human through the prototype
person.canEat();              // "Yes I Can Eat", inherited too
```

This is considered the cleaner, more modern way to create an object with a specific prototype, compared to creating a plain object first and assigning its prototype afterward with `Object.setPrototypeOf()`.

---

## Composition over Inheritance

Inheritance (`extends`) is not always the best tool. It creates a tight, rigid connection between a parent and child class, and forcing every shared behavior into one parent class can become messy as a project grows (this is sometimes called a fragile hierarchy).

**Composition** means building an object by combining small, independent pieces of behavior together, instead of inheriting everything from one single parent class.

```jsx
// small, independent pieces of behavior
let canEat = {
  eat() {
    console.log(this.name + " is eating");
  },
};

let canWalk = {
  walk() {
    console.log(this.name + " is walking");
  },
};

// combine the pieces into one object
let person = Object.assign({ name: "Diaa" }, canEat, canWalk);

person.eat();  // "Diaa is eating"
person.walk(); // "Diaa is walking"
```

Instead of saying “a `Person` IS a `Human`” (inheritance), composition says “a `Person` HAS the ability to eat, and HAS the ability to walk” (composition). This is more flexible, because you can mix and match small pieces of behavior freely, without being locked into one rigid parent class chain.

A simple rule many developers follow: use **Inheritance** when there’s a true, clear “IS A” relationship (a `Car` IS A `Vehicle`). Use **Composition** when you just want to share some behavior between unrelated things, without a real “IS A” relationship.

---

## this Inside Class Methods

`this` inside a normal class method refers to whatever object is calling that method. But if you take the method out of the object and call it on its own, `this` can be lost, since it depends on HOW the function is called, not where it was defined.

```jsx
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log("Hello, " + this.name);
  }
}

let person = new Person("Diaa");

person.greet(); // "Hello, Diaa", works fine, called directly on person

let greetFunction = person.greet;
greetFunction(); // TypeError: Cannot read properties of undefined, this is now lost
```

This commonly happens by accident when passing a method as a callback, for example to `setTimeout` or to an event listener, since the method gets called later, separately from the object.

**The fix:** use an **arrow function** instead of a normal method, since arrow functions don’t have their own `this`. They simply use the `this` of whatever is around them when they were created (this is called the surrounding, or lexical, scope).

```jsx
class Person {
  constructor(name) {
    this.name = name;

    // arrow function as a class property, this stays locked to the object
    this.greet = () => {
      console.log("Hello, " + this.name);
    };
  }
}

let person = new Person("Diaa");
let greetFunction = person.greet;

greetFunction(); // "Hello, Diaa", works correctly now
```

---

## Abstract Classes (Simulated)

An **Abstract class** is a class that is never meant to be used directly to create objects from. It only exists to be extended by other classes, which are then required to implement certain methods themselves.

JavaScript has no real, built-in keyword for this (unlike some other languages). Instead, developers simulate it using a check inside the constructor.

```jsx
class Shape {
  constructor() {
    if (this.constructor === Shape) {
      throw new Error("Shape is abstract and cannot be created directly");
    }
  }

  getArea() {
    throw new Error("getArea() must be implemented by the child class");
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }

  getArea() {
    return 3.14 * this.radius * this.radius;
  }
}

let shape = new Shape();  // Error: Shape is abstract and cannot be created directly
let circle = new Circle(2);
console.log(circle.getArea()); // 12.56, Circle properly implements getArea()
```

`this.constructor` always points to the actual class that was used with `new`. So if someone tries `new Shape()` directly, `this.constructor` equals `Shape` itself, and the error fires. But if someone creates a `Circle`, `this.constructor` equals `Circle`, not `Shape`, so no error happens, and the object is created normally.

---

*End of Session 07*