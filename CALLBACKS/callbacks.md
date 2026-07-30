# Callbacks in Node.js

Today, I learned all about how asynchronous tasks work in Node.js. If you've ever wondered how JavaScript handles things that take time (like reading a file or calling an API) without freezing the entire application, this is for you! Let's break it down in simple words.

## 1. What is a Callback?

Think of a callback as a promise to do something **after** a task is finished. When we ask Node.js to do a job that takes time (like fetching data from a database, reading a file, or waiting for a timer), we don't just stop and wait. Instead, we give Node.js a function (the callback) and say, "Hey, go do this job, and **when you are done**, run this function."

Here is a custom example (`orderTea`):

```javascript
function orderTea(callback) {
  console.log("Ordering tea...");
  // Simulate time passing with a timeout (e.g., waiting for the tea to be made)
  setTimeout(() => {
    callback(); // <-- Calling it when the job is done
  }, 2000);
}

orderTea(function () {
  console.log("Yay, my tea is here! ☕");
});
```

Notice how we pass a function inside `orderTea()`. That passed-in function is the callback!

## 2. Error-First Callbacks

In the real Node.js world, things can go wrong. A file might be missing, or a database might be down. Because of this, Node.js follows a strict rule: **Error-First Callbacks**.

This means the very first argument passed to a callback is always an error (if one occurred). If everything went fine, the error is `null` and you get the result in the second argument.

```javascript
function playSong(songName, callback) {
  if (!songName) {
    return callback("No song name provided!"); // Error first
  }
  callback(null, "Now playing: " + songName); // null = no error, then the result
}

playSong("Shape of You", function (error, result) {
  if (error) {
    console.log("Error:", error);
  } else {
    console.log("Success:", result);
  }
});
```

**Best Practice:** Always check `if (error)` first! Using `return` after the error stops your app from running the success code by mistake and crashing later.

## 3. The Dreaded Callback Hell

What happens if you have to do multiple things in order? For example:

1. Boil water
2. Add tea leaves
3. Pour into cup

Because each step takes time, we must wait for one to finish before starting the next. We do this by putting the next step _inside_ the callback of the previous step.

```javascript
boilWater(function () {
  // nested level 1
  addTeaLeaves(function () {
    // nested level 2
    pourIntoCup(function () {
      // nested level 3
      console.log("Tea is finally ready! 🍵");
    });
  });
});
```

See how the code starts shifting to the right, forming a pyramid shape? This is called **Callback Hell**. The pile up of `});` at the bottom makes code extremely hard to read and debug.

## 4. Escaping Callback Hell (3 Fixes)

Luckily, we don't have to live in Callback Hell. Here are three ways to fix it:

### Fix 1: Named Functions

The simplest fix is to stop using anonymous (unnamed) functions. We can define them separately and give each step its own name. This flattens the pyramid into a neat list of calls.

```javascript
function ready() {
  console.log("Tea is finally ready! 🍵");
}
function step2() {
  pourIntoCup(ready);
}
function step1() {
  addTeaLeaves(step2);
}

boilWater(step1); // <-- One call starts the chain
```

### Fix 2: Promises

A modern way to fix this is using Promises. A function returns a "Promise" that it will finish later. We can chain these using `.then()`, and catch errors nicely.

```javascript
boilWater()
  .then(addTeaLeaves)
  .then(pourIntoCup)
  .then((result) => {
    console.log("Tea is finally ready! 🍵");
  });
```

It reads top-to-bottom, which is much cleaner!

### Fix 3: Async / Await (The Cleanest Way)

This is the modern and absolute best way to write asynchronous Node.js code. It makes our code look exactly like normal, synchronous steps. We just put `await` in front of the function, and JavaScript will wait for it to finish before moving to the next line.

```javascript
async function makeMyTea() {
  const result1 = await boilWater();
  const result2 = await addTeaLeaves();
  const finalTea = await pourIntoCup();
  console.log("Tea is finally ready! 🍵");
}

makeMyTea(); // <-- calling it
```

No nesting, no messy syntax, just clean, readable top-to-bottom code!

## 📝 Summary

- **Callback:** A function you run when a job is done.
- **Error-First:** Always handle the error first before touching the result.
- **Callback Hell:** Messy, nested callbacks (the pyramid of doom).
- **Named Functions:** Flattens the mess.
- **Promises:** Cleans it up with `.then()`.
- **Async/Await:** The absolute cleanest way to run steps in order.
