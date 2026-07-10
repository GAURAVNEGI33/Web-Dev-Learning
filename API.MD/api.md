# 🌐 JavaScript - Working with APIs & Fetch

# 🍽 What is an API?

An API (**Application Programming Interface**) acts as a bridge between the client and the server.

A simple restaurant analogy:

- 👤 Client → Customer
- 🧑‍🍳 Server → Kitchen
- 🤵 API → Waiter

The client sends a request, the API delivers it to the server, and the server sends the response back through the API.

---

# 💻 Client & Server

A client can be:

- Browser
- Mobile App
- Desktop App

The server stores and manages data.

Important points:

- The client always sends the first request.
- One request receives one response.
- Communication happens using HTTP.

---

# 📨 Request & Response

## Request

A request contains:

- HTTP Method
- URL
- Headers
- Body (optional)

Example:

```http
GET /users
```

---

## Response

A response contains:

- Status Code
- Headers
- Body

Always check the **status code first** before trusting the response body.

---

# 📦 JSON

JSON (**JavaScript Object Notation**) is the most common format used to exchange data.

Convert JSON into JavaScript:

```javascript
const data = await response.json();
```

Convert JavaScript into JSON:

```javascript
JSON.stringify(user);
```

---

# 🚀 HTTP Methods

## GET

Retrieve data.

```http
GET /users
```

---

## POST

Create new data.

```http
POST /users
```

---

## PUT

Replace an entire resource.

```http
PUT /users/1
```

---

## PATCH

Update only specific fields.

```http
PATCH /users/1
```

---

## DELETE

Remove data.

```http
DELETE /users/1
```

---

# 🔄 GET vs POST

## GET

- Used to retrieve data.
- Data is sent through URL parameters.
- Can be bookmarked.
- Safe to repeat.

Example:

```text
/users?id=10
```

---

## POST

- Used to create new data.
- Data is sent in the request body.
- Cannot be bookmarked.
- Not safe to repeat because duplicate records may be created.

---

# 📊 HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Too Many Requests |
| 500 | Internal Server Error |

Status code groups:

- **1xx** → Informational
- **2xx** → Success
- **3xx** → Redirection
- **4xx** → Client Error
- **5xx** → Server Error

---

# 🔗 REST API Design

REST follows a simple convention:

- URLs are **nouns**
- HTTP methods are **verbs**

Examples:

```text
GET    /users
POST   /users
PUT    /users/1
PATCH  /users/1
DELETE /users/1
```

---

# ⚡ Using Fetch API

### Using `.then()`

```javascript
fetch(url)
  .then(res => res.json())
  .then(data => console.log(data));
```

---

### Using `async/await`

```javascript
const response = await fetch(url);

const data = await response.json();
```

Why do we use `await` twice?

- First `await` waits for the server response.
- Second `await` converts the JSON response into a JavaScript object.

---

# 📤 Sending Data with Fetch

```javascript
fetch(url, {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify(user)
});
```

Important parts:

- Method
- Headers
- Body

---

# ❌ Error Handling

`fetch()` only throws an error when there is a **network failure**.

For errors like **404** or **500**, we must manually check:

```javascript
if (!response.ok) {
    throw new Error("Request Failed");
}
```

Never assume a successful response just because `fetch()` completed.

---

# 🔐 Authentication Basics

Many APIs require authentication.

Common authentication methods:

- API Keys
- Bearer Tokens

Best Practice:

- Store secret keys in `.env`
- Never upload secrets to GitHub.
- Never expose secret keys in frontend code.

---

# 🚂 Express Basics

Basic Express routes:

```javascript
app.get();

app.post();

res.status().json();
```

Express is commonly used to build backend APIs.

---

# 🎮 Live Demo

During the session, I explored the **PokeAPI** using the browser console.

This helped me understand:

- Making API requests
- Reading JSON responses
- Working with real-world API data

---

# 📝 ONE LINER

- APIs allow clients and servers to communicate.
- Every request receives one response.
- Always check the status code before using the response.
- JSON is the standard format for exchanging data.
- `GET` retrieves data, while `POST` creates new data.
- REST URLs use nouns, and HTTP methods act as verbs.
- `fetch()` supports both `.then()` and `async/await`.
- Always check `response.ok` because `fetch()` does not throw errors for 404 or 500 responses.
- Store API keys and Bearer tokens securely in `.env`.
- Never commit secret keys to GitHub.


## 📚 Today's Learning

### 🌐 Understanding REST APIs

Today I learned how a REST API allows the frontend and backend to communicate with each other. Instead of directly accessing the database, the frontend sends a request to the server, and the server processes it before sending back a response.

I also understood that every resource (such as a user, document, or template) has its own endpoint, and different HTTP methods perform different actions.

- **GET** → Fetch existing data
- **POST** → Create new data
- **PUT** → Update existing data
- **DELETE** → Remove existing data

I learned that APIs usually exchange information in **JSON** format and return HTTP status codes like **200 (Success)**, **201 (Created)**, **404 (Not Found)**, and **500 (Server Error)**.

---

### 🛠️ API Structure

We explored how a Resume Builder API can be organized into different resources.

Some of the main API modules include:

- Authentication
- Users
- Documents
- Sections & Items
- Templates
- Version History
- AI Features
- ATS Check
- Resume Tailoring
- Export (PDF & DOCX)
- Sharing
- Job Application Tracker

This made it easier to understand how large applications organize their backend APIs.

---

### 💾 Browser Storage

Another important topic was how browsers store data locally.

#### 🍪 Cookies
Used mainly for authentication and login sessions. Cookies are automatically sent to the server with every request.

#### 📦 Local Storage
Stores data permanently until it is manually removed. Useful for saving user preferences like themes or language settings.

#### 📝 Session Storage
Works only while the browser tab is open. Once the tab is closed, all stored data is cleared.

#### 🗄️ IndexedDB
A powerful browser database used for storing large amounts of structured data, making offline applications possible.

---

### 🌍 Real-World Examples

- Gmail uses **Cookies** to keep users logged in.
- YouTube uses **Local Storage** to remember user preferences.
- Amazon combines **Cookies** and **Local Storage** for login and cart information.
- Google Docs uses **IndexedDB** for offline document editing.
- Spotify stores downloaded songs using **IndexedDB**.
- Google Maps saves offline maps using **IndexedDB**.

---

### 🎯 Key Takeaways

- REST APIs make communication between frontend and backend simple and organized.
- HTTP methods define what action should be performed on a resource.
- JSON is the standard format for sending and receiving API data.
- Different browser storage options exist for different use cases.
- Choosing the right storage method improves both performance and user experience.