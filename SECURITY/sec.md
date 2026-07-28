## 1. Register API & Token

When a user signs up, we need their name, email, and password. We first check the database to make sure the email isn't already taken. If it's unique, we save the user. Instead of making them log in immediately after, we automatically give them a "token" in the response so their app knows they are already authenticated.

## 2. Password Encryption & Hashing

We never save raw passwords (like `password123`) in the database. Instead, we scramble them into an unreadable string called a hash. Because hashing works only one way, even if a hacker steals the database (or if a developer looks at it), they can't reverse the hash back to the real password. When a user logs in, we just take the password they typed, hash it the exact same way, and compare the two scrambles.

## 3. Promises

In JavaScript, tasks that take time (like saving to a database) don't freeze the rest of the code. Instead, JavaScript gives you a "Promise" (an IOU) that it will finish the job later. A Promise has three states:

- **Pending**: It's still working on it.
- **Resolved**: It succeeded (and triggers `.then()`).
- **Rejected**: It failed (and triggers `.catch()`).

## 4. Callback Hell & Modern Solutions

In the old days, to handle time-consuming tasks sequentially, developers put functions inside functions (callbacks). This created a pyramid shape of code that was messy and hard to read ("callback hell"). Today, we solve this in two ways:

- **`.then()` chaining**: Flattens out the code.
- **`async / await`**: Makes asynchronous code read exactly like normal, line-by-line synchronous code, making it extremely easy to read and catch errors with `try...catch`.

---

## 💻 Promise Code Example

Below is a small example demonstrating a Promise that resolves and one that rejects:

```javascript
// 1. A Promise that RESOLVES
const successfulPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Yay! The task was completed successfully.");
  }, 1000); // Waits 1 second
});

successfulPromise
  .then((result) => console.log("Success:", result))
  .catch((error) => console.log("Error:", error));

// 2. A Promise that REJECTS
const failingPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("Oh no! The database connection failed.");
  }, 1000); // Waits 1 second
});

failingPromise
  .then((result) => console.log("Success:", result))
  .catch((error) => console.log("Error Caught:", error));
```
