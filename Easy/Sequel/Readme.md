HTB Writeup: Sequel (Very Easy)

Summary
Sequel is a beginner-friendly HackTheBox machine focused on network service enumeration with Nmap and basic interaction with a MariaDB database. It's a great starting point for understanding how misconfigured database services can lead directly to sensitive data exposure.

Enumeration
Started with an Nmap scan to identify open ports and running services on the target.

nmap -sV -sC <target-ip>

The scan revealed a MariaDB/MySQL service running on the target. From there, explored the database structure using standard SQL client commands:

sql
SHOW DATABASES;
USE <database>;
SHOW TABLES;
DESCRIBE <table>;

These commands helped map out what databases and tables existed, and what kind of data each table held — the essential first step before querying anything meaningful.

Foothold
Connected to the database using the `root` account **with no password required**. This is a classic misconfiguration on intentionally vulnerable/beginner-level machines, but it reflects a very real issue seen in production environments: default or blank credentials on exposed database services.


mysql -u root -h <target-ip>

Flags
With access established, used a wildcard `SELECT` to inspect table contents:

sql
SELECT * FROM <table>;


Working through the tables, the flag was located inside a table named `config`, within an entry specifically holding the flag string.

What I Learned

Nmap fundamentals — identifying open ports and service versions as the starting point of any assessment

Basic MariaDB/SQL commands — `USE`, `SHOW DATABASES`, `SHOW TABLES`, `DESCRIBE`, and `SELECT *` for navigating and querying a database

Real-world takeaway— blank/default credentials on database services remain a common and dangerous misconfiguration, even outside of lab environments


Machine: Sequel | Difficulty: Very Easy | Platform: HackTheBox
