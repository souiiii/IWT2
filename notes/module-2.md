# Module II: JavaScript, HTML DOM and BOM

These notes cover the complete Module II syllabus, including the advanced JavaScript topics on the next syllabus page. Prepare both explanations and programs: the mostly-theoretical instruction for Module III does **not** apply here.

**Contents:** [JavaScript basics](#1-javascript-basics) · [Built-in objects](#2-strings-numbers-arrays-and-other-built-in-objects) · [DOM and forms](#3-the-html-dom-documents-events-and-forms) · [BOM and cookies](#4-the-browser-object-model-bom) · [Advanced JavaScript](#5-advanced-javascript-concepts) · [Programming practice](#6-two-additional-pyq-programming-patterns) · [Revision and sources](#7-exam-preparation-and-source-basis)

Use each code block as a separate example unless it explicitly continues an earlier one. Browser examples that refer to page elements assume that those elements already exist, usually because the script appears after the HTML or uses `defer`.

# 1. JavaScript basics

## 1.1 What JavaScript does

**JavaScript** is a programming language used to make web pages respond to user actions. It can validate a form, calculate a result, change page content or styles, and communicate with a server.

HTML provides the page structure, CSS controls its appearance, and JavaScript controls behaviour. JavaScript is dynamically typed: a variable can hold values of different types during execution. It supports objects, functions and event-driven programming, where code responds to actions such as clicks.

### JavaScript versus Java

| JavaScript | Java |
| --- | --- |
| Dynamically typed. | Statically typed. |
| Uses prototype-based inheritance; also offers class syntax. | Uses class-based inheritance. |
| Runs in a JavaScript engine, including engines inside browsers. | Usually runs as bytecode on the JVM. |
| Commonly controls web-page behaviour through browser APIs. | Does not directly manipulate a browser's HTML DOM in ordinary Java programs. |

They are different languages despite their similar names. JavaScript is not limited to browsers, although browser scripting is the focus of this module.

### Adding JavaScript to HTML

An internal script is placed inside `<script> ... </script>`. An external script is linked as follows:

```html
<script src="app.js" defer></script>
```

`src` gives the file location. For an external classic script, `defer` allows HTML parsing to continue and runs the script after parsing; deferred scripts retain their document order. `async` runs a downloaded script as soon as it is ready, without guaranteeing order. `type="module"` selects JavaScript module behaviour; ordinary JavaScript does not require a `type` attribute. The old `language` attribute is unnecessary.

## 1.2 Statements, comments and values

A **statement** performs an action, such as declaring a variable or calling a function. An **expression** produces a value, such as `price * quantity`. Braces group statements into a block.

```javascript
const price = 50;             // A single-line comment.
let quantity = 3;
const total = price * quantity;
/* A comment can span
   more than one line. */
console.log(total);           // 150
```

JavaScript is case-sensitive: `total` and `Total` are different names. Use semicolons consistently rather than depending on automatic insertion.

The primitive types are **string, number, boolean, undefined, null, bigint and symbol**. Objects include arrays, dates and functions. For normal exam programs, strings, numbers, booleans, objects and arrays are the most important.

- `undefined` commonly means no value has been assigned or returned.
- `null` is an explicit value meaning “no value” or “no object.”
- `typeof 12` is `"number"`; `typeof "12"` is `"string"`.
- `typeof null` is historically `"object"`, even though `null` is a primitive. Use `Array.isArray(value)` to identify an array.

## 1.3 Variables, scope, `var`, `let` and `const`

A variable gives a value a name. Its **scope** is the part of a program in which that name can be accessed.

| Feature | `var` | `let` | `const` |
| --- | --- | --- | --- |
| Main local scope | Function scope. | Block scope. | Block scope. |
| Reassign a value? | Yes. | Yes. | No. |
| Redeclare in the same scope? | Generally allowed for another `var`. | No. | No. |
| Initial value required? | No. | No. | Yes. |
| Access before declaration | Reads as `undefined`, before assignment. | Throws a `ReferenceError`. | Throws a `ReferenceError`. |

Function scope means the name is available throughout its containing function. Block scope confines the name to its `{ ... }` block and nested blocks.

```javascript
function scopeDemo() {
  if (true) {
    var a = 10;
    let b = 20;
  }
  console.log(a);           // 10
  // console.log(b);        // ReferenceError: b is outside its scope.
}
scopeDemo();
```

Prefer `const` when the binding will not change and `let` when reassignment is required. A constant binding does not make an object immutable:

```javascript
const student = { name: "Asha" };
student.name = "Ravi";      // Allowed: changes the existing object.
// student = {};           // Not allowed: replaces the binding.
```

### Hoisting and the temporal dead zone

**Hoisting** describes how declarations affect their scope before execution reaches their written position. The engine does not literally move source lines.

```javascript
console.log(score);        // undefined
var score = 10;

console.log(add(2, 3));     // 5
function add(a, b) { return a + b; }
```

The `var` binding exists before its assignment, and a function declaration is available with its function body. `let`, `const` and class declarations cannot be accessed before initialization. This period is called the **temporal dead zone**. A function expression assigned to `const` is therefore not callable before that declaration executes. See [MDN's explanation of let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let).

## 1.4 Operators and type conversion

| Group | Operators and use |
| --- | --- |
| Arithmetic | `+`, `-`, `*`, `/`, `%` (remainder), `**` (power). |
| Assignment | `=`, `+=`, `-=`, `*=`, `/=` update a binding. |
| Comparison | `<`, `<=`, `>`, `>=`, `===`, `!==`, `==`, `!=`. |
| Logical | `&&` (and), `\|\|` (or), `!` (not). |
| Increment/decrement | `++`, `--` add or subtract one. |
| Conditional | `condition ? valueIfTrue : valueIfFalse`. |
| Type check | `typeof value`. |

`===` compares without converting types: `5 === "5"` is false, while `5 == "5"` is true. Prefer strict equality to avoid surprising conversions. `+` also joins strings: `"5" + 2` produces `"52"`, but `Number("5") + 2` produces `7`.

For `let x = 3`, `x++` produces the old value and then increments; `++x` increments first and produces the new value. Use parentheses to make complex expressions clear.

Falsy values include `false`, `0`, `""`, `null`, `undefined` and `NaN`. Non-empty strings, including `"false"`, are truthy. Logical operators short-circuit: in `a && b`, `b` is evaluated only if `a` is truthy. They return operand values, not necessarily booleans.

**Form values and prompt results are strings.** Check for empty input or cancellation before conversion: `Number("")` and `Number(null)` both produce `0`.

## 1.5 Decisions and loops

Use `if`/`else` to choose between conditions and `switch` to choose among fixed alternatives.

```javascript
const marks = 72;
let grade;
if (marks >= 80) grade = "A";
else if (marks >= 60) grade = "B";
else grade = "C";

const operation = "add";
let result;
switch (operation) {
  case "add": result = 2 + 3; break;
  case "multiply": result = 2 * 3; break;
  default: result = "Unknown operation";
}
```

`break` exits the switch or nearest loop. Without a switch `break`, execution can continue into the next case. `continue` skips the rest of the current loop iteration.

| Loop | When to use it |
| --- | --- |
| `for (let i = 0; i < n; i++)` | When initialization, condition and update belong together. |
| `while (condition)` | Repeat while a condition holds; may execute zero times. |
| `do { ... } while (condition)` | Execute at least once, then test. |
| `for (const value of values)` | Read values from an iterable, such as an array or string. |
| `for (const key in object)` | Enumerate enumerable property names, including inherited ones. |

For arrays, use `for...of` for values or a normal `for` loop for indexes. Do not confuse `for...in` property names with array values.

## 1.6 Functions and return values

A **function** groups reusable steps. Parameters are names in the definition; arguments are values supplied in a call. `return` sends a value back and ends the function. Without an explicit return value, the result is `undefined`.

```javascript
function area(width, height = 1) {
  return width * height;
}
const square = function (n) { return n * n; }; // Function expression.
const double = n => n * 2;                    // Arrow function.
console.log(area(4, 3), square(5), double(6)); // 12 25 12
```

An arrow with an expression body returns that expression automatically. With a block body, write `return` explicitly. Arrow functions also differ in their handling of `this` (§1.7).

Primitive arguments are passed as values. When an object is passed, the parameter receives a value referring to that object: changing a property affects the shared object, but assigning a different object to the parameter does not reassign the caller's variable.

### Factorial from a prompt — a recurring programming pattern

```javascript
function factorial(n) {
  let product = 1;
  for (let i = 2; i <= n; i++) product *= i;
  return product;
}

const input = prompt("Enter an integer from 0 to 18:");
if (input !== null && input.trim() !== "") {
  const n = Number(input);
  if (Number.isInteger(n) && n >= 0 && n <= 18) {
    alert(factorial(n));
  } else {
    alert("Enter a whole number from 0 to 18.");
  }
}
```

The product starts at `1`, so `0!` and `1!` correctly produce `1`. The small input range keeps the result within JavaScript's safe integer range. A recursive version would return `1` for the base case and otherwise calculate `n * factorial(n - 1)`; every recursive solution needs a terminating base case.

## 1.7 Objects, methods and `this`

An **object** groups related properties. A property whose value is a function is a **method**.

```javascript
const student = { name: "Asha", marks: 80 };
student.describe = function () {           // Add a method to an object.
  return this.name + " scored " + this.marks;
};
console.log(student.describe());           // Asha scored 80
console.log(student["name"]);             // Asha
```

Dot notation uses a known property name; bracket notation also supports a computed name, such as `student[field]`. Properties can be added, changed or removed with `delete student.marks`.

For an ordinary function, **`this` depends on how it is called**:

- In `student.describe()`, `this` is `student`.
- In a constructor called with `new`, it is the newly created object.
- In a plain function call in strict mode, it is `undefined`.
- `call`/`apply` can supply a receiver; `bind` creates a function with a fixed receiver.
- An arrow function inherits `this` from its surrounding scope; it does not obtain it from the object used to call it.

Therefore, use an ordinary method when it needs `this` to refer to its receiver. Merely storing a function inside an object does not permanently bind it to that object. See the [this reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this).

## 1.8 Errors and debugging

| Error kind | Meaning | Example |
| --- | --- | --- |
| Syntax error | The program cannot be parsed. | A missing closing bracket. |
| Runtime error | A failure occurs while executing code. | Accessing a property of `null`. |
| Logical error | The program runs but computes the wrong result. | Using the wrong formula or loop boundary. |

Common exception names include `ReferenceError` for an unavailable name, `TypeError` for an invalid operation on a value, and `SyntaxError` for invalid syntax.

Use `try`/`catch` to handle exceptions and `throw` to report an invalid situation. `finally` runs after the try/catch, whether or not an exception was thrown.

```javascript
function divide(a, b) {
  if (b === 0) throw new Error("The divisor must not be zero.");
  return a / b;
}
try {
  console.log(divide(10, 0));
} catch (error) {
  console.log(error.message);
} finally {
  console.log("Calculation finished.");
}
```

JavaScript normally produces `Infinity` for `10 / 0`, rather than throwing; the explicit check implements the required rule. A `try` block does not repair logical errors or catch a syntax error that prevents the containing script from starting.

**Debugging** means finding and correcting the cause of an error. A useful sequence is:

1. Reproduce the problem with a small input.
2. Read the browser console's message and line number.
3. Use `console.log()` to inspect values and types.
4. Set a breakpoint in developer tools, or insert `debugger;`.
5. Step through the code, watch variables and inspect the call stack.
6. Correct the cause and retest normal, boundary and invalid inputs.

# 2. Strings, numbers, arrays and other built-in objects

## 2.1 Strings

A string is text enclosed in single quotes, double quotes or backticks. Strings are immutable: string operations return new strings rather than changing the original text.

```javascript
const name = "Asha";
console.log(`Hello, ${name}!`); // Template literal: Hello, Asha!
```

For `const text = "Web Technology"`:

| Operation | Result or purpose |
| --- | --- |
| `text.length` | `14`; length is a property, not a method. |
| `text.charAt(0)` | `"W"`; indexing starts at zero. |
| `text.slice(0, 3)` | `"Web"`; end index is excluded. |
| `text.indexOf("Tech")` | `4`; returns `-1` if absent. |
| `text.includes("Web")` | `true`. |
| `text.toUpperCase()` | `"WEB TECHNOLOGY"`. |
| `text.toLowerCase()` | `"web technology"`. |
| `"  Web  ".trim()` | `"Web"`; removes surrounding whitespace. |
| `text.replace("Web", "Internet")` | `"Internet Technology"`. |
| `text.split(" ")` | `["Web", "Technology"]`. |

Know at least five methods with their effects; an older paper explicitly asks for them. Regular-expression forms of `search()` and `replace()` are covered in §2.6.

## 2.2 Numbers and conversions

JavaScript's usual `Number` type represents both integers and floating-point values. Decimal calculations can have small rounding errors; for example, `0.1 + 0.2` is not exactly `0.3`.

| Operation | Meaning |
| --- | --- |
| `Number("42")` | Converts the whole string to `42`; invalid numeric text produces `NaN`. |
| `parseInt("42px", 10)` | Reads an initial integer and produces `42`. |
| `parseFloat("3.5kg")` | Reads an initial decimal number and produces `3.5`. |
| `Number.isNaN(value)` | Tests whether the value is the special numeric value `NaN`. |
| `Number.isFinite(value)` | Tests whether the value is a finite number. |
| `Number.isInteger(value)` | Tests whether it is an integer-valued number. |
| `(12.345).toFixed(2)` | Produces the **string** `"12.35"`. |
| `(42).toString()` | Produces `"42"`. |

`NaN` means “Not a Number” and commonly results from a failed numeric calculation or conversion. It is not equal to itself, so do not test it using `value === NaN`. For validating a fully numeric form field, `Number()` plus appropriate checks is usually clearer than accepting a numeric prefix with `parseInt()`.

## 2.3 Arrays

An array stores an ordered collection of values. The first index is `0`, and `length` gives the number of slots. Arrays can grow and can contain objects or other arrays.

| Method | Meaning | Changes original array? |
| --- | --- | --- |
| `push(value)` | Adds at the end; returns new length. | Yes. |
| `pop()` | Removes and returns the last element. | Yes. |
| `unshift(value)` | Adds at the start; returns new length. | Yes. |
| `shift()` | Removes and returns the first element. | Yes. |
| `splice(start, count, ...items)` | Removes/replaces/inserts items; returns removed items. | Yes. |
| `slice(start, end)` | Copies a portion; end is excluded. | No. |
| `join(separator)` | Joins values into a string. | No. |
| `includes(value)` | Tests whether a value occurs. | No. |
| `indexOf(value)` | Finds an index, or `-1`. | No. |
| `sort(compareFn)` | Sorts the array in place. | Yes. |

```javascript
const items = ["B", "C"];
items.unshift("A");                   // ["A", "B", "C"]
console.log(items.shift());            // A; items is ["B", "C"]
console.log(items.splice(1, 1, "D"));   // ["C"]; items is ["B", "D"]
console.log(items.join(" - "));        // B - D
```

This example covers all four array operations named in the 2024 paper. Remember that `slice()` copies while `splice()` changes the original.

Default `sort()` compares string representations, so numeric sorting needs a comparator:

```javascript
const numbers = [10, 2, 30];
numbers.sort((a, b) => a - b);
console.log(numbers);                 // [2, 10, 30]
```

`map`, `filter` and `reduce` are explained with higher-order functions in §5.3.

## 2.4 Date and time

`new Date()` creates a date-time object for the current instant. Local getter methods interpret it using the device's local time zone.

| Method | Meaning |
| --- | --- |
| `getFullYear()` | Four-digit year. |
| `getMonth()` | Month index: January is `0`, December is `11`. |
| `getDate()` | Day of the month: 1–31. |
| `getDay()` | Day of the week: Sunday is `0`, Saturday is `6`. |
| `getHours()`, `getMinutes()`, `getSeconds()` | Local time components. |
| `getTime()` | Milliseconds since 1 January 1970 UTC. |
| `toLocaleDateString()` | A date formatted according to locale settings. |

To display today's date in the browser:

```html
<p id="today"></p>
<script>
  function showToday() {
    const now = new Date();
    document.getElementById("today").textContent =
      `${now.getDate()}/${now.getMonth() + 1}/${now.getFullYear()}`;
  }
  showToday();
</script>
```

For a chosen local date, `new Date(2026, 10, 15)` represents 15 November 2026. Add one to `getMonth()` for human-readable month numbers; do not confuse `getDay()` with `getDate()`.

## 2.5 Math and random numbers

`Math` provides mathematical constants and functions. Use it directly, without `new Math()`.

| Example | Result |
| --- | --- |
| `Math.abs(-5)` | `5` |
| `Math.sqrt(25)` | `5` |
| `Math.pow(2, 3)` | `8` |
| `Math.floor(3.8)` | `3`, rounds down |
| `Math.ceil(3.2)` | `4`, rounds up |
| `Math.round(3.6)` | `4`, rounds to the nearest integer |
| `Math.min(4, 2, 7)` | `2` |
| `Math.max(4, 2, 7)` | `7` |
| `Math.PI` | The constant π |

`Math.random()` produces a pseudorandom number from `0` inclusive to `1` exclusive. To generate an integer from `min` to `max`, inclusive:

```javascript
function randomInteger(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
console.log(randomInteger(1, 6)); // A dice value from 1 to 6.
```

Here, `min` and `max` are integers with `min <= max`. Multiplying sets the range, flooring creates an integer, and adding `min` shifts the range. This is suitable for simple simulations, not security tokens.

## 2.6 Regular expressions

A **regular expression (RegExp)** describes a text pattern. It can search for text, replace matching text or check whether input follows a rule.

A literal has the form `/pattern/flags`, such as `/web/i`, where `i` ignores case. The constructor form is `new RegExp("web", "i")`.

| Pattern part | Meaning |
| --- | --- |
| `^`, `$` | Beginning and end of input. |
| `[A-Za-z]` | One ASCII letter. |
| `[0-9]` or `\d` | One digit. |
| `\s` | A whitespace character. |
| `.` | Any character except line terminators, by default. |
| `+`, `*`, `?` | One or more, zero or more, zero or one repetition. |
| `{10}` | Exactly ten repetitions. |
| `g`, `i` flags | Global matching; case-insensitive matching. |

```javascript
const text = "Web and web";
console.log(text.search(/web/i));          // 0: first match's index.
console.log(text.replace(/web/gi, "IWT")); // IWT and IWT
console.log(/^\d{10}$/.test("9876543210")); // true
```

`search()` is a string method returning the first matching index or `-1`. `replace()` returns a new string; the `g` flag replaces all matches. `test()` is a RegExp method returning a boolean.

For validation, anchors matter: `/\d+/` accepts a digit sequence anywhere in `"abc123"`, whereas `/^\d+$/` requires the entire input to be digits. Avoid `g` on a repeatedly used validation pattern because it makes `test()` track a position between calls.

# 3. The HTML DOM: Documents, events and forms

## 3.1 What the DOM represents

The **Document Object Model (DOM)** represents an HTML document as a tree of objects called nodes. JavaScript uses this tree to inspect and change a page after it loads.

For `<body><p id="message">Hello</p></body>`, part of the tree is:

```text
Document
└── html element
    └── body element
        └── p element [id="message"]
            └── text node: "Hello"
```

An element node represents an element, a text node holds text, and a comment node holds a comment. Attributes belong to elements; they are not ordinary child nodes. A method performs an operation, such as `getElementById()`. A property stores or exposes a value, such as `textContent`.

The `document` object represents the page. Useful properties include `document.title`, `document.URL`, `document.body`, `document.documentElement` (the root `html` element) and `document.forms`.

To display a result, usually set an element's `textContent`. `console.log()` writes to the developer console, while `alert()` opens a dialog. Although older examples use `document.write()`, using it after the page has loaded can replace the page, so it is unsuitable for ordinary updates.

## 3.2 Finding elements and using CSS selectors

| Method | Result |
| --- | --- |
| `document.getElementById("message")` | The matching element, or `null`. |
| `document.getElementsByTagName("p")` | A live HTMLCollection of paragraphs. |
| `document.getElementsByClassName("note")` | A live HTMLCollection of elements with that class. |
| `document.querySelector(".note")` | The first CSS-selector match, or `null`. |
| `document.querySelectorAll(".note")` | A static NodeList of all CSS-selector matches. |

“Live” means the collection updates as the DOM changes. “Static” means its membership is a snapshot from the time of the query.

```javascript
function highlightNotes() {
  const notes = document.querySelectorAll("#content p.note");
  notes.forEach(p => p.classList.add("highlight"));
}
```

The selector chooses paragraphs with class `note` inside the element whose ID is `content`. CSS must define `.highlight` for a visible style change. A selector beginning with `#` matches an ID; `.` matches a class.

### NodeList versus array

A **NodeList** is an indexed collection of nodes with `length` and access such as `nodes[0]`. It supports iteration, but it is not an Array and does not directly provide methods such as `map()` or `filter()`. Use `Array.from(nodes)` when array operations are needed.

Not all NodeLists are static: `querySelectorAll()` returns a static list, while `childNodes` is live. An HTMLCollection contains elements; a NodeList may also contain text or comment nodes. See the [NodeList reference](https://developer.mozilla.org/en-US/docs/Web/API/NodeList).

## 3.3 Changing HTML content, attributes and CSS

Assume `message` refers to an existing element:

```javascript
const message = document.getElementById("message");
message.textContent = "Saved";
message.setAttribute("title", "Your changes were saved");
message.style.color = "green";
message.style.backgroundColor = "lightyellow";
message.classList.add("notice");
```

- `textContent` treats its value as text. `innerHTML` parses it as HTML markup; use text for ordinary user-provided values.
- `getAttribute`, `setAttribute` and `removeAttribute` inspect or change attributes.
- `style` changes inline CSS. Hyphenated CSS names become camelCase, such as `backgroundColor`.
- `classList.add`, `remove`, `toggle` and `contains` manage classes without replacing the whole class list.
- `getComputedStyle(element).color` reads the resolved style, including styles supplied by stylesheets; `element.style.color` reads only the inline setting.

An input's current contents are read through `.value`; a checkbox's current state through `.checked`. These properties describe current state, which may differ from the initial HTML attributes.

## 3.4 Navigating, creating and removing nodes

| Navigation property | Meaning |
| --- | --- |
| `parentNode` / `parentElement` | Parent node / parent element. |
| `childNodes` / `children` | All child nodes / only child elements. |
| `firstChild` / `firstElementChild` | First child node / first child element. |
| `nextSibling` / `nextElementSibling` | Next sibling node / next sibling element. |
| `previousSibling` / `previousElementSibling` | Previous sibling node / previous sibling element. |

Whitespace between tags can create text nodes. Therefore, `firstChild` may be whitespace when you expected an element. For node identification, `nodeType` is `1` for an element, `3` for text and `9` for a document; `nodeName` identifies the node, and `nodeValue` holds text-node content rather than an element's combined text.

To build a new element and insert it into `<ul id="books"></ul>`:

```javascript
const list = document.getElementById("books");
const item = document.createElement("li");
item.appendChild(document.createTextNode("Web Basics"));
list.appendChild(item);
```

`appendChild()` inserts at the end. `parent.insertBefore(newNode, existingChild)` inserts before a child. `parent.replaceChild(newNode, oldChild)` replaces one. `parent.removeChild(child)` or `child.remove()` removes a node. These operations change the displayed DOM, not the original source file on disk.

## 3.5 Events and event handlers

An **event** reports something that happened, such as a click, a key press or form submission. An **event handler** is the function that responds.

Three ways to attach a handler are an inline attribute (`onclick="save()"`), a property (`button.onclick = save`) and `button.addEventListener("click", save)`. `addEventListener()` supports multiple listeners without overwriting an existing property handler.

Pass the function itself, such as `save`, rather than `save()`, which would call it immediately.

| Event | When it occurs |
| --- | --- |
| `click`, `dblclick` | A click or double-click. |
| `mousedown`, `mouseup` | A mouse button is pressed or released over the element. |
| `mouseover`, `mouseout` | The pointer moves onto or off an element; transitions involving descendants can also trigger them. |
| `keydown`, `keyup` | A key is pressed or released. |
| `input` | A user edits a field's value. |
| `change` | A value change is committed, depending on the control. |
| `submit` | A form is submitted. |
| `focus`, `blur` | A control gains or loses focus. |
| `DOMContentLoaded` | The document has been parsed and deferred scripts have run. |
| `load` | A resource, or the page and its dependent resources, has loaded. |

### Mouse-event example

```html
<button id="box">Move the pointer here and press the mouse button</button>
<p id="status"></p>
<script>
  const box = document.getElementById("box");
  const statusLine = document.getElementById("status");
  function report(event) {
    statusLine.textContent = event.type;
  }
  for (const type of ["mouseup", "mousedown", "mouseover", "mouseout"]) {
    box.addEventListener(type, report);
  }
</script>
```

The event object supplies details such as `type`, `target` (where the event originated) and `currentTarget` (the element whose listener is running).

Many events **bubble** from the target through its ancestors. A listener can instead use the capture phase with `{ capture: true }`. `event.stopPropagation()` stops further propagation, while `event.preventDefault()` cancels a default action, such as following a link or submitting a form. Cancelling the default action does not itself stop propagation.

## 3.6 Forms and constraint validation

**Validation** checks whether input satisfies rules before it is used. Client-side validation gives quick feedback, but the server must validate again because browser checks can be bypassed.

HTML provides built-in **constraints**, meaning rules on acceptable values:

| Attribute | Rule |
| --- | --- |
| `required` | A value must be supplied. |
| `type="email"` | The value must have an email-like format. |
| `type="number"`, `min`, `max`, `step` | Restrict numeric values and permitted steps. |
| `minlength`, `maxlength` | Restrict text length. |
| `pattern` | Requires a text value to match the stated regular expression. |

In an HTML `pattern`, write the pattern without `/.../` delimiters. The pattern applies to the whole value. An optional empty input can still pass, so combine it with `required` when a value is mandatory.

JavaScript's constraint-validation API provides:

- `checkValidity()`: returns whether the constraints are satisfied.
- `reportValidity()`: checks and also shows the browser's validation feedback.
- `validity`: exposes states such as `valueMissing`, `typeMismatch` and `patternMismatch`.
- `validationMessage`: gives the validation message.
- `setCustomValidity(message)`: sets a custom error; pass `""` to clear it.

These are documented in the [constraint-validation guide](https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Constraint_validation).

### Registration form: validate and display the entered details

This example covers the older name/mobile/email question and the newer constraint-validation question. The simple name rule intentionally follows the exam's “alphabetic name” requirement; real names may need a broader rule.

```html
<form id="registration">
  <label>Name
    <input name="fullName" required pattern="[A-Za-z]+( [A-Za-z]+)*">
  </label>
  <label>Mobile
    <input name="mobile" type="tel" required pattern="[0-9]{10}">
  </label>
  <label>Email
    <input name="email" type="email" required>
  </label>
  <button type="submit">Show details</button>
</form>
<textarea id="summary" rows="4" cols="40" readonly></textarea>
<button id="confirm" type="button" hidden>Confirm</button>
<p id="message"></p>
<script>
  const form = document.getElementById("registration");
  const summary = document.getElementById("summary");
  const confirmButton = document.getElementById("confirm");
  const message = document.getElementById("message");

  form.addEventListener("submit", function (event) {
    event.preventDefault(); // Show a preview instead of navigating away.
    const fields = form.elements;
    summary.value =
      `Name: ${fields.namedItem("fullName").value}\n` +
      `Mobile: ${fields.namedItem("mobile").value}\n` +
      `Email: ${fields.namedItem("email").value}`;
    confirmButton.hidden = false;
    message.textContent = "Check the details, then confirm.";
  });

  form.addEventListener("input", function () {
    confirmButton.hidden = true;
    summary.value = "";
    message.textContent = "";
  });

  confirmButton.addEventListener("click", function () {
    message.textContent = "Details confirmed.";
  });
</script>
```

On an ordinary submit attempt, the browser checks the constraints before firing `submit`. Invalid input therefore does not reach this handler. `form.elements` gives access to controls, and `.value` reads their contents. Changing a field hides the old confirmation so it cannot confirm an outdated preview. The example confirms details locally; it does not save them on a server.

For a JavaScript-only rule, read the value and test it, for example `/^[A-Za-z]+( [A-Za-z]+)*$/.test(name)`. Use `setCustomValidity()` when the rule depends on several fields, and clear the old message when the user corrects the input.

# 4. The Browser Object Model (BOM)

## 4.1 DOM versus BOM and the window object

The **DOM** represents the document's contents. The **Browser Object Model (BOM)** is the conventional name for browser features around the document, such as the window, navigation history, browser information and timers.

| DOM | BOM |
| --- | --- |
| Works with page elements, text and attributes. | Works with the browser window and its environment. |
| Main entry point: `document`. | Main entry point: `window`. |
| Example: change a paragraph. | Example: go back in history or start a timer. |

In browser scripts, `window.document` refers to the document, and methods such as `alert()` and `setTimeout()` can be called through `window` or by their usual short names.

| Window feature | Purpose |
| --- | --- |
| `alert(message)` | Displays a message. |
| `confirm(message)` | Returns `true` for OK and `false` for Cancel. |
| `prompt(message)` | Returns entered text or `null` for Cancel. |
| `window.open(url)` | Requests a new browsing context; popup rules may block it. |
| `window.close()` | Can close permitted script-opened windows. |
| `window.location.href` | Reads the current URL; assigning a URL navigates there. |

## 4.2 Window size and history

`window.innerWidth` and `innerHeight` give the viewport dimensions in CSS pixels. `outerWidth` and `outerHeight` include the surrounding browser window. Screen dimensions are different: they describe the display rather than the page's available area.

```javascript
window.addEventListener("resize", function () {
  console.log(window.innerWidth, window.innerHeight);
});
```

`window.history` manages session-history navigation:

- `history.back()` moves back one entry.
- `history.forward()` moves forward one entry.
- `history.go(-2)` moves back two entries.
- `history.length` gives the number of entries in the session history.

These methods navigate existing history; they do not provide unrestricted access to every URL visited by the user.

## 4.3 Navigator and browser detection

`window.navigator` exposes information about the browser and its capabilities. Common properties include `language`, `userAgent`, `onLine` and `cookieEnabled`. `onLine` is a connectivity hint, not proof that a particular website is reachable.

**Browser detection** tries to identify the browser, often by inspecting `navigator.userAgent`. Such strings can be changed, reduced or misleading. Prefer **feature detection**: check whether the feature you need exists.

```javascript
if ("geolocation" in navigator) {
  console.log("The geolocation API is available.");
} else {
  console.log("Geolocation is unavailable.");
}
```

Availability does not guarantee permission or success. The [userAgent documentation](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/userAgent) explains why browser-name checks are unreliable.

## 4.4 Timing events

`setTimeout(callback, delay)` schedules a callback once. `setInterval(callback, delay)` schedules repeated callbacks. Delays are in milliseconds; the returned identifier can cancel the schedule.

```javascript
const timeoutId = setTimeout(() => console.log("Ready"), 1000);
// clearTimeout(timeoutId); // Uncomment to cancel before it runs.

let seconds = 0;
const intervalId = setInterval(function () {
  seconds++;
  console.log(seconds);
  if (seconds === 3) clearInterval(intervalId);
}, 1000);
```

A delay is not a guarantee of an exact execution time. The callback must wait until JavaScript can run it. Timers schedule work; they do not pause the current function. The event loop explains this behaviour (§5.5).

## 4.5 Cookies: Creating, reading, updating and deleting

A **cookie** is a small name/value record stored by the browser for a site. Matching cookies are sent with relevant HTTP requests, so they can support sessions and preferences across otherwise independent requests.

A session cookie has no explicit expiry and is intended for the browser session. A persistent cookie uses `Max-Age` or `Expires` to specify its lifetime.

Use `document.cookie` to manage cookies accessible to JavaScript:

```javascript
// Create a cookie for one day; encode characters in the value.
document.cookie = "user=" + encodeURIComponent("Asha Rao") +
  "; Max-Age=86400; Path=/; SameSite=Lax";

function readCookie(name) {
  const prefix = encodeURIComponent(name) + "=";
  const entry = document.cookie.split(";")
    .map(part => part.trim())
    .find(part => part.startsWith(prefix));
  return entry ? decodeURIComponent(entry.slice(prefix.length)) : null;
}
console.log(readCookie("user")); // Asha Rao, if the cookie was accepted.

// Update the cookie with the same name and path.
document.cookie = "user=Ravi; Max-Age=86400; Path=/; SameSite=Lax";

// Delete it by expiring the same name/path combination.
document.cookie = "user=; Max-Age=0; Path=/";
```

Assigning to `document.cookie` sets one cookie; it does not replace the whole cookie collection. Reading it returns accessible cookies as a semicolon-separated string, without their attributes.

Important attributes are `Path` (URL-path scope), `Domain` (domain scope), `Max-Age`/`Expires` (lifetime), `Secure` (HTTPS transmission) and `SameSite` (cross-site sending rules). A server can set `HttpOnly` to prevent JavaScript access. To delete a cookie, match its name, path and domain scope. Run cookie examples on a served page; local `file:` pages do not provide normal website cookie behaviour. See the [cookie API reference](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie).

### Display whether cookies are enabled

```html
<p id="cookieStatus"></p>
<script>
  function showCookieStatus() {
    document.getElementById("cookieStatus").textContent =
      navigator.cookieEnabled ? "Cookies are enabled." : "Cookies are disabled.";
  }
  showCookieStatus();
</script>
```

This answers the direct PYQ. The reported setting is not a guarantee that every cookie will be accepted: browser policies may still restrict particular contexts.

# 5. Advanced JavaScript concepts

These topics are explicitly in Module II, even where the supplied PYQs do not ask about them directly. Learn the purpose, mechanism and a small example for each.

## 5.1 Classes and inheritance

A **class** groups the construction and behaviour of similar objects. Its `constructor` initializes an instance created with `new`; methods describe what instances can do.

**Inheritance** lets a new type reuse and extend an existing type's behaviour. JavaScript implements inheritance through prototype links: if a property is not found on an object, lookup continues along its prototype chain. Class syntax provides a convenient way to define this relationship.

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  describe() {
    return `Name: ${this.name}`;
  }
}

class Student extends Person {
  constructor(name, roll) {
    super(name);
    this.roll = roll;
  }
  describe() {
    return `${super.describe()}, Roll: ${this.roll}`;
  }
}

const student = new Student("Asha", 12);
console.log(student.describe()); // Name: Asha, Roll: 12
```

`extends` establishes inheritance. In a derived constructor, call `super()` before using `this`. `super.describe()` calls the parent method. The child overrides `describe()` to add its own behaviour. This example demonstrates JavaScript inheritance; the older papers' Java class hierarchies do not require adding a Java chapter here.

## 5.2 Closures

A **closure** is a function together with access to the surrounding variables where it was created. This access remains even after the outer function returns.

```javascript
function makeCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}
const next = makeCounter();
console.log(next()); // 1
console.log(next()); // 2
```

The returned function remembers the binding for `count`. Each call to `makeCounter()` creates an independent counter. Closures are useful for private state, callbacks and functions configured with an earlier value. They remember access to variables, not necessarily a frozen copy of each value. See the [closure guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures).

## 5.3 Callbacks, higher-order functions and functional programming

A **callback** is a function passed to another function to be called as part of its work. A callback is not necessarily asynchronous.

```javascript
function calculate(a, b, operation) {
  return operation(a, b);
}
console.log(calculate(4, 3, (a, b) => a + b)); // 7
```

Here the callback runs immediately during `calculate()`. A timer callback runs later.

A **higher-order function** accepts a function as an argument or returns one. `calculate()` and `makeCounter()` are both examples. Functions are **first-class values** because they can be stored, passed and returned like other values.

**Functional programming** emphasizes combining functions to transform data. A **pure function** returns the same result for the same inputs and does not change external state. An **immutable approach** creates a new value rather than modifying an existing one.

```javascript
const marks = [40, 65, 80];
const increased = marks.map(mark => mark + 5);       // [45, 70, 85]
const passed = marks.filter(mark => mark >= 50);    // [65, 80]
const total = marks.reduce((sum, mark) => sum + mark, 0); // 185
```

`map()` transforms every item, `filter()` selects items, and `reduce()` combines them into one result. These examples leave `marks` unchanged. Pure functions and limited mutation make code easier to test and reason about; JavaScript allows both functional and other programming styles.

## 5.4 Promises and async/await

A **Promise** represents the eventual outcome of an operation. Its states are:

- **Pending:** the outcome is not yet known.
- **Fulfilled:** it completed successfully with a value.
- **Rejected:** it failed with a reason, usually an error.

A fulfilled or rejected promise is **settled**; it cannot later switch outcomes. `.then()` handles fulfillment, `.catch()` handles rejection, and `.finally()` runs cleanup regardless of the outcome.

```javascript
function delayedDouble(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (typeof n !== "number" || !Number.isFinite(n)) {
        reject(new Error("A finite number is required."));
      } else {
        resolve(n * 2);
      }
    }, 10);
  });
}

delayedDouble(4)
  .then(value => value + 1)
  .then(value => console.log(value)) // 9
  .catch(error => console.log(error.message));
```

Each `.then()` returns a new promise. Returning a value passes it to the next handler; returning a promise makes the chain wait for its outcome. A thrown error becomes a rejection that can be handled by `.catch()`.

### Async/await

An `async` function always returns a promise. Inside it, `await` pauses that function's continuation until a value/promise is ready; it does not block the browser's whole JavaScript thread.

Using `delayedDouble()` above:

```javascript
async function showDouble() {
  try {
    const value = await delayedDouble(4);
    console.log(value); // 8
  } catch (error) {
    console.log(error.message);
  }
}
showDouble();
```

A rejection at `await` behaves like a thrown exception, so `try`/`catch` provides readable error handling. Async/await organizes promise-based work; it does not make a long synchronous calculation non-blocking.

## 5.5 The event loop

The browser runs ordinary JavaScript on its main thread one task at a time. The **call stack** tracks active function calls. Browser facilities handle activities such as timers while the current code continues.

When work becomes ready, its callback is scheduled. The **event loop** coordinates when queued work can run. Promise reactions use a **microtask queue**, which is processed after current synchronous work finishes and before the next task, such as a timer callback.

```javascript
console.log("A");
setTimeout(() => console.log("B"), 0);
Promise.resolve().then(() => console.log("C"));
console.log("D");
// Output: A, D, C, B
```

`A` and `D` run synchronously. `C` is a promise microtask. `B` is a timer task; a delay of zero does not mean “run immediately.” Long-running synchronous code delays both callbacks and page interaction. See the [microtask guide](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide).

## 5.6 Generators

A **generator function**, written with `function*`, can pause at `yield` and resume later. Calling it creates an iterator; it does not immediately execute the whole function.

```javascript
function* ids() {
  yield 1;
  yield 2;
}
const iterator = ids();
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

Each `next()` resumes execution until the next `yield` or completion. Generators are useful for producing values on demand without constructing an entire collection at once. An ordinary generator is not automatically asynchronous; `yield` pauses iteration, while `await` waits for an asynchronous result.

## 5.7 Immediately invoked function expressions (IIFEs)

An **IIFE** is a function expression executed immediately after it is defined:

```javascript
(function () {
  const message = "Initialized";
  console.log(message);
})();
```

The first parentheses make the function an expression; the final `()` calls it. Its local variables stay inside the function. IIFEs are useful for one-time initialization and avoiding global names, especially in older code. An IIFE runs immediately; a closure describes access to surrounding variables. They are different ideas and can be used together.

## 5.8 Memoization

**Memoization** stores a function's results so repeated calls with the same input can reuse them. It trades memory for less repeated computation and is most suitable for deterministic functions whose result depends on their inputs.

```javascript
function makeMemoizedSquare() {
  const cache = new Map();
  return function (n) {
    if (cache.has(n)) return cache.get(n);
    const result = n * n;
    cache.set(n, result);
    return result;
  };
}
const square = makeMemoizedSquare();
console.log(square(5)); // 25: computed and cached.
console.log(square(5)); // 25: reused from the cache.
```

The arithmetic is deliberately small to show the mechanism. Memoization is more useful for expensive repeated calculations. Do not reuse a cached result when relevant external data may have changed; the cache must represent all inputs that determine the result.

## 5.9 Currying

**Currying** converts a function taking several arguments into a sequence of functions, each taking one argument.

```javascript
function add(a) {
  return function (b) {
    return a + b;
  };
}
const addTen = add(10);
console.log(addTen(3)); // 13
console.log(add(2)(5)); // 7
```

The first call stores access to `a` through a closure; the second supplies `b`. Currying is useful when one value will be reused across many calls, such as a fixed tax rate or prefix. It is a transformation of the calling structure, not another name for caching.

# 6. Two additional PYQ programming patterns

## 6.1 Sum of the first n prime numbers

A prime is an integer greater than one divisible only by one and itself. To test a candidate, try divisors up to its square root: if it has a larger factor, the matching smaller factor will already have been checked.

```javascript
function isPrime(n) {
  if (n < 2) return false;
  for (let divisor = 2; divisor * divisor <= n; divisor++) {
    if (n % divisor === 0) return false;
  }
  return true;
}

function sumFirstPrimes(n) {
  let count = 0;
  let candidate = 2;
  let total = 0;
  while (count < n) {
    if (isPrime(candidate)) {
      total += candidate;
      count++;
    }
    candidate++;
  }
  return total;
}
console.log(sumFirstPrimes(5)); // 2 + 3 + 5 + 7 + 11 = 28
```

To read `n` from a form, attach a submit handler:

```html
<form id="primeForm">
  <input name="number" type="number" min="1" max="1000" step="1" required>
  <button>Find sum</button>
</form>
<p id="primeResult"></p>
<script>
  // Include isPrime() and sumFirstPrimes() from the preceding block here.
  document.getElementById("primeForm").addEventListener("submit", function (event) {
    event.preventDefault();
    const n = Number(this.elements.namedItem("number").value);
    document.getElementById("primeResult").textContent = sumFirstPrimes(n);
  });
</script>
```

The input constraints keep the example to positive, reasonably small integers. Count the number of primes found, not merely the numbers examined: summing primes up to `n` is a different question.

## 6.2 Set a DIV's dimensions from user input and wrap text

```html
<form id="sizeForm">
  <label>Width <input name="width" type="number" min="50" max="800" required></label>
  <label>Height <input name="height" type="number" min="50" max="800" required></label>
  <button>Apply size</button>
</form>
<div id="panel"><p>This paragraph wraps to fit the chosen width.</p></div>
<script>
  document.getElementById("sizeForm").addEventListener("submit", function (event) {
    event.preventDefault();
    const panel = document.getElementById("panel");
    panel.style.width = Number(this.elements.namedItem("width").value) + "px";
    panel.style.height = Number(this.elements.namedItem("height").value) + "px";
    panel.style.overflowWrap = "anywhere";
    panel.style.overflow = "auto";
  });
</script>
```

JavaScript applies dimensions; CSS performs the wrapping. `overflowWrap` also breaks a long word when necessary, while `overflow: auto` allows scrolling if the content exceeds the selected height. Include the unit `px`: a bare number is not a valid CSS length for these properties.

# 7. Exam preparation and source basis

## What to practise first

The priorities below reflect the supplied papers, not a restriction of the syllabus.

| Source | Relevant emphasis | Covered here |
| --- | --- | --- |
| November 2024, [page 1](../sources/pyqs/A_1.png), Q1(b); [page 2](../sources/pyqs/A_2.png), Q3(b), Q4(a–e), Q5(a–e) | Application form; DOM methods; object methods; RegExp search/replace/test; let versus var; join/splice/shift/unshift; date display; cookie status; mouse events; CSS-selector access; debugging; constraint validation. | §§1.3, 1.7–1.8, 2.3–2.6, 3 and 4.5. |
| [April 2019](../sources/pyqs/CS201_Internet_and_Web_Technology_April_2019.pdf), pp. 1–2: Q2.A, Q2.B.ii, Q5.B.ii, Q6.A | `this`; creating/deleting cookies; factorial from prompt; five Math/string methods; Java versus JavaScript and script attributes; sum of the first n primes from a form. | §§1.1, 1.6–1.7, 2.1, 2.5, 4.5 and 6.1. |
| [2021-labelled, 2018–22 batch](<../sources/pyqs/IWT 2021 Endsem (2018-22 batch).pdf>), p. 1, Q2 | Form validation, displaying entered details and a confirmation button. | §3.6; adapt the fields to the question. |
| [2021-labelled, 2019–23 batch](<../sources/pyqs/IWT 2021 Endsem (2019-23 batch).pdf>), p. 2, Q2(i–ii) | Name/mobile/email validation and textarea output; input-driven DIV dimensions and wrapping. | §§3.6 and 6.2. |
| [Collected IWT-II papers](../sources/pyqs/IWT_II_Previous_Year_Question_Papers.pdf), 2023 Q2(b); [2023-labelled collection](../sources/pyqs/IWT_Question_Papers_2023.pdf) | Callback/asynchronous concepts overlap with Module II, but most questions concern Module III or other syllabi. | §5 supports the overlap; AJAX and jQuery remain in Module III. |

All five PDF collections and the two 2024 images were considered. Duplicate 2023 pages were not treated as extra evidence. The unrelated Android page and Java/JDBC/Servlet questions do not expand this module's scope. Dates in the two 2021 filenames are retained as file labels, not independently verified exam dates.

## Final coverage and answer check

- **JavaScript foundations:** introduction, statements, comments, variables, operators, decisions, loops, functions, objects, `this`, `let`/`const`, errors and debugging (§1).
- **Built-in objects:** strings, numbers, arrays, Date, Math, random numbers and RegExp (§2).
- **Documents, forms and HTML DOM:** document properties, methods, elements, HTML/CSS changes, events, navigation, nodes, NodeLists and form validation (§3).
- **BOM:** window object, size, history, navigator, browser/feature detection, timing events and cookie operations (§4).
- **Advanced concepts:** classes/inheritance, closures, callbacks, higher-order functions, functional programming, promises, async/await, event loop, generators, IIFEs, memoization and currying (§5); hoisting is explained with scope in §1.3.

For a theory answer, explain the purpose, how the concept works and one example. For a program, identify the input, validate it, perform the calculation or DOM operation, and show the result. Check boundary cases such as empty input, zero, a missing element and the first/last array index.

Coverage follows the [official syllabus, pages 2–3](../sources/syllabus/IWT-7th-Semester-IT.pdf#page=2). No dedicated Module II teacher handout is currently supplied. The available [jQuery](<../sources/sir's notes/jQuery.pdf>) and [AJAX](<../sources/sir's notes/AJAX.pdf>) materials provide limited context for events, DOM updates and callbacks; they do not replace the plain JavaScript syllabus. Technical references are linked beside concepts where they clarify important behaviour.
