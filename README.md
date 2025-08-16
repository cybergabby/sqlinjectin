# sqlinjection
# SQL Injection on DVWA – Write-Up

Target
Application: Damn Vulnerable Web Application (DVWA)
Vulnerability: SQL Injection (sqli module)
---------------------------------------------------
# Initial Reconnaissance

After accessing the SQL Injection module in DVWA, I noticed that the application accepted a GET parameter (id) that could be vulnerable to injection.

Example vulnerable request:
http://127.0.0.1/DVWA/vulnerabilities/sqli/?id=1&Submit=Submit

<img width="1335" height="538" alt="sqli dvwa1" src="https://github.com/user-attachments/assets/d896c1bc-d472-46db-92da-e84c4f173018" />

# Database Enumeration
I used sqlmap to automate the exploitation of the injection point.
Command executed:
sqlmap -u "http://127.0.0.1/DVWA/vulnerabilities/sqli/?id=1&Submit=Submit" --dbs --batch

<img width="1054" height="553" alt="sqli2" src="https://github.com/user-attachments/assets/188123b5-be99-4a0d-9711-f4f3cbc7db81" />

# Database Exfiltration
Once databases were confirmed, I selected the target database (in this case dvwa).
Command: sqlmap -u "http://127.0.0.1/DVWA/vulnerabilities/sqli/?id=1&Submit=Submit" -D dvwa --tables --batch
<img width="1353" height="587" alt="sqlidbexfiltrate" src="https://github.com/user-attachments/assets/e8846dc2-f5f9-4885-8dfb-2d20f0e90e7f" />
Result: Extracted table names from the dvwa database (e.g., users, guestbook).

# Table Extraction
Next, I dumped the contents of the users table, which typically stores credentials.
Command: sqlmap -u "http://127.0.0.1/DVWA/vulnerabilities/sqli/?id=1&Submit=Submit" -D dvwa -T users --dump --batch

<img width="1053" height="567" alt="sqligainedaccess" src="https://github.com/user-attachments/assets/2e8c9d2f-9f6b-4c8a-9e97-dd70c3d1394c" />
Result: Extracted user information including usernames, password hashes, and other sensitive details.
which resulted in me gaining access.
<img width="1053" height="567" alt="sqligainedaccess" src="https://github.com/user-attachments/assets/b18272c9-4d80-4d5f-8444-b500a2d286a9" />

# Information Disclosure
The SQL injection allowed unauthorized access to backend data. This demonstrated:
Database discovery (identified the DBMS and schema).
Table extraction (enumerated sensitive tables).
Sensitive data exfiltration (retrieved user credentials).
This confirms that the SQL injection vulnerability can lead to full database compromise if left unpatched.

# Impact
Attackers could exfiltrate sensitive information (usernames, password hashes, etc.).
With cracked credentials, attackers could escalate privileges.
Possible pivoting into the backend infrastructure.

# Recommendation
Implement prepared statements (parameterized queries).
Use stored procedures where possible.
Apply least privilege to the database user.
Regularly test web apps for injection flaws.

Thanks For Reading my writeup
