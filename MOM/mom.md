# Doubt Clearing Session (MOM)

## What This Session Was About

This wasn't a new-topic class - it was a revision/doubt-clearing session. We went back over everything that was still confusing from the last few days and rebuilt it from the ground up, so the basics are solid before moving forward.

---

## 1. MVC Architecture

MVC stands for **Model - View - Controller**. It's a way of organizing backend code so that each part has one clear job, instead of everything being dumped into a single file.

| Part           | Job                                                                                                          |
| -------------- | ------------------------------------------------------------------------------------------------------------ |
| **Model**      | Talks to the database - defines what the data looks like and how it's stored/fetched                         |
| **View**       | The response format sent back (in an API, this is usually just JSON, not an HTML page)                       |
| **Controller** | Contains the actual logic - decides what to do when a request comes in, and calls the Model to get/save data |
| **Route**      | Decides which URL maps to which controller function                                                          |

**Why separate them?** If everything is written in one file, the code becomes hard to read, hard to debug, and hard to change later. By splitting responsibilities, I can change how data is stored (Model) without touching how requests are handled (Controller), and vice versa. This is the same pattern I already used in my `resume-api` project (34 routes, MVC/controller pattern).

---

## 2. Backend and APIs

An **API** is basically a way for two programs to talk to each other - my frontend sends a **request**, my backend processes it and sends back a **response**.

- **Request:** What the client sends - includes the URL, the method (GET, POST, PUT, DELETE), and sometimes a body (data being sent, like a form).
- **Response:** What the server sends back - includes a status code and usually a body in **JSON** format.
- **Status Codes:** Numbers that tell you what happened with the request:
  - `200` - OK, success
  - `201` - Created (something new was added)
  - `400` - Bad Request (client sent something wrong)
  - `401` / `403` - Not allowed/unauthorized
  - `404` - Not Found
  - `500` - Server Error (something broke on the backend)
- **JSON (JavaScript Object Notation):** The standard format data is sent in - basically key-value pairs, easy for both humans and machines to read.
- **Why Postman:** Postman lets me test my API endpoints (send requests, check responses) without needing a frontend built yet. It's how I can confirm my backend logic works correctly before connecting any UI to it.

---

## 3. JavaScript - Basics to Array Methods

We revisited JS fundamentals and moved up to array methods, since these come up constantly in both frontend and backend code.

**Basics revisited:** variables (`let`/`const`), functions, conditionals, loops, and how JS handles data types.

**Array methods** - these let me transform arrays without writing manual loops every time:

| Method      | What it does                                                  |
| ----------- | ------------------------------------------------------------- |
| `map()`     | Transforms every item in an array and returns a new array     |
| `filter()`  | Returns a new array with only the items that pass a condition |
| `reduce()`  | Combines all items into a single value (like a sum or total)  |
| `find()`    | Returns the first item that matches a condition               |
| `forEach()` | Runs a function on every item, without returning a new array  |

These are used all the time in backend code too - like transforming database results before sending them as a JSON response.

---

## 4. Error Handling

In backend code, things can go wrong - bad input, database failure, etc. Error handling means catching these problems instead of letting the app crash.

```javascript
try {
  // code that might fail
} catch (error) {
  // handle the error here
  res.status(500).json({ message: "Something went wrong" });
}
```

- `try` block: the code that might throw an error.
- `catch` block: runs only if something inside `try` fails - this is where I handle it gracefully instead of crashing.
- **Sending the right status code matters** - if I always send `200` even on errors, the client (frontend) has no way to know something actually failed. The status code should always match what really happened (e.g., `400` for bad input, `500` for server-side failure).

---

## 5. Git and GitHub

Revisited the core workflow:

```bash
git add .
git commit -m "message"
git push
```

- `git add .` - stages my changes (tells Git which files to include in the next commit).
- `git commit -m "..."` - saves a snapshot of those changes with a message describing what changed.
- `git push` - sends my committed changes to GitHub (the remote repository).

**Writing real commit messages** - instead of vague messages like "update" or "fix", a good commit message says what actually changed and why, like `"fix: resolve nested repo issue in resume-api"`. This makes it much easier to look back at project history later - something I've already dealt with in my `resume-api` project (nested repos, merge conflicts, force pushes).

_(Branching workflow to be planned/covered later.)_

---

## Key Takeaways

- MVC keeps backend code organized by giving each part - Model, View, Controller, Route - one clear responsibility.
- APIs work on a request → response cycle, and status codes tell the client what actually happened.
- Postman is for testing backend endpoints independently, before a frontend exists.
- Array methods (`map`, `filter`, `reduce`, `find`, `forEach`) replace manual loops for common data transformations.
- Proper error handling (`try`/`catch` + correct status codes) prevents silent failures and helps the frontend respond correctly.
- Clear Git commit messages make project history readable and easier to explain later - important for both teamwork and interviews.

## Note

A notes PDF covering all of today's topics in more detail was shared separately.
