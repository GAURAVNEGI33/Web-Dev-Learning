# JavaScript Functions & Events

Today I learned about one of the most important concepts in JavaScript: **Functions** and **Events**. These topics help us write cleaner code and make websites interactive.

---

## 📌 What I Learned

### Functions

A function is a reusable block of code that performs a specific task. Instead of writing the same code again and again, I can create a function once and call it whenever I need it.

### Why Functions Are Useful

- They reduce code repetition.
- They make code easier to read.
- They help organize logic into small parts.
- Fixing bugs becomes easier because changes are made in one place.
- Small functions are easier to test and maintain.

> I learned that a good function should focus on doing only **one task**.

---

## Function Syntax

A function can take **parameters**, which act like placeholders for values.

When calling a function, the actual values passed are called **arguments**.

Example:

```javascript
function add(a, b) {
  return a + b;
}

add(2, 3);
```

- **Parameters:** `a`, `b`
- **Arguments:** `2`, `3`

---

## Return Statement

The `return` keyword sends a value back from the function.

Once `return` is executed, the function stops running.

Not every function needs a return value. Some functions simply perform an action.

---

## Function vs Procedure

I learned that in JavaScript, everything is written as a function.

Some functions return a value, while others only perform a task.

---

## Pure vs Impure Functions

### Pure Function

- Produces the same output for the same input.
- Does not change anything outside itself.
- Easy to test and predict.

### Impure Function

- Can modify external variables or interact with the outside world.
- Output may change even with the same input.

Whenever possible, writing pure functions is considered a better practice.

---

## Different Ways to Write Functions

### Function Declaration

```javascript
function greet(name) {
  return "Hi " + name;
}
```

### Function Expression

```javascript
const greet = function(name) {
  return "Hi " + name;
};
```

### Arrow Function

```javascript
const greet = (name) => {
  return "Hi " + name;
};
```

I also learned that arrow functions can be written in a shorter form using **implicit return**.

---

# JavaScript Events

An **event** is something that happens on a webpage, like:

- Clicking a button
- Pressing a key
- Moving the mouse
- Typing in an input field
- Submitting a form

Events allow JavaScript to respond to user actions.

---

## addEventListener()

The main way to handle events is using `addEventListener()`.

Example:

```javascript
button.addEventListener("click", () => {
  console.log("Button clicked");
});
```

The function passed inside is called a **callback function** because it runs only when the event occurs.

---

## Event Object

Whenever an event happens, JavaScript provides an **event object** containing useful information.

Some common properties are:

- `event.target`
- `event.type`

These help identify what triggered the event.

---

## Common Events

Some events I learned today:

- click
- dblclick
- mouseover
- mouseout
- mousemove
- keydown
- keyup
- input
- change
- submit
- blur
- scroll
- resize
- load

---

## Event Handling Pattern

A simple way to remember event handling is:

1. Select the element.
2. Listen for an event.
3. Perform an action.

This pattern is used in most interactive JavaScript programs.

---

## Removing Event Listeners

JavaScript also allows removing an event listener using:

```javascript
removeEventListener()
```

This is useful when an event should stop working after a certain condition.

I also learned that the same named function must be used while removing an event listener.

---

## Graceful Degradation

A website should still display its basic content even if JavaScript is disabled.

JavaScript should improve the user experience instead of being the only thing that makes the page work.

---

# Key Takeaways

- Functions help write reusable and organized code.
- A good function should perform only one task.
- Parameters are placeholders, while arguments are the actual values passed.
- Functions can be written using declarations, expressions, or arrow syntax.
- Events make webpages interactive.
- `addEventListener()` is used to respond to user actions.
- Every interactive feature follows the pattern: **Select → Listen → Do**.
- Websites should remain usable even if JavaScript is disabled.

---

## What I Practiced

- Creating reusable functions
- Using parameters and return values
- Writing arrow functions
- Understanding pure and impure functions
- Handling user events with `addEventListener()`
- Accessing the event object
- Following the Select → Listen → Do approach
- Removing event listeners when needed