# js-fundamentals-assignment

# section-A

## A1. Difference between var, let, and const

- `var` has function scope, while `let` and `const` have block scope.
- All three are hoisted, but `var` is initialized with `undefined`.
- `let` and `const` stay in the Temporal Dead Zone (TDZ) until they are declared.
- `var` can be re-declared and re-assigned.
- `let` can be re-assigned but cannot be re-declared in the same scope.
- `const` cannot be re-assigned or re-declared. However, if it stores an object or array, its properties can still be changed.
- In modern JavaScript, developers usually prefer `const` by default and use `let` only when the value needs to change. `var` is generally avoided.
Example:
```js
let age = 20;
age = 21; // allowed

const name = "Ali";
// name = "Sara"; // Error
```
### Which one to use in modern JS and why?
- Always prefer `const` by default — it signals intent that the value won't be reassigned, preventing accidental bugs.
- Use `let` only when you know the variable will be reassigned.
- Avoid `var` entirely because function scope leaks and hoisting to `undefined` creates hard-to-debug issues.

---

## A2. What is the V8 Engine? What does single-threaded mean?

- V8 is the JavaScript engine created by Google.
- It is used in Chrome and also powers Node.js.
- V8 uses Just-In-Time (JIT) compilation, which means it quickly converts JavaScript into machine code while the program runs.
- JavaScript is called single-threaded because it uses one main call stack and executes one task at a time.
- Asynchronous work like `setTimeout` and `fetch` is handled by Web APIs or Node.js features. When finished, callbacks go to the Callback Queue and the Event Loop sends them to the Call Stack when it is free.
- Node.js JavaScript execution is also single-threaded, but Node uses libuv (multi-threaded C++) behind the scenes to handle I/O operations efficiently.
Example:
```js
console.log("Start");

setTimeout(() => {
  console.log("Done");
}, 1000);

console.log("End");
```
### How JS handles async despite being single-threaded:
1. **Call Stack**: Where synchronous code executes (one function at a time).
2. **Web APIs**: Browser features (setTimeout, fetch, DOM events) that run outside the main thread.
3. **Callback Queue**: When async operations complete, their callbacks wait here.
4. **Event Loop**: Continuously checks if the Call Stack is empty; if yes, pushes callbacks from the Queue to the Stack.
---

## A3. JavaScript Data Types and Type Coercion

JavaScript has 8 data types.

Primitive types:
- String
- Number
- Boolean
- Undefined
- Null
- BigInt
- Symbol

Non-primitive:
- Object (including arrays and functions)

`typeof null` returns `"object"`. This is a well-known old JavaScript bug kept for compatibility.
### Why is typeof null === 'object' a bug?
In 1995 when JavaScript was created, computers stored types as binary numbers. Null was stored as all zeros (000...). Objects were also marked with zeros. So the computer got confused between null and objects. This was never fixed so old websites wouldn't break.

### Why == is dangerous:
- 0 == false → true (wrong!)
- "" == false → true (wrong!)
- null == undefined → true (confusing!)

=== checks both value AND type — much safer
Implicit coercion happens automatically.

Example:
```js
"5" + 2     // "52"
"5" * 2     // 10
```

Explicit coercion is done by the programmer.

Example:
```js
Number("25")   // 25
String(100)    // "100"
Boolean(1)     // true
```
```
Boolean("") // false
```
```
5 == "5"   // true
5 === "5"  // false
```

`==` performs type conversion before comparing values, while `===` compares both value and type, so `===` is usually safer.

---

## A4. Primitive vs Non-Primitive Data Types

Primitive values are:
- String
- Number
- Boolean
- Undefined
- Null
- BigInt
- Symbol

They are copied by value.

Non-primitive values include:
- Objects
- Arrays
- Functions

These are copied by reference.
Primitive values are usually stored in stack memory, while objects, arrays, and functions are stored in heap memory. When a primitive is copied, a new value is created. When an object or array is copied, the reference is copied, so both variables point to the same data.
Example:
```js
const user1 = { name: "Ali" };
const user2 = user1;
user2.name = "Sara";
console.log(user1.name); // "Sara"
```
###  Why two different memory types?
- **Stack** is small and fast — primitives are small (numbers, text) so they fit here easily
- **Heap** is big and flexible — objects and arrays can grow (you can add more items), so they need more space
```

```
## A5. Pass by Value vs Pass by Reference

Primitive values behave like pass by value.JavaScript is not truly pass by reference. It passes the reference by value. This means changing an object's properties affects the original object, but assigning the parameter to a completely new object does not change the original

Example:
```js
function change(x) {
  x = 50;
}

let num = 10;
change(num);
console.log(num); // 10
```

Objects behave differently because JavaScript passes the reference by value.JavaScript copies the **address** (location) of the object, not the object itself.
- If you change what's inside the object → original changes too (both point to same place)
- If you replace the whole object → original does NOT change (only your local copy changes)

Changing object properties affects the original object.

```js
function update(user) {
  user.name = "Sara";
}

const person = { name: "Ali" };
update(person);
console.log(person.name); // "Sara"

The parameter receives a copy of the reference. Reassigning the parameter creates a new object and does not affect the original object.
```
function replaceUser(user) {
  user = { name: "Sara" };
}
const person = { name: "Ali" };
replaceUser(person);
console.log(person.name); // "Ali"

---

## A6. What is a Function?

A function is a reusable block of code that performs a specific task. It helps avoid repeating code.

Syntax:

```js
function greet(name) {
  return "Hello " + name;
}
```

Function declarations are hoisted, so they can be called before they are written.

A parameter is the variable written in the function definition, while an argument is the actual value passed when the function is called. An argument is the actual value passed when calling the function.

If a function does not have a return statement, it automatically returns undefined

Example:

```js
function checkAge(age) {
  if (age >= 18) {
    return "Allowed";
  }

  return "Not Allowed";
}

console.log(checkAge(20));
```

Functions are special objects in JavaScript. They can have properties and methods.
```
Example:

function greet(name) {
  return `Hello ${name}`;
}

console.log(typeof greet);           // "function"
console.log(greet instanceof Object); // true
console.log(greet.name);             // "greet"
console.log(greet.length);           // 1

```

