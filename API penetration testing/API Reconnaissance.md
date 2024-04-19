### what is API?
An **API**(*Application Programming Interface*), is a set of rules and protocols that allows different software applications to communicate and interact with each other. APIs define the ==methods== and ==data formats== that applications can use to request and exchange information. Think of an API as a bridge that enables two different systems or applications to talk to each other and work together seamlessly.

## API reconnaissance
It refers to the process of gathering information and understanding the characteristics, functionalities, and security aspects of an API.
- It is the process of actively inspecting and gathering information about APIs used by a web application. It involves identifying various *endpoints, methods, parameters, and authentication mechanisms* that an API supports.
-  This reconnaissance is typically performed by developers, security researchers, or testers to gain insights into how an API operates and how it can be interacted with.


In the context of API reconnaissance testing or security assessments, "passive" and "active" refer to different approaches and techniques used to gather information, analyse traffic, and identify potential vulnerabilities.
#### Passive Reconnaissance
It involves collecting information and analysing data without directly interacting with the target system or API. It focuses on gathering publicly available information and observing network traffic passively. The goal of passive reconnaissance is to gain insights into the target's infrastructure, configuration, and behaviour without triggering any alarms or alerts.
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
		
#### Active Reconnaissance
It involves interacting directly with the target system or API to gather information, perform tests, and analyze responses actively. This approach may involve sending requests, probing endpoints, and interacting with the API to identify vulnerabilities or misconfigurations.
##### Techniques used in Active Reconnaissance
- Endpoint Enumeration
- Fuzzing and Input Validation Testing
- Authentication Testing
- Authorization Testing
- Error Handling Testing
- Rate Limiting and Throttling Testing


