## Classic Authentication Attack
Authentication attacks target the authentication mechanisms of a system, application, or API with the goal of bypassing or compromising authentication controls to gain unauthorized access. These attacks exploit weaknesses or vulnerabilities in the authentication process to impersonate legitimate users, escalate privileges, or perform unauthorized actions.

Classic authentication attacks include techniques that have been around for a while like *brute forcing* and *password spraying*, and others...

##### Brute Force Attack
- An attacker attempts to guess valid usernames and passwords by systematically trying different combinations until the correct credentials are found.
- **Countermeasures**: Implement account lockout mechanisms, use strong and complex passwords, and employ rate-limiting to prevent brute force attacks.

  Brute-forcing an API’s authentication is not very different from any other brute-force attack, except you’ll send the request to an API endpoint, the payload will often be in JSON, and the authentication values may require base64 encoding.

	there are suitable tools to perform the attack:
	- ***Burp suite intruder*** 
	- ***Wfuzz*** - is a powerful command-line tool used for web application brute-forcing and fuzzing.
		- allows users to fuzz web applications by replacing parts of URLs, headers, parameters, and POST data with custom payloads (such as lists of common strings, numbers, or specific payloads for known vulnerabilities).
		 e.g: `wfuzz -c -z file,wordlist.txt --hc 404 https://example.com/FUZZ`

		 `Important items to note for API testing include the headers option (-H), hide responses (--hc, --hl, --hw, --hh), and POST body requests (-d). All of these will be useful when fuzzing APIs.`
		
##### Password Spraying
- Attackers attempt a small number of commonly used passwords against many accounts, aiming to avoid account *lockouts* and *detection*.
- **Countermeasures**: Enforce account lockout policies, monitor and analyze failed login attempts, and implement multi-factor authentication (MFA) to mitigate this attack.
##### Credential Stuffing
- Attackers use lists of previously compromised credentials (e.g., obtained from data breaches) to automate login attempts on other services.
- **Countermeasures**: Implement multi-factor authentication (MFA), monitor login attempts for suspicious patterns, and educate users about password hygiene.


## API Token Attacks
API token attacks refer to various types of security threats and attacks targeting the authentication tokens used in APIs. API tokens are commonly used for authentication and authorization purposes in web and mobile applications. These tokens are typically issued to users or applications after successful authentication and are used to authenticate subsequent requests to the API.

### Token Analysis
It involves examining the *characteristics*, *usage patterns*, and *security implications* of tokens used for authentication and authorization in APIs.
When implemented correctly, tokens can be an excellent tool that can be used to authenticate and authorize users. However, if anything goes wrong when generating, processing, or handling tokens, they can become our keys to the kingdom.

###### some common API token attacks
**Stolen Token:** If an attacker manages to steal a valid API token, they can impersonate the legitimate user or application associated with that token and perform unauthorized actions. This can happen through various means such as intercepting network traffic, exploiting vulnerabilities in the application, or obtaining tokens from compromised devices.
    
**Brute Force Attacks:** Attackers may attempt to guess or brute force API tokens by trying different combinations until they find a valid token. This can be particularly effective if the tokens are not sufficiently long or complex.
    
**Token Leakage:** If sensitive information such as API tokens is inadvertently exposed in logs, error messages, or response headers, attackers may exploit this information leakage to obtain valid tokens and launch attacks.


### JWT Attacks
JSON Web Tokens (JWTs) are one of the most prevalent API token types because they operate across a wide variety of programming languages, including Python, Java, Node.js, and Ruby. These tokens are susceptible to all sorts of misconfiguration mistakes that can leave the tokens vulnerable to several additional attacks. These attacks could provide you with sensitive information, grant you basic unauthorized access, or even administrative access to an API.

JWTs consist of three parts, all of which are base64 encoded and separated by periods: the **header**, **payload**, and **signature**. [JWT.io](https://jwt.io/) is a free web JWT debugger that you can use to check out these tokens. It begin with =="ey"== because that is what happens when you base64 encode a curly bracket followed by a quote.

###### JWTs are susceptible to various attacks. some of them are:
- **JWT Token Leakage** If JWTs are not properly protected, attackers may attempt to steal them through various means such as Cross-Site Scripting (XSS) vulnerabilities, session fixation attacks, or interception of network traffic.
- **JWT Token Tampering:** Attackers may attempt to tamper with JWTs to modify their contents like(user ID, role, permissions...) and gain unauthorized access or privileges.
- **JWT Token Forgery:** Attackers may attempt to forge or create counterfeit JWTs to impersonate legitimate users or applications.
- **WT Token Expiration Bypass:** If JWT expiration checks are not enforced properly, attackers may be able to bypass token expiration by tampering with the token's expiration claim (exp) or by replaying expired tokens.
- **Algorithm Substitution Attacks:** If an application allows multiple JWT signature algorithms, attackers may attempt to exploit vulnerabilities in weaker algorithms (e.g., HMAC-SHA1) to forge or tamper with JWTs.

- **Brute Force Attacks:**
    - Attackers may attempt to brute force JWT *signatures* or *encryption keys* to create valid JWTs or decrypt encrypted tokens.
    - This is particularly dangerous if weak or predictable keys are used for token signing or encryption.

### Automating JWT attacks with **JWT_Tool**
 The JSON Web Token Toolkit or JWT_Tool is a great command line tool that we can use for analyzing and attacking JWTs. With this, we will be able to analyze JWTs, scan for *weaknesses*, *forge tokens*, and *brute-force signature* secrets.
 
Some of the options to note include:
- -h           to show more verbose help options
- -t            to specify the target URL
- -M          to specify the scan mode
    - pb         to perform a playbook audit (default tests) -  used to target a web application and scan for common JWT vulnerabilities.
    - at          to perform all tests
- -rc          to add request cookies
- -rh          to add request headers
- -pd         to add POST data
e.g: `jwt_tool -t [target API/URL] -rh "Authorization: Bearer [JWT_Token]" -M pb`

**for more info you can look at `jwt_tool -h`**

#### The None Attack
The "None" attack, also known as the "none algorithm" or "none signature," is a vulnerability associated with JSON Web Tokens (JWTs). It occurs when a JWT is signed using the "none" algorithm, meaning the token is not signed at all.
It occurs when an attacker crafts a JWT with the algorithm set to "none" and includes arbitrary or manipulated claims in the token. Since the token is not signed, the server may mistakenly accept it as valid, assuming it has been signed with the "none" algorithm.

#### The Algorithm Switch Attack
The Algorithm Switch Attack targets systems that use JSON Web Tokens (JWTs) for authentication. Attackers exploit vulnerabilities in the JWT verification process to trick the system into accepting JWTs with altered algorithms.

**No Signature:** Attackers attempt to send a JWT without a signature by removing it from the token and leaving the last period intact.
    
**"None" Algorithm:** If the first method fails, attackers try changing the algorithm header field to "none" to create a token without a signature.
    
**Algorithm Substitution:** Attackers may alter the algorithm from a secure one to a weaker one (e.g., from RS256 to HS256) if the system accepts multiple algorithms without proper validation.
#### JWT Crack Attack
The JWT Crack attack attempts to crack the secret used for the JWT signature hash, giving us full control over the process of creating our own valid JWTs.
 Hash-cracking attacks like this take place offline and do not interact with the provider. Therefore, we do not need to worry about causing havoc by sending millions of requests to an API provider. You can use JWT_Tool or a tool like Hashcat to crack JWT secrets.
 
 We can use ***Crunch***, a password-generating tool, to create a list of all possible character combinations to use against a target. see the detail in `man crunch`
 e.g: `crunch 1 8 -o [filename]`
 - crunch will generate a wordlist that starts at *a* and ends at *zzzzzzzz* and write the wordlists to a file named `[filename]`
 
 To perform a JWT Crack attack using JWT_Tool, use the following command:
 ` jwt_tool [TOKEN] -C -d [wordlist]`
- -C - indicates conducting a hash crack attack(crack key for an HMAC-SHA token)
- -d - specifies the dictionary or wordlist you’ll be using against the hash

