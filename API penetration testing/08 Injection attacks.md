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
