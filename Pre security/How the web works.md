##### How websites work?
Websites are digital spaces hosted on servers that store and deliver web pages and associated content to users over the internet.

There are two major components that make up a website:
**Front End (Client-Side)**: The front end of a website refers to the part of the web application that users interact with directly in their web browsers.
- Key technologies used in front-end development include: **HTML**, **CSS**, **JavaScript**.
**Back End (Server-Side)**: The back end of a website comprises the server-side components responsible for handling requests from clients (such as web browsers), processing data, and generating responses to be sent back to the client.

*HTML Injection*
HTML Injection is a vulnerability that occurs when unfiltered user input is displayed on the page. If a website fails to sanitise user input (filter any "malicious" text that a user inputs into a website), and that input is used on the page, an attacker can inject HTML code into a vulnerable website.
Developers should follow security best practices such as using Content Security Policy (CSP) headers, sanitizing user input to mitigate the risk.

### DNS
- It translates human-readable domain names into IP addresses, which are numerical identifiers for computer systems on a network. 
-  ***Domain Hierarchy*** 
	- **TLD (Top-Level Domain)** - is the most righthand part of a domain name.
	- ***Second-Level Domain*** - is a part of a domain name that is located directly to the left of TLD.
	- ***Subdomain*** - sits on the left-hand side of the Second-Level Domain using a period to separate it.

- ***DNS Record Types***
	- **A Record** - These records resolve to IPv4 addresses.
	- **AAAA Record** - These records resolve to IPv6 addresses.
	- **CNAME Record** - resolve to another domain name.
	- **MX Record** - resolve to the address of the servers.
	- **TXT Record** - TXT records are free text fields where any text-based data can be stored.
### HTTP
- It is the set of rules used for communicating with web servers for the transmitting of webpage data.
- ***HTTPS*** is the secure version of HTTP.
#### URL
- Uniform Resource Locator - It is a web address used to specify the location of a resource on the internet.
	- The below image shows what a URL looks like with all of its features (it does not use all features in every request).
	![A diagram showing different parts of a URL on an example, where http is the scheme, user:password is the user, tryhackme.com is a domain or the host, 80 is the port, view-room is the path, ?id=1 is the query string, and #task3 is the fragment. The full address is http://user:password@tryhackme.com:80/view-room?id=1#task3.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5c549500924ec576f953d9fc/room-content/34ad66d8b90aaaa35f9536d3b152ea97.png)
	
	- **Scheme** It is instructs on what ==protocol== to use for accessing the resource.
	- **User** Some services require authentication to log in.  
	- **Host** it is domain name or IP address of the server you wish to access.  
	- **Port** It Port that you are going to connect to.
	- **Path** It is file name or location of the resource you are trying to access.  
	- **Query String** Extra bits of information that can be sent to the requested path.
	- **Fragment** It is a reference to a location on the actual page requested.

#### Request and Response
request and response are fundamental concepts that describe the communication between a client (typically a web browser) and a server.
- **Request**: When you type a URL into your web browser and press Enter, your browser sends an HTTP request to the server hosting the requested resource (web page, image, file, etc.).
- **Response**: Upon receiving the request, the server processes it and sends back an HTTP response to the client. This response includes different information.


#### HTTP Methods
- Those are used to indicate the desired action to be performed on a resource identified by a given URL.
- Those are commands/ways used by clients to communicate with web servers. some most common methods are GET/POST/PUT/DELETE request.

#### HTTP Status codes
- Those are three-digit numbers that the server sends to the client in response to a request made to the server.
- These status codes can be broken down into 5 different ranges:
	- **(1xx)** 100-199 - Information Response
	- **(2xx)** 200-299 - Success
	- **(3xx)** 300-399 - Redirection
	- **(4xx)** 400-499 - Client Errors
	- **(5xx)** 500-599 - Server Errors

###### Common HTTP Status Codes:

|                                  |                                                                                                                                                                                                                               |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **200 - OK**                     | The request was completed successfully.                                                                                                                                                                                       |
| **201 - Created**                | A resource has been created (for example a new user or new blog post).                                                                                                                                                        |
| **301 - Moved Permanently**      | This redirects the client's browser to a new webpage or tells search engines that the page has moved somewhere else and to look there instead.                                                                                |
| **302 - Found**                  | Similar to the above permanent redirect, but as the name suggests, this is only a temporary change and it may change again in the near future.                                                                                |
| **400 - Bad Request**            | This tells the browser that something was either wrong or missing in their request. This could sometimes be used if the web server resource that is being requested expected a certain parameter that the client didn't send. |
| **401 - Not Authorised**         | You are not currently allowed to view this resource until you have authorised with the web application, most commonly with a username and password.                                                                           |
| **403 - Forbidden**              | You do not have permission to view this resource whether you are logged in or not.                                                                                                                                            |
| **405 - Method Not Allowed**     | The resource does not allow this method request, for example, you send a GET request to the resource /create-account when it was expecting a POST request instead.                                                            |
| **404 - Page Not Found**         | The page/resource you requested does not exist.                                                                                                                                                                               |
| **500 - Internal Service Error** | The server has encountered some kind of error with your request that it doesn't know how to handle properly.                                                                                                                  |
| **503 - Service Unavailable**    | This server cannot handle your request as it's either overloaded or down for maintenance.                                                                                                                                     |

#### Headers
- headers are key-value pairs sent between a client (e.g., a web browser) and a server (e.g., a web server) to provide additional information about the request or the response.

- ***Common Request Headers*** 
	Sent by the client to the server along with the request.
	- **Host**
	- **User-Agent**
	- **Content-Length**
	- **Accept-Encoding**
	- **Cookie**
- ***Common Response Headers***
	- **Set-Cookie**
	- **Cache-Control**
	- **Content-Type**
	- **Content-Encoding**
	
#### Cookies
- It is a small piece of data that a web server sends to a user's web browser. The browser then stores this data and sends it back with subsequent requests to the same server.
