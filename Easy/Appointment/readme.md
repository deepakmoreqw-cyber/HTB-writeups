Writeup: SQL Injection Fundamentals (Appointment)

Summary
This room walks through the basics of SQL Injection (SQLi) — from understanding what it is and where it sits in the OWASP Top 10, to enumerating a real web service and using a classic comment-based injection to bypass a login form.

 Task 1–2: Core Concepts
SQL stands for Structured Query Language.
One of the most common types of SQL vulnerabilities is SQL Injection — where unsanitized user input is passed directly into a database query, allowing an attacker to manipulate that query.

Task 4: OWASP Classification
Under the 2021 OWASP Top 10, SQL Injection falls under A03:2021 — Injection, a category covering flaws where untrusted input is interpreted as part of a command or query.

 Task 5–7: Web Enumeration
Ran an Nmap scan against the target to identify the web service:

nmap -sV -sC <target-ip>


 Port 80 was running Apache httpd 2.4.38 (Debian).
 The standard port for HTTPS is 443.
In web-application terminology, a folder is referred to as a directory.

Task 8–9: Directory Discovery
 A web server returns a 404 response code for "Not Found" errors.
 Used Gobuster to brute-force directories on the target. The `-w` switch specifies the wordlist, and the flag used to restrict discovery to directories (not subdomains) was `dir` — i.e., the `dir` mode: gobuster dir -u <target-url> -w <wordlist>`.

 Task 10–11: Exploiting the Login Form
In MySQL, a single # character comments out the rest of a line.
Since the login form didn't sanitize user input, a classic SQLi comment trick was used to bypass authentication — supplying an injection payload (e.g. `admin'#) that closes out the intended query logic and ignores the password check entirely.
Logging in this way returned a page beginning with the word "Congratulations", confirming the bypass worked.

 Flag
Submitted the flag obtained after bypassing the login page, confirming root flag owned.

 What I Learned
 How SQL Injection maps to the OWASP Top 10 (A03:2021 – Injection)
 Using Nmap to fingerprint web services and versions
Using Gobuster in directory brute-force mode (`dir`) to discover hidden paths
How MySQL comment syntax (`#`) can be abused to bypass login authentication when input isn't properly sanitized
 Why input validation and parameterized queries are essential defenses against SQLi

