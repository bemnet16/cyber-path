# Evasive Maneuvers
Evasive maneuvers in API penetration testing refer to techniques used by testers to mimic malicious actors attempting to exploit vulnerabilities in an API (Application Programming Interface) without triggering alarms or detection mechanisms. These maneuvers involve various strategies to evade detection by security measures while probing the API for weaknesses.

Many APIs in the wild will not be exploited easily.  Often times security controls like web applications firewalls (WAFs) and rate-limiting can block your attacks. Security controls may differ from one API provider to the next, but at a high level, they will have some threshold for malicious activity that will trigger a response. WAFs, for example, can be triggered by a wide variety of things, like:

- Too many requests for resources that do not exist.
- Too many requests within a certain amount of time
- Common attack attempts, like SQL injection and XSS attacks
- Abnormal behavior, like tests for authorization vulnerabilities


The following measures can be effective at evading or bypassing these restrictions.
##### String Terminators
String terminators are characters or sequences of characters used to signify the end of a string in programming languages or data formats. They are essential for delineating where a string begins and ends, which is crucial for correctly interpreting and manipulating data.

Null bytes and other combinations of symbols are often interpreted as string terminators used to end a string. If these symbols are not filtered out, they could terminate the API security control filters that may be in place. Here is a list of potential string terminators you can use:

- %00
- 0x00
- //
- ;
- %
- !
- ?
- \[]
- %5B%5D
- %09
- %0a
- %0b
- %0c
- %0e

String terminators can be placed in different parts of the request, like the path or POST body, to attempt to bypass any restrictions in place.

  
##### Case Switching
Case switching, also known as case conversion or case mapping, refers to the process of changing the case (uppercase or lowercase) of characters within a string.

Sometimes API security controls are pretty easy to beat. If a security control is built around the literal spelling and case of the components within a request, then case switching can be an effective technique to bypass the controls. Case switching is the literal switching of the case of the letters within the URL path or payload.

e.g: 
	POST /api/myProfile 
	POST /api/MyProfile 
	POST /aPi/MypRoFiLe


##### Encoding Payloads
Encoded payloads can often trick WAFs while still being processed by the target application or database. Even if the WAF or an input-validation measure blocks certain characters or strings, they might miss encoded versions of those characters. Alternatively, you could also try double encoding your attacks.




## Combining Techniques

##### Plot Twist
- Combined an excessive data exposure finding with a password-spraying attack to compromise accounts.
- Combined a Broken Function Level Authorization finding with a Mass Assignment attack to add some funds back to your crAPI account.
- Combined an Improper Assets Management vulnerability to exploit a successful brute force attack against a multi-factor authentication system.

##### Suggestions for Combining Vulnerabilities and Techniques
Here are many different possible ways to combine API attack techniques. One thing that we may not realize is that once there is a small crack in the security of an API then we may have found a launchpad for the rest of our attack.

Excessive Data Exposure and Improper Assets Management are both API weaknesses that stand out above the others to combine in order to escalate the impact of the attacks.

- Excessive data exposure is a vulnerability that can be leveraged in many other attacks. Excessive Data Exposure can be a great source of information about users and administrators. Make sure that you take note of all parameters leaked in exposure, such as username, email, privilege level, and unique user identifiers. All of this information could be challenging to come across otherwise but can be leveraged in authentication and authorization attacks.

- Improper Assets Management vulnerability is a weakness that should be combined with all other attack techniques. Improper Assets Management implies that a version of the API may not be as supported as the current version and thus it may not be as protected. Perform all of the other attacks against the unmanaged endpoint. You may find that the security of a api/test/login, api/mobile/login, api/v1/login, api/v1/internal/users may have fewer protections than the supported counterparts. With Improper Assets Management findings there could be lax rate-limiting, missing protections for brute force attempts, missing input validation, unlimited multifactor authentication requests, authorization vulnerabilities, etc. With APIs affected by Improper Assets Management vulnerabilities you never know what you’re going to get. So, make sure to perform the full gambit of attack techniques against unsupported versions of the API.

- If you are testing a file upload function, attempt to use the directory traversal fuzzing attack along with the file upload to manipulate where the file is stored. Upload files that lead to a shell and attempt to execute the uploaded file with the corresponding web application.

- Perhaps you have discovered a Broken Function Level Authorization (BFLA) or Mass Assignment weakness that has given you access to administrative functionality. There is a chance that an API provider trusts its administrators and does not have as many technical protections in place. You could then use this new level of access to search for Improper Assets Management, injection weaknesses, or Broken Object Level Authorization weaknesses.

- If you are able to perform a mass assignment, then there is a chance that the parameter or parameters that you can alter may not be protected from injection attacks and/or Server-side Request Forgery. When you have discovered mass assignment fuzz the parameter for other weaknesses.

- If you are able to update to perform a BFLA attack and update another user’s profile information perhaps you could combine this with an XSS attack.

- Combine Injection attacks with most other vulnerabilities. For example, if you are able to manipulate a JSON Web Token. Consider including various different types of injection attacks into the JSON Web Token.

- Consider ways to fully leverage BOLA findings. Use the information that you are able to access to gain access elsewhere. Always test to see if your BOLA discovery can be used in a BFLA attack. In other words, if you find a BOLA vulnerability that provides you with other users’ bank account information check to see if you are also able to exploit a BFLA vulnerability to transfer funds.

- If you are up against API security controls like a WAF or rate-limiting, make sure to apply evasion techniques to your attacks. Encode your attacks and attempt to find ways to bypass these security measures. If you have exhausted your testing efforts, return to fuzzing wide with encoded and obfuscated attacks.