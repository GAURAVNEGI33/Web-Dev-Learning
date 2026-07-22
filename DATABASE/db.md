# MySQL, Tables and Joins

## What This Class Was About

Before today, I was thinking of storing data in plain files, but that has real problems. Today's class showed why a real database like MySQL solves those problems, and I learned how to actually build tables, connect them, and read data from multiple tables together.

---

## Indexing - Why Search is Fast

An **index** is like the index/table of contents page at the back of a textbook. Instead of flipping through every single page to find a topic, you check the index, and it tells you exactly which page to jump to.

- A `PRIMARY KEY` automatically creates an index on that column - that's part of why searching by `id` is instant even with millions of rows.
- Without an index, MySQL has to scan every single row one by one to find what you're looking for (this is called a "full table scan") - which is slow on large tables.
- With an index, MySQL can jump almost directly to the matching row, similar to how you'd jump to a page number instead of reading the whole book.
- Indexes can also be added manually on other columns that are searched often (like `email`), so those lookups are fast too.

**Trade-off to remember:** Indexes make reading/searching faster, but they slightly slow down writing (INSERT/UPDATE), because MySQL also has to update the index every time data changes. So indexes are added on columns that are searched a lot, not on every column blindly.

---

## File System vs Database (in simple words)

Imagine you keep all your college notes in a single Excel/Notepad file on your laptop.

**Problems with a plain file:**

- If two people open and save the same file at once, one person's changes get overwritten - data is lost.
- If the file has thousands of rows, finding one specific entry means scrolling/searching through everything - it's slow.
- There's no built-in rule to stop duplicate or wrong data from being entered. Anyone can type anything.
- If your app crashes while writing to the file, the file can get corrupted.

**How a database is different:**

- It's built to handle many people reading and writing data at the same time, safely.
- It can find one record out of millions almost instantly (because of indexing - explained below).
- It lets you set rules, like "this field can never be empty" or "this value must be unique" - so bad data is rejected automatically.
- It keeps data safe and consistent even if something goes wrong mid-operation.

**In short:** A file is like one person maintaining a single notebook by hand. A database is like a well-organized office with a proper filing system, a librarian who finds things instantly, and rules about what can and can't be added.

---

## Setup

I installed **MySQL Server** and **MySQL Workbench**.

- **MySQL Server** is the actual database engine that stores and manages the data in the background.
- **MySQL Workbench** is just the GUI tool I use to write queries and view tables - it talks to the server for me.

A root password was set during installation, which is used to log in and manage the server.

---

## Understanding the Structure

A database is organized like folders inside folders:

```
Server
 └── Database
      └── Table
           └── Row (a single record)
```

One server can hold many databases, one database can hold many tables, and one table can hold many rows.

---

## Creating a Database

```sql
CREATE DATABASE resume_db;
USE resume_db;
```

- `CREATE DATABASE` makes a new, empty database.
- `USE` tells MySQL "from now on, work inside this database" so I don't have to mention its name in every query after this.

---

## Creating My First Table - `users`

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE
);
```

**Syntax breakdown, column by column:**

| Part                        | Meaning in simple words                                                                 |
| --------------------------- | --------------------------------------------------------------------------------------- |
| `CREATE TABLE users (...)`  | Make a new table named `users` with the columns listed inside the brackets              |
| `id INT`                    | A column named `id` that stores whole numbers                                           |
| `AUTO_INCREMENT`            | MySQL automatically gives the next number (1, 2, 3...) - I never type the id myself     |
| `PRIMARY KEY`               | This column is the unique identity of each row - no two rows can have the same id       |
| `name VARCHAR(100)`         | A text column named `name` that can hold up to 100 characters                           |
| `NOT NULL`                  | This column can never be left empty while inserting a row                               |
| `email VARCHAR(100) UNIQUE` | A text column that must be different for every single row - no duplicate emails allowed |

---

## Adding and Reading Data

**Inserting a new record:**

```sql
INSERT INTO users (name, email) VALUES ('Gaurav', 'gaurav@mail.com');
```

This means: "In the `users` table, add a new row where `name` is Gaurav and `email` is gaurav@mail.com." I don't need to give the `id` - MySQL fills it automatically because of `AUTO_INCREMENT`.

**Reading records back:**

```sql
SELECT * FROM users;
SELECT * FROM users WHERE id = 1;
```

- `SELECT * FROM users;` means "show me every column, for every row, in the users table."
- `WHERE id = 1;` narrows it down to only the row where id equals 1, instead of showing everything.

---

## Creating a Related Table - `resumes` (using Foreign Key)

```sql
CREATE TABLE resumes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    userId INT,
    title VARCHAR(100),
    FOREIGN KEY (userId) REFERENCES users(id)
);
```

**Why this matters:** Without a database feature like this, if I wanted to make sure every resume belongs to a real, existing user, I would have to write extra code every time - checking manually if the `userId` someone typed actually exists in the `users` table. That's error-prone and repetitive.

MySQL's **FOREIGN KEY** feature does this check automatically. It's a built-in rule that says: _"the value in `resumes.userId` must already exist as an `id` in the `users` table - otherwise, reject it."_ MySQL enforces this itself, so I don't have to write any validation code for it.

| Term          | Simple meaning                                                                          |
| ------------- | --------------------------------------------------------------------------------------- |
| `PRIMARY KEY` | Uniquely identifies a row in **its own** table                                          |
| `FOREIGN KEY` | A column that points to the Primary Key of **another** table, to connect the two tables |

So `resumes.userId` "points to" `users.id`. This is how the two tables stay linked without repeating all the user's information (name, email) inside every resume row.

---

## Joins - Reading Two Tables Together

A **JOIN** lets me combine data from two related tables into one result, matching rows based on the foreign key relationship.

```sql
SELECT users.name, resumes.title
FROM resumes
JOIN users ON resumes.userId = users.id;
```

This means: "Go through the `resumes` table, and for each resume, find the matching user in `users` where `resumes.userId = users.id`, then show me the user's name along with the resume title."

### Types of Joins (other than the one used today)

| Join Type                                               | What it does, in simple words                                                                                                                                                                                  |
| ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **INNER JOIN** (what we used - plain `JOIN` means this) | Shows only rows where a match exists in **both** tables. If a user has no resume, they won't show up.                                                                                                          |
| **LEFT JOIN**                                           | Shows **all** rows from the left (first) table, even if there's no matching row in the right table. Unmatched columns show as empty (NULL). Example: show all users, whether or not they've uploaded a resume. |
| **RIGHT JOIN**                                          | The opposite of LEFT JOIN - shows all rows from the right table, even if there's no match in the left table.                                                                                                   |
| **FULL OUTER JOIN**                                     | Shows all rows from both tables, matched where possible, and empty where there's no match on either side. (MySQL doesn't support this directly - it's done using a workaround.)                                |

**Easy way to remember:** INNER JOIN = only the common part. LEFT/RIGHT JOIN = keep everything from one side no matter what. FULL JOIN = keep everything from both sides.

---

## Key Takeaways

- A database solves the concurrency and slow-search problems that plain files have.
- The hierarchy is Server → Database → Table → Row.
- `PRIMARY KEY` uniquely identifies a row in its own table; `FOREIGN KEY` connects to another table's primary key and lets MySQL auto-enforce valid relationships, without me writing that validation logic myself.
- `NOT NULL` and `UNIQUE` are schema-level rules that stop bad data from ever getting saved.
- `JOIN` combines data from multiple tables in one query - `INNER`, `LEFT`, `RIGHT`, and `FULL OUTER` each keep different rows depending on whether a match exists.
- **Indexing** is what makes searching in a database fast - it works like a book's index page instead of reading every page one by one.

## Next Steps for Me

My `resume-api` project currently uses JSON file storage. Now that I understand relational schema design, foreign keys, joins, and indexing, migrating it to MySQL is a good next step - it'll be a solid, explainable talking point for interviews and will help me practice these concepts hands-on.

# 📌 What is Data?

Data is a collection of facts or information that can be stored, processed, and managed.

### Examples

- Name
- Age
- Email
- Phone Number
- Salary

---

# 🗃️ What is a Database?

A database is an organized collection of data that allows us to store, retrieve, update, and delete information efficiently.

Instead of keeping data in files or notebooks, databases make it easier to manage large amounts of information.

### Example

A college database may store:

- Students
- Teachers
- Courses
- Marks

---

# 💾 Types of Data

## 1. Structured Data

Structured data is organized in rows and columns.

It follows a fixed format, making it easy to search and manage.

### Examples

- Student records
- Employee details
- Bank transactions

---

## 2. Unstructured Data

Unstructured data does not follow any fixed format.

Examples include:

- Images
- Videos
- Audio files
- PDFs
- Social media posts

---

# 🏢 What is DBMS?

DBMS stands for **Database Management System**.

It is software used to create, store, update, and manage databases.

### Responsibilities

- Store data
- Retrieve data
- Update records
- Delete records
- Manage security

### Examples

- MySQL
- PostgreSQL
- Oracle
- SQL Server

---

# 🏛️ What is RDBMS?

RDBMS stands for **Relational Database Management System**.

It stores data in tables consisting of rows and columns.

Different tables can be connected using relationships.

### Example

Students Table

| ID  | Name  |
| --- | ----- |
| 1   | Rahul |

Courses Table

| Student_ID | Course     |
| ---------- | ---------- |
| 1          | JavaScript |

Here, both tables are connected using **Student_ID**.

---

# 🌐 Database Languages

Database languages are used to communicate with a database.

Main categories:

- DDL (Data Definition Language)
- DML (Data Manipulation Language)
- DQL (Data Query Language)
- DCL (Data Control Language)
- TCL (Transaction Control Language)

---

# 💻 What is SQL?

SQL stands for **Structured Query Language**.

It is the standard language used to communicate with relational databases.

Using SQL, we can:

- Create databases
- Create tables
- Insert data
- Update data
- Delete data
- Retrieve data

---

# ❓ Why Learn SQL?

Even if modern frameworks use ORMs like Sequelize, SQL is still important.

Reasons:

- Understand what happens behind the scenes.
- Write optimized queries.
- Debug database problems.
- Work with any relational database.
- Perform complex joins and reports.
- Crack technical interviews.

SQL is a fundamental skill for backend development.

---

# ⚡ SQL Commands

## DDL (Data Definition Language)

Used to define database structure.

Commands:

- CREATE
- ALTER
- DROP
- TRUNCATE

---

## DML (Data Manipulation Language)

Used to modify data.

Commands:

- INSERT
- UPDATE
- DELETE

---

## DQL (Data Query Language)

Used to retrieve data.

Command:

- SELECT

---

## DCL (Data Control Language)

Used for permissions.

Commands:

- GRANT
- REVOKE

---

## TCL (Transaction Control Language)

Used to manage transactions.

Commands:

- COMMIT
- ROLLBACK
- SAVEPOINT

---

# 🧩 MySQL Data Types

Choosing the correct data type improves storage efficiency and performance.

---

## VARCHAR

Stores variable-length text.

Example:

```sql
name VARCHAR(100)
```

Suitable for:

- Name
- Email
- Address

---

## STRING (TEXT)

MySQL does not have a STRING data type.

Instead, it provides TEXT types.

Example:

```sql
description TEXT
```

Used for:

- Long descriptions
- Articles
- Comments

---

## BOOLEAN

Stores True or False values.

Internally, MySQL stores BOOLEAN as TINYINT.

Example:

```sql
isActive BOOLEAN
```

Values:

- TRUE → 1
- FALSE → 0

---

## TINYINT

Stores very small integers.

Range:

Signed:

-128 to 127

Unsigned:

0 to 255

Example:

```sql
age TINYINT
```

---

## ENUM

Allows only predefined values.

Example:

```sql
gender ENUM('Male','Female','Other')
```

Benefits:

- Prevents invalid values.
- Saves storage.
- Keeps data consistent.

---

# ⚔️ SQL vs NoSQL

| SQL                         | NoSQL                                                   |
| --------------------------- | ------------------------------------------------------- |
| Stores data in tables       | Stores data in documents, key-value pairs, graphs, etc. |
| Fixed schema                | Flexible schema                                         |
| Uses SQL language           | Different query methods                                 |
| Best for structured data    | Best for semi/unstructured data                         |
| Supports joins              | Usually avoids joins                                    |
| Examples: MySQL, PostgreSQL | MongoDB, Cassandra, Redis                               |

---

# ⚙️ What is ORM?

ORM stands for **Object Relational Mapping**.

It allows developers to work with database records using programming language objects instead of writing SQL manually.

Example using Sequelize:

```javascript
User.findAll();
```

Instead of writing:

```sql
SELECT * FROM users;
```

The ORM converts JavaScript code into SQL automatically.

---

# 🤔 If Sequelize Writes SQL, Why Learn SQL?

Because Sequelize is only a tool.

Knowing SQL helps you:

- Understand generated queries.
- Optimize performance.
- Debug errors.
- Write custom queries.
- Work without an ORM when required.
- Handle real-world backend projects confidently.

A good backend developer knows both **SQL** and **ORMs**.

---

# 📌 Summary

- Database stores organized data.
- DBMS manages databases.
- RDBMS stores related data in tables.
- SQL is used to communicate with relational databases.
- SQL has different command categories like DDL, DML, DQL, DCL, and TCL.
- MySQL provides different data types such as VARCHAR, TEXT, BOOLEAN, TINYINT, and ENUM.
- SQL works best with structured data, while NoSQL is suitable for flexible or unstructured data.
- ORMs like Sequelize simplify database operations, but learning SQL is still essential for writing efficient and scalable applications.

---

# Presentation Topics: Answers

> These are worked answers to the five presentation topics. The point of every one is the same: to make you think like a backend developer, someone who assumes things will be attacked and builds so they cannot break. Read your own topic closely, but read all five, because they connect into one lesson. Where a real incident is involved, the facts here are from news reports; check them yourself before you present.

---

## 1. The CBSE Portal, and How Marks Could Be Changed

**Himanshu's topic.** What actually happened, from the news reports: In 2026 a 19-year-old named Nisarga Adhikary, who had just finished class 12, looked at CBSE's On-Screen Marking (OSM) portal — the site where teachers evaluate scanned answer sheets online. He found a set of basic security holes and reported them. CBSE first denied any breach, saying it was only a test site, then later acknowledged vulnerabilities in the provider's portal and ordered an audit. He did not do it for gain, he reported it, which is exactly what an ethical hacker does.

### The flaws, and what each one teaches

| The flaw                                                                                          | The backend lesson                                                                 |
| ------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| A master password was written into the source code, visible in the browser                        | Never put secrets in code that reaches the browser. Anyone can read it with Ctrl+F |
| The OTP check could be bypassed                                                                   | A security check the client can skip is not a security check                       |
| Password reset did not ask for the old password. Any user ID plus any gibberish reset it          | The server must verify every step. Never trust that the client did the checking    |
| An examiner's identity could be edited through browser storage, letting you impersonate a teacher | Never trust data from the browser. The server decides who you are, not the client  |
| Internal dashboards were open with no login                                                       | Every private route needs an access-control check, on the server, every time       |

**The one-sentence answer.** The marks were changeable because the system trusted the browser. Passwords, identities and checks all lived where the user could see and edit them. His own words: "These aren't advanced defences. They're the basics." A backend developer's first rule is the opposite of what CBSE did: never trust the client, verify everything on the server.

---

## 2. How 10,000+ Records Get Created in Minutes

**Deepesh's topic, and it happened in Shorter Loop.** A human types slowly. To create 10,000 records by hand would take days. So it was not done by hand. It was done by a **script**, a small program that repeats an action in a loop, as fast as the server will allow.

> **Script or bot.** A program that does a repetitive task automatically. A few lines can send the same "create record" request thousands of times in a loop. What takes a person all day takes a script a few seconds. This is not advanced, it is a beginner-level loop pointed at an endpoint that was not protected.

### Why a site lets this happen, and how you stop it

| What was missing                                                 | The fix a backend developer adds                                        |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------- |
| No rate limit. The server accepted requests as fast as they came | Rate limiting: cap how many requests one user or IP can make per minute |
| No bot check on the form                                         | A CAPTCHA or similar on public create-endpoints                         |
| The endpoint was open, no login needed to create records         | Require authentication, so anonymous scripts cannot post at all         |
| No validation catching obviously fake or duplicate data          | Server-side validation and duplicate checks that reject junk            |

**The one-sentence answer.** 10,000 records appeared because a script hit an endpoint that had no rate limit, no bot check and no login. The lesson: assume any open endpoint will be hit by a machine, not a polite human, and put limits in before it happens, not after.

---

## 3. What Information Is Kept in the UI

**Divya Bora's topic.** The UI is the HTML, CSS and JavaScript that runs in the browser. The single most important fact about it: **everything in it is visible to everyone**. Anyone can right-click, choose "view source" or open developer tools, and read every line of your HTML, CSS and JavaScript. Nothing there is hidden.

| What lives in the UI                                  | So remember                               |
| ----------------------------------------------------- | ----------------------------------------- |
| The page structure and text (HTML)                    | Readable by anyone, always                |
| The styling (CSS)                                     | Readable, harmless, fine to be public     |
| The logic that runs in the browser (JavaScript)       | Fully readable. Never hide a secret in it |
| Values stored in the browser (local storage, cookies) | Readable AND editable by the user         |

**The one-sentence answer.** The UI holds only what you are willing to show the whole world, because the whole world can read it. This is exactly the CBSE mistake from topic 1: a master password sat in the browser code, and browser storage decided who you were. Rule: never put a secret, a password, or a trust decision in the UI. The browser is the user's territory, not yours.

---

## 4. Who Are Ethical Hackers

**Gaurav and Ayush's topic.** An **ethical hacker** uses the same skills as an attacker, but with permission, to find security holes so they can be fixed before a real attacker uses them. Same knowledge, opposite intent, and crucially, done legally and reported responsibly.

| Ethical hacker                                 | Malicious hacker                 |
| ---------------------------------------------- | -------------------------------- |
| Has permission, or reports responsibly         | No permission, hides the act     |
| Finds a hole and tells the owner to fix it     | Uses the hole for gain or damage |
| Makes systems safer                            | Makes victims                    |
| Paid by companies, bug bounties, security jobs | Faces jail                       |

Nisarga from topic 1 is the live example. He found serious flaws and reported them to the authorities and CERT-In instead of changing marks for money. That single choice, report it rather than abuse it, is the whole line between the two columns. Companies pay ethical hackers, run bug bounty programs, and hire security teams precisely because they would rather a friendly researcher find the hole first.

**The one-sentence answer.** Ethical hackers are the people who find the holes before the criminals do, with permission and in the open, and it is a real, paid career. The skills are the same as an attacker's. The intent and the legality are what make them ethical.

---

## 5. The Movie: Pritam Pedro

**Unishka and Divyani's topic.** The film is _Pritam Pedro_, Rajkumar Hirani's 2026 cybercrime thriller on JioHotstar. It pairs two opposite cops: **Pedro**, an old-school investigator who trusts traditional methods, and **Pritam**, a young tech-savvy officer who works through modern technology and code. Together they solve cybercrimes, starting with a mystery around an abandoned ATM on a beach and building to a kidnapping case. The Hindu described it as a story where "old-school muscle beautifully collides with modern computer code." Watch it, and take notes on what it shows about how technology and crime meet, and about the backend developer's way of thinking.

### Watch for these, and note them

- **The two mindsets.** Pedro trusts instinct, Pritam trusts the system and the code. A backend developer needs both: the discipline to verify everything, and the instinct to sense when something is wrong. Notice where each approach wins.
- **Where does trust break?** A cybercrime happens because someone trusted something they should not have — a system, a person, a piece of data. This is the exact lesson from the CBSE topic. Note every point where misplaced trust opens a door.
- **What is exposed, and what is hidden?** Cybercrime turns on information: what an attacker can see, reach, or fake. Connect it to what we learned about the UI exposing everything, and identities being faked through the browser.
- **How is the attacker caught?** The tech-savvy side of the investigation is backend thinking in action: following data, spotting what does not add up, understanding how the system really works underneath.

### The Coding Side: What a Clicked Link Actually Reveals

Your practical task is the location question, so present the honest version of it, because most people get it wrong. When someone clicks a link you sent, **you do not get their exact location**. You get their **public IP address**, and an IP maps only to a rough area: a city or the internet provider's region, and it is often wrong for mobile data, VPNs and company networks.

| What people believe                            | What actually happens                                                 |
| ---------------------------------------------- | --------------------------------------------------------------------- |
| A clicked link reveals exact location          | It reveals the public IP, which is city or ISP level at best          |
| You can read their local address (192.168.x.x) | You cannot. It stays inside their router and never reaches any server |

> **Why the local IP never arrives.** The person's device has a private address like `192.168.1.7` inside their home network. On the way out, their router swaps it for the one public IP the whole house shares. This is called NAT. Your server only ever receives that public IP. The private one never leaves their network. So no server, in any language, can read a visitor's local IP from a request.

So how do the stories of "they found my exact location from a link" happen? Almost always one of these, and none of them is the link by itself:

| How exact location really leaks                           | What it needs                                                                  |
| --------------------------------------------------------- | ------------------------------------------------------------------------------ |
| The page asks "Allow location?" and the person taps Allow | The person's consent. The browser's GPS prompt cannot be skipped               |
| The person uploaded a photo or posted with location on    | The person handing the data over themselves                                    |
| Malware or a hacked account or phone                      | The device already compromised. A different, illegal thing                     |
| Police mapping an IP to a real address                    | A legal request to the internet provider. Not something a normal person can do |

**The one-sentence answer for the location task.** A link on its own leaks only the public IP, which is a rough area, never a street address and never the local IP. Exact location requires the person's consent, their own leaked data, or something already compromised. The gap between what people fear a link reveals and what it actually reveals is the real lesson, and it is pure backend thinking: the server only sees what the request carries, and everything precise stays behind a wall the user controls.

**What to bring back.** Do not just retell the plot. Bring three moments from the series that connect to something we have learned: misplaced trust, exposed information, scale, or how the crime was traced through the system. Show the batch how a cybercrime thriller is really a lesson in how systems break, and how a backend developer thinks to keep them from breaking. That is watching it like a backend developer.

---

## 6. The Thread Through All Five

**One idea, five topics.** Marks were changed because the browser was trusted. Records were flooded because an endpoint had no limits. The UI exposes everything because it runs on the user's machine. Ethical hackers are the people who find these gaps first. And the film is there to make the mindset stick. The single sentence under all of it:

**A backend developer assumes attack, never trusts the client, and verifies everything on the server.**

Present your own topic well, but show the batch how it connects to this.

---

_Everything here ran live in class. If a part does not click, message the instructor with the file name and the line, and read it again while it is fresh._

_Source: Day5_Session2_Answers.pdf — Full-Stack Internship coursework (Shorter Loop × Snapied)_
