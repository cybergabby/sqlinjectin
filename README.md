# sqlinjectin
# SQL Injection on DVWA – Write-Up

Target
Application: Damn Vulnerable Web Application (DVWA)
Vulnerability: SQL Injection (sqli module)
---------------------------------------------------
1. Initial Reconnaissance

After accessing the SQL Injection module in DVWA, I noticed that the application accepted a GET parameter (id) that could be vulnerable to injection.

Example vulnerable request:
http://127.0.0.1/DVWA/vulnerabilities/sqli/?id=1&Submit=Submit



<img width="1335" height="538" alt="sqli dvwa1" src="https://github.com/user-attachments/assets/d896c1bc-d472-46db-92da-e84c4f173018" />
