## Gobuster
There are some useful Global flags that can be used as well. I've included them in the table below. You can review these in the main documentation as well - [here](https://github.com/OJ/gobuster).

| Flag | Long Flag     | Description                               |
| ---- | ------------- | ----------------------------------------- |
| -t   | --threads     | Number of concurrent threads (default 10) |
| -v   | --verbose     | Verbose output                            |
| -z   | --no-progress | Don't display progress                    |
| -q   | --quiet       | Don't print the banner and other noise    |
| -o   | --output      | Output file to write results to           |
#### Gobuster mode
##### "dir" Mode
Dirbuster has a "dir" mode that allows the user to enumerate website directories. This is useful when you are performing a penetration test and would like to see what the directory structure of a website is.
`gobuster dir -u http://10.10.252.123/myfolder -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x.html,.css,.js`

These flags are useful in certain scenarios.  Note that these are not all of the flag options, but some of the more common ones that you'll use in penetration tests

|Flag|Long Flag|Description|
|---|---|---|
|-c|--cookies|Cookies to use for requests|
|-x|--extensions|File extension(s) to search for|
|-H|--headers|Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2'|
|-k|--no-tls-validation|Skip TLS certificate verification|
|-n|--no-status|Don't print status codes|
|-P|--password|Password for Basic Auth|
|-s|--status-codes|Positive status codes|
|-b|--status-codes-blacklist|Negative status codes|
|-U|--username|Username for Basic Auth|
The **-k flag** is special because it has an important use during penetration tests. if HTTPS is enabled, you will most likely encounter an invalid cert error.

#####  "dns" Mode
This tells Gobuster that you want to perform a sub-domain brute-force, instead of one of one of the other methods as previously mentioned.
`gobuster dns -d mydomain.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt`

-d and -w are the main flags that you'll need for _most_ of your scans. But there are a few others that are worth mentioning that we can go over. They are in the table below.

|Flag|Long Flag|Description|
|---|---|---|
|-c|--show-cname|Show CNAME Records (cannot be used with '-i' option)|
|-i|--show-ips|Show IP Addresses|
|-r|--resolver|Use custom DNS server (format server.com or server.com:port)|
##### "vhost" Mode
The last and final mode we'll focus on is the "vhost" mode. This allows Gobuster to brute-force virtual hosts. Virtual hosts are different websites on the same machine. In some instances, they can appear to look like sub-domains, but don't be deceived! Virtual Hosts are IP based and are running on the same server.
`gobuster vhost -u http://example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt`


## WPScan
The WPScan framework is capable of enumerating & researching a few security vulnerability categories present in WordPress sites - including - but not limited to:
- Sensitive Information Disclosure (Plugin & Theme installation versions for disclosed vulnerabilities or CVE's)
- Path Discovery (Looking for misconfigured file permissions i.e. wp-config.php)
- Weak Password Policies (Password bruteforcing)
- Presence of Default Installation (Looking for default files)
- Testing Web Application Firewalls (Common WAF plugins)

#### WPScan mode
**Enumerating for Installed Themes**
WPScan has a few methods of determining the active theme on a running WordPress installation.
`wpscan --url http://cmnatics.playground/ --enumerate t`

**Enumerating for Installed Plugins**
	WPScan can enumerate for common/known plugins.
	`wpscan --url http://cmnatics.playground/ --enumerate p`

**Enumerating for Users**
`wpscan --url http://cmnatics.playground/ --enumerate u`

**Performing a Password Attack**
After determining a list of possible usernames on the WordPress install, we can use WPScan to perform a bruteforcing technique against the username we specify and a password list that we provide. Simply, we use the output of our username enumeration to build a command like so: 
`wpscan –-url http://cmnatics.playground –-passwords rockyou.txt –-usernames cmnatic`

Summary - Cheatsheet

|            |                                                                                                       |                                |
| ---------- | ----------------------------------------------------------------------------------------------------- | ------------------------------ |
| Flag       | Description                                                                                           | Full Example                   |
| p          | Enumerate Plugins                                                                                     | --enumerate p                  |
| t          | Enumerate Themes                                                                                      | --enumerate t                  |
| u          | Enumerate Usernames                                                                                   | --enumerate -u                 |
| v          | Use WPVulnDB to cross-reference for vulnerabilities. Example command looks for vulnerable plugins (p) | --enumerate vp                 |
| aggressive | This is an aggressiveness profile for WPScan to use.                                                  | --plugins-detection aggressive |

## Nikto
It has made leaps and bounds over the years and has proven to be a very popular vulnerability scanner due to being both open-source nature and feature-rich. Nikto is capable of performing an assessment on all types of webservers. Nikto can be used to discover possible vulnerabilities including:

- Sensitive files
- Outdated servers and programs (i.e. [vulnerable web server installs](https://httpd.apache.org/security/vulnerabilities_24.html))
- Common server and software misconfigurations (Directory indexing, cgi scripts, x-ss protections)

#### Basic Scanning
     -> `nikto -h vulnerable_ip` 
This scan type will retrieve the headers advertised by the webserver or application (I.e. Apache2, Apache Tomcat, Jenkins or JBoss) and will look for any sensitive files or directories (i.e. login.php, /admin/, etc).

#### Scanning Multiple Hosts & Ports
 A much more common scenario will be scanning multiple ports on one specific host. We can do this by using the `-p` flag and providing a list of port numbers delimited by a comma - such as the following:
  `nikto -h 10.10.10.1 -p 80,8000,8080`

**Introduction to Plugins**

Plugins further extend the capabilities of Nikto. Using information gathered from our basic scans, we can pick and choose plugins that are appropriate to our target. You can use the `--list-plugins` flag with Nikto to list the plugins or [view the whole list in an easier to read format online](https://github.com/sullo/nikto/wiki/Plugin-list).

Some interesting plugins include:

|   |   |
|---|---|
|Plugin Name|Description|
|apacheusers|Attempt to enumerate Apache HTTP Authentication Users|
|cgi|Look for CGI scripts that we may be able to exploit|
|robots|Analyse the robots.txt file which dictates what files/folders we are able to navigate to|
|dir_traversal|Attempt to use a directory traversal attack (i.e. LFI) to look for system files such as /etc/passwd on Linux (http://ip_address/application.php?view=../../../../../../../etc/passwd)|

We can specify the plugin we wish to use by using the `-Plugin` argument and the name of the plugin we wish to use...For example, to use the "_apacheuser_" plugin, our Nikto scan would look like so: `nikto -h 10.10.10.1 -Plugin apacheuser`

**Verbosing our Scan**

We can increase the verbosity of our Nikto scan by providing the following arguments with the `-Display` flag. Unless specified, the output given by Nikto is not the entire output, as it can sometimes be irrelevant (but that isn't always the case!)

|   |   |   |
|---|---|---|
|Argument|Description|Reasons for Use|
|1|Show any redirects that are given by the web server.|Web servers may want to relocate us to a specific file or directory, so we will need to adjust our scan accordingly for this.|
|2|Show any cookies received|Applications often use cookies as a means of storing data. For example, web servers use sessions, where e-commerce sites may store products in your basket as these cookies. Credentials can also be stored in cookies.|
|E|Output any errors|This will be useful for debugging if your scan is not returning the results that you expect!|


**Tuning Your Scan for Vulnerability Searching**

Nikto has several categories of vulnerabilities that we can specify our scan to enumerate and test for. The following list is not extensive and only include the ones that you may commonly use. We can use the `-Tuning` flag and provide a value in our Nikto scan: 

|   |   |   |
|---|---|---|
|Category Name|Description|Tuning Option|
|File Upload|Search for anything on the web server that may permit us to upload a file. This could be used to upload a reverse shell for an application to execute.|0|
|Misconfigurations / Default Files|Search for common files that are sensitive (and shouldn't be accessible such as configuration files) on the web server.|2|
|Information Disclosure|Gather information about the web server or application (i.e. verison numbers, HTTP headers, or any information that may be useful to leverage in our attack later)|3|
|Injection|Search for possible locations in which we can perform some kind of injection attack such as XSS or HTML|4|
|Command Execution|Search for anything that permits us to execute OS commands (such as to spawn a shell)|8|
|SQL Injection|Look for applications that have URL parameters that are vulnerable to SQL Injection|9|

  
**Saving Your Findings**

Rather than working with the output on the terminal, we can instead, just dump it directly into a file for further analysis - making our lives much easier!

Nikto is capable of putting to a few file formats including:

- Text File
- HTML report

We can use the `-o` argument (short for `-Output`) and provide both a filename and compatible extension. We _can_ specify the format (`-f`) specifically, but Nikto is smart enough to use the extension we provide in the `-o` argument to adjust the output accordingly.

For example, let's scan a web server and output this to "_report.html_": `nikto -h http://ip_address -o report.html`