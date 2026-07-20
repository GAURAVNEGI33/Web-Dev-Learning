# Day 18 - MySQL, Tables and Joins

**Module:** Database (Module 4)
**Instructor:** Dinesh Rawat
**Class:** 1 of 5

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
