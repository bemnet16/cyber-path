 **owasp top 10 2021**
 *Broken Access Control* 
 - If a website visitor can access protected pages they are not meant to see, then the access controls are broken.
	 - **IDOR** or **Insecure Direct Object Reference** refers to an access control vulnerability where you can access resources you wouldn't ordinarily be able to see.

 When Broken Access Control exploits or bugs are found, it will be categorised into one of **two types**:

|                                     |                                                                                                                 |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Horizontal** Privilege Escalation | Occurs when a user can perform an action or access data of another user with the **same** level of permissions. |
| **Vertical** Privilege Escalation   | Occurs when a user can perform an action or access data of another user with a higher level of permissions.     |

 *Cryptographic Failures*
 -  refers to any vulnerability arising from the misuse (or lack of use) of cryptographic algorithms for protecting sensitive information. Web applications require cryptography to provide confidentiality for their users at many levels.
 - **sqlite3**
	 - The most common (and simplest) format of a flat-file database is an SQLite database. and `sqlite3` is used for querying them on the command line. 
		After download it 
			To access   `sqlite3 <database-name>`
			see the tables in the database by using the `.tables` command
			use `PRAGMA table_info(customers);` to see the table information.
			use `SELECT * FROM customers;` to dump the information from the table
			
*Injection*
- Injection flaws are very common in applications today. These flaws occur because the application interprets user-controlled input as commands or parameters.
	- **SQL Injection:** This occurs when user-controlled input is passed to SQL queries.
	- **Command Injection:** This occurs when user input is passed to system commands.
	
 *Insecure Design*
 -  refers to vulnerabilities which are inherent to the application's architecture. They are not vulnerabilities regarding bad implementations or configurations, but the idea behind the whole application (or a part of it) is flawed from the start.

*Security Misconfiguration*
- distinct from the other Top 10 vulnerabilities because they occur when security could have been appropriately configured but was not. Even if you download the latest up-to-date software, poor configurations could make your installation vulnerable.

*Vulnerable and Outdated Components*
- using a program with a well-known vulnerability. If a company misses a single update for a program they use, it could be vulnerable to any number of attacks.

*Identification and Authentication Failures*
- If an attacker is able to find flaws in an authentication mechanism, they might successfully gain access to other users' accounts. Some common flaws in authentication mechanisms include the following:
	- Brute force attacks
	- Use of weak credentials
	- Weak Session Cookies


*Software and Data Integrity Failures*
- This vulnerability arises from code or infrastructure that uses software or data without using any kind of integrity checks. Since no integrity verification is being done, an attacker might modify the software or data passed to the application, resulting in unexpected consequences. There are mainly two types of vulnerabilities in this category:
	- Software Integrity Failures
	- Data Integrity Failures

*Security Logging & Monitoring Failures*
- are security vulnerabilities that can occur when a system or application fails to log or monitor security events properly. This can allow attackers to gain unauthorized access to systems and data without detection.

*Server-Side Request Forgery (SSRF)*
- This type of vulnerability occurs when an attacker can coerce a web application into sending requests on their behalf to arbitrary destinations while having control of the contents of the request itself. SSRF vulnerabilities often arise from implementations where our web application needs to use third-party services.




