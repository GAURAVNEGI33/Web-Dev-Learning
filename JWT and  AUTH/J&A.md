# Authentication & JWT Learning Notes

This document contains my personal notes and understanding of how Authentication, JWT (JSON Web Tokens), and npm versions work in a Node.js/Express application.

## 2. Why Hash Passwords?

If you look in the database, a password looks like this: `$2b$10$KR0tMiO/l6JH51Bipc3...`
This scramble is created using a tool like `bcrypt`. It is a **one-way hash**. You can turn a password into a hash, but you cannot turn a hash back into a password.
During login, we hash the fresh password attempt and compare the two scrambles. This means even if the database is leaked, no one can read the real passwords!

## 3. What is a JWT Token actually?

A JWT looks like random text, but it is exactly three parts joined by dots: `header.payload.signature`

- **Header:** Says which algorithm signed it (e.g., HS256).
- **Payload:** The actual data (like `user.id`, `name`, `email`). This is the part our middleware reads to know who is calling.
- **Signature:** A mathematical fingerprint made from the header, the payload, and our server's `JWT_SECRET`.

> **CRITICAL RULE:** The payload is NOT secret. Anyone holding the token can decode and read the payload. It is only encoded, not encrypted. **Never put a password or anything private in a JWT.**

## 4. How JWT Security Works (Tamper-Proofing)

Because the payload is readable, what stops someone from changing their `id` to 1 to become an admin? **The Signature.**
Only our server knows the `JWT_SECRET` (stored safely in our `.env` file). If a hacker changes even one character in the payload, the signature no longer matches. When our server runs `jwt.verify()`, it will instantly reject the tampered token. The token is readable, but not forgeable.

## 6. Understanding npm Versions (`~` vs `^`)

When looking at `package.json`, the symbols in front of the version numbers (like `MAJOR.MINOR.PATCH` -> `4.17.21`) control how npm updates packages:

- `4.17.21` (no symbol): Exact version only. No updates allowed.
- `~4.17.21` (tilde): Allows **Patch** updates only (e.g., `4.17.22` for bug fixes).
- `^4.17.21` (caret): The npm default. Allows **Minor and Patch** updates (new features and fixes, e.g., `4.18.0`), but NEVER a major breaking change (like `5.0.0`) which could break the code.
