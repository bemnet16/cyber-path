# Mass Assignment Attacks
It is a security vulnerability that occurs when an application assigns user-controlled input to model attributes without proper validation. This type of attack exploits the ability of attackers to manipulate data sent to the application to modify or create objects they shouldn't have access to.

A few things need to be in play for this to happen. An API must have requests that accept user input, these requests must be able to alter values not available to the user, and the API must be missing security controls that would otherwise prevent the user input from altering data objects.

## Finding Mass Assignment Vulnerabilities
One of the ways that you can discover mass assignment vulnerabilities by finding interesting parameters in API documentation and then adding those parameters to requests. Look for parameters involved in user account properties, critical functions, and administrative actions.

Additionally, make sure to use the API as it was designed so that you can study the parameters that are used by the API provider. Doing this will help you understand the names and spelling conventions of the parameters that your target uses. If you find parameters used in some requests, you may be able to leverage those in your mass assignment attacks in other requests.You can also test for mass assignment blind by fuzzing parameter values within requests. Mass assignment attacks like this will be necessary when your target API does not have documentation available.


## Mitigation mechanisms
To mitigate mass assignment attacks, several countermeasures can be implemented:

1. **Input Validation**: Validate and sanitize all incoming user-controlled input to ensure that it adheres to expected formats and values. This helps prevent attackers from injecting unexpected data into requests.
    
2. **Parameter Whitelisting**: Explicitly define which parameters are allowed to be modified through user input and reject any additional parameters that are not explicitly whitelisted. This ensures that only authorized attributes can be modified.
    
3. **Use of Safe Defaults**: Set default values for attributes that should not be directly modifiable by users. This can prevent unintended changes to critical data.
    
4. **Role-Based Access Control (RBAC)**: Implement RBAC mechanisms to restrict access to sensitive functionality and data based on the roles and permissions of authenticated users. This ensures that only authorized users can perform specific actions.
    
5. **Separation of Concerns**: Separate user input handling from business logic and data manipulation. By decoupling these components, you can enforce stricter controls over which attributes can be modified by users.
    
6. **Security Headers**: Utilize security headers, such as Content Security Policy (CSP), to mitigate against certain types of attacks, including cross-site scripting (XSS), which could be used in conjunction with mass assignment vulnerabilities to execute malicious scripts.
    
7. **Regular Security Audits and Testing**: Conduct regular security audits and penetration testing to identify and address vulnerabilities, including mass assignment vulnerabilities. This should include both automated scanning tools and manual testing by security professionals.
    
8. **Secure Coding Practices**: Train developers on secure coding practices and emphasize the importance of validating and sanitizing user input. Incorporate security reviews into the software development lifecycle to catch vulnerabilities early in the development process.