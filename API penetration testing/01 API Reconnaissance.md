### what is API?
An **API**(*Application Programming Interface*), is a set of rules and protocols that allows different software applications to communicate and interact with each other. APIs define the ==methods== and ==data formats== that applications can use to request and exchange information. Think of an API as a bridge that enables two different systems or applications to talk to each other and work together seamlessly.

## API reconnaissance
It refers to the process of gathering information and understanding the characteristics, functionalities, and security aspects of an API.
- It is the process of actively inspecting and gathering information about APIs used by a web application. It involves identifying various *endpoints, methods, parameters, and authentication mechanisms* that an API supports.
-  This reconnaissance is typically performed by developers, security researchers, or testers to gain insights into how an API operates and how it can be interacted with.


In the context of API reconnaissance testing or security assessments, "passive" and "active" refer to different approaches and techniques used to gather information, analyse traffic, and identify potential vulnerabilities.
#### Passive Reconnaissance
It involves collecting information and analyzing data without directly interacting with the target system or API. It focuses on gathering publicly available information and observing network traffic passively. The goal of passive reconnaissance is to gain insights into the target's infrastructure, configuration, and behavior without triggering any alarms or alerts.
##### Techniques used in Passive Reconnaissance
- DNS Analysis
- Public Information Gathering
- Network Traffic Monitoring - use tools like *Wireshark, tcpdump*
- OSINT (Open Source Intelligence) - `data collected from publicly available sources`
- Passive Scanning - using tools like *Shodan or Censys
- dorking
	**Google Dorking**
	 refers to the use of advanced search techniques in Google (and other search engines) to locate specific information on the internet that may not typically be readily accessible through normal search queries.

| **Google Dorking Query**                                | **Expected results**                                                                                                                              |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| inurl:"/wp-json/wp/v2/users"                            | Finds all publicly available WordPress API user directories.                                                                                      |
| intitle:"index.of" intext:"api.txt"                     | Finds publicly available API key files.                                                                                                           |
| inurl:"/api/v1" intext:"index of /"                     | Finds potentially interesting API directories.                                                                                                    |
| ext:php inurl:"api.php?action="                         | Finds all sites with a XenAPI SQL injection vulnerability. (This query was posted in 2016; four years later, there are currently 141,000 results. |
| intitle:"index of" api_key OR "api key" OR apiKey -pool | This is one of my favorite queries. It lists potentially exposed API keys.                                                                        |
	 **git Dorking**
		It is practice of using advanced search techniques to discover sensitive information or vulnerabilities in Git repositories that have been exposed or leaked online. This technique involves crafting specific search queries (known as "dorks") to uncover Git repository URLs or contents that are unintentionally made public or accessible.
	**TruffleHog**
		TruffleHog is a great tool for automatically discovering exposed secrets. It used in cyber-security for searching and detecting sensitive data that may have been inadvertently exposed within version control systems like Git repositories. 
		You can simply use the following Docker run to initiate a TruffleHog scan of your target's GitHub.
		`$ sudo docker run -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --org=target-name`
	 **Shodan**
		 Shodan is the go-to search engine for devices accessible from the internet. It use to discover external-facing APIs and get information about your target’s open ports, making it useful if you have only an IP address or organization’s name to work from.

| **Shodan Queries**               | **Purpose**                                                                                                                                                                                                  |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| hostname:"targetname.com"        | Using hostname will perform a basic Shodan search for your target’s domain name. This should be combined with the following queries to get results specific to your target.                                  |
| "content-type: application/json" | APIs should have their content-type set to JSON or XML. This query will filter results that respond with JSON.                                                                                               |
| "content-type: application/xml"  | This query will filter results that respond with XML.                                                                                                                                                        |
| "200 OK"                         | You can add "200 OK" to your search queries to get results that have had successful requests. However, if an API does not accept the format of Shodan’s request, it will likely issue a 300 or 400 response. |
| "wp-json"                        | This will search for web applications using the WordPress API.                                                                                                                                               |
	 **Wayback Machine**
		 The Wayback Machine is an archive of various web pages over time. It allows users to browse and access archived versions of web pages as they appeared at different points in time. This is great for passive API reconnaissance because this allows you to check out historical changes to your target.
#### Active Reconnaissance
It involves interacting directly with the target system or API to gather information, perform tests, and analyze responses actively. This approach may involve sending requests, probing endpoints, and interacting with the API to identify vulnerabilities or mis-configurations.
##### Techniques used in Active Reconnaissance
- Endpoint Enumeration
- Fuzzing and Input Validation Testing
- Authentication Testing
- Authorization Testing
- Error Handling Testing
- Rate Limiting and Throttling Testing

###### During active recon we will use tools like:
- **Nmap** - is a powerful tool for scanning ports, searching for vulnerabilities, enumerating services, and discovering live hosts.
- **OWASP Amass** - is a command-line tool that can map a target’s external network by collecting OSINT from over 55 different sources.
	- Use the intel command to collect SSL certificates
		-  Use the intel command to collect SSL certificates
			- `amass intel -addr [target IP addresses]`
			- These domains can then be passed to intel with the `whois` option to perform a reverse `Whois` lookup.
			- `amass intel -d [target domain] –whois`
		-  Once you have a list of interesting domains, upgrade to the `enum` sub-command to begin enumerating subdomains.
			- passive recon - ` amass enum -passive -d [target domain]`
			- active recon - `amass enum -active -d [target domain]`
		-  add the -brute option to brute-force subdomains
			- `amass enum -active -brute -w [wordlist] -d [target domain] -dir [directory name]`
- **Kiterunner** - is an excellent tool that was developed and released by Assetnote. It is currently the best tool available for discovering API endpoints and resources. While directory brute force tools like Gobuster/Dirbuster/ work to discover URL paths, it typically relies on standard HTTP GET requests. Kiterunner will not only use all HTTP request methods common with APIs (GET, POST, PUT, and DELETE) but also mimic common API path structures. In other words, instead of requesting GET /api/v1/user/create, Kiterunner will try POST /api/v1/user/create, mimicking a more realistic request.
	- You can perform a quick scan of your target’s URL or IP address like this:
		 `kr scan [target] -w [(.kite file)]`
		 If you want to use a text wordlist rather than a .kite file
			 `kr brute <target> -w [wordlist]`
			 
		 

