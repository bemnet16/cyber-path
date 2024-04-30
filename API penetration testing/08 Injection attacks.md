# Injection Attacks
Injection attacks is a security vulnerabilities where an attacker injects malicious code or data into an application, typically through user-controllable input fields. These attacks exploit weaknesses in the application's input validation or inadequate handling of external data.

### Testing for Injection Vulnerabilities
Before you can inject a payload using an API, you must uncover the best requests to attack. The best way to discover these injection points is by fuzzing and then analyzing the responses you receive. ***Fuzzing APIs*** is the process of sending various types of input to an endpoint to provoke an unintended response. The payloads used for fuzzing include symbols, numbers, system commands, SQL queries, NoSQL queries, emojis, hexadecimal, boolean statements, and more. Essentially, you want any payload that the API may not be programmed to handle to cause the API to send a verbose response or to cause the application to behave adversely.

We should attempt fuzzing against all potential inputs and especially within the following:
- Headers
- Query string parameters
- Parameters in POST/PUT requests

### Discovering Injection Vulnerabilities
involves identifying security weaknesses in an application that could be exploited by injecting malicious code or data.

Fuzzing is like exploring the edges and corners of a system to find weak spots. You're essentially throwing unexpected inputs at it to see how it reacts. To do it effectively, you need to be strategic about where you're fuzzing (like user inputs, headers, or URLs) and what you're fuzzing with (like large numbers, strings, or random characters).

Fuzzing is a technique used to find vulnerabilities in software by sending unexpected inputs. To effectively fuzz an API, it's important to know where and what to fuzz.

When reviewing API documentation, if the API is expecting a certain type of input (number, string, boolean value) send:
- A very large number
- A very large string
- A negative number
- A string (instead of a number or boolean value)
- Random characters
- Boolean values
- Meta characters

By sending over this input we are testing the limits of the target's input validation. If a certain type of input causes a verbose error or causes a delayed response then you could be on the trail of an injection vulnerability.


##### SQL Injection Meta-characters
Those are special characters used in SQL queries to perform malicious actions when injected into vulnerable SQL statements. These characters have special meanings in SQL syntax and can be exploited by attackers to manipulate the behavior of the SQL query.

Here are some *SQL metacharacters* that can cause some issues:
- `'` - used to terminate and manipulate SQL queries.
- `''`
- `;%00` - (null byte) - could cause a verbose SQL-related error to be sent as a response.
- `--` -  represent the beginning of a single-line comment.
- `-- -`
- `""` - Double quotes to manipulate SQL queries similarly to single quotes.
- `;` - Statement terminator in SQL queries.
- `' OR '1` - Payload for Boolean-based SQLi, attempting to make a condition always true.
- `' OR 1 -- -` - Injects a true condition to bypass authentication.
- `" OR "" = "` - Similar to previous but using double quotes instead of single quotes.
- `" OR 1 = 1 -- -` - Injects a true condition using double quotes.
- `' OR '' = '`
- `OR 1=1` - logical operations always true.

##### NoSQL Injection
NoSQL injection is a type of injection attack specific to NoSQL databases, where malicious input is used to manipulate the logic of database queries.

NoSQL injection techniques aren’t as well-known as their structured counterparts. Due to this one small fact, you might be more likely to find NoSQL injections.

Remember that NoSQL databases do not share as many commonalities as the different SQL databases do.

The following are common NoSQL metacharacters you could send in an API request to manipulate the database:

- `$gt` - selects documents that are greater than the provided value.
- `{"$gt":""}` - used to exploit type coercion vulnerabilities.
- `{"$gt":-1}` - select documents where the field value is greater than -1.
- `$ne` - selects documents where the value is not equal to the provided value.
- `{"$ne":""}` - test for non-existence or inequality.
- `{"$ne":-1}` - select documents where the field value is not equal to -1.
- `$nin` - select documents where the field value is not within the specified array.
- `{"$nin":1}` - potentially to exclude documents with that value.
- `{"$nin":[1]}` - similar to the previous but with array notation.
- `{"$where":  "sleep(1000)"}` - to execute arbitrary JavaScript code, potentially causing a server delay or denial of service (DoS) attack.

##### OS Injection (command injection)
It is a type of security vulnerability that occurs when an attacker is able to execute arbitrary operating system commands on a server or system through a vulnerable application. This vulnerability arises when an application accepts user-supplied input and uses it to construct a system command without proper validation or sanitization.

Attackers exploit OS injection vulnerabilities by inserting malicious commands into input fields or parameters that are then executed by the underlying operating system. These commands can range from simple system commands (such as ls, dir) to more malicious ones (such as rm -rf \*, del ., or format).

Common vectors for OS injection include **web forms**, **query parameters**, and **HTTP headers**.

Characters such as the following all act as command separators, which enable a program to pair multiple commands together on a single line.
- `|`
- `||`
- `&`
- `&&`
- `'`
- `"`
- `;`
- `'"`


## Mitigation mechanism

1. **Input Validation and Sanitization**: Implement strict input validation to ensure that user-supplied input meets expected formats and constraints. Sanitize input to remove any potentially dangerous characters or commands. Use input validation libraries or frameworks to automate this process.
    
2. **Parameterized Queries**: Utilize parameterized queries or prepared statements when interacting with databases to separate SQL code from data. Parameterized queries ensure that user input is treated as data rather than executable code, preventing SQL injection vulnerabilities.
    
3. **ORMs and Safe APIs**: Use Object-Relational Mapping (ORM) libraries or secure APIs provided by frameworks to interact with databases. These libraries abstract away low-level database operations and automatically handle input validation and parameterization.
    
4. **Least Privilege Principle**: Limit the privileges of database accounts and application processes to only those necessary for their intended functionality. This reduces the potential impact of successful injection attacks by limiting the attacker's access to sensitive resources.
    
5. **Regular Security Audits and Code Reviews**: Conduct regular security audits and code reviews to identify and remediate vulnerabilities in the application codebase. Automated scanning tools and manual penetration testing can help detect injection vulnerabilities and other security issues.
    
6. **Security Headers and Configuration**: Implement security headers (e.g., Content Security Policy, X-Content-Type-Options) and server configurations (e.g., secure HTTP headers, input/output encoding) to mitigate the risk of injection attacks and other web security vulnerabilities.
    
7. **Web Application Firewalls (WAFs)**: Deploy WAFs to monitor and filter incoming traffic for suspicious patterns and known attack signatures. WAFs can help block malicious requests before they reach the application, providing an additional layer of defense against injection attacks.
    
8. **Education and Training**: Educate developers and system administrators about the risks of injection attacks and best practices for secure coding and configuration. Training programs should cover topics such as input validation, parameterized queries, and secure coding guidelines.


