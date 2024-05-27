## Endpoint analysis
refers to the process of thoroughly examining and assessing the endpoints (i.e., URLs or URIs) exposed by an API to understand their functionality, security mechanisms, potential vulnerabilities, and attack surface. This analysis is crucial for identifying weaknesses that could be exploited by malicious actors.

### Reverse Engineering an API
Reverse engineering an API involves the process of understanding and dissecting how an API works, even when its internal workings or documentation are not readily available. This can be necessary for various reasons, such as developing compatible client applications, understanding undocumented APIs, or analyzing security aspects.

The process of reverse engineering an API typically involves techniques such as intercepting and analyzing network traffic, inspecting API endpoints and parameters, examining client applications that interact with the API, and experimenting with different inputs to observe the corresponding outputs and behaviors. Tools such as network sniffers, proxy servers, decompilers, and debugging tools are commonly used during this process.

#### Automatic Documentation

First, we will begin by proxying all web application traffic using ==mitmweb==.

Simply use a terminal and run:  
$ `mitmweb`

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/6KuYXxA4RqKE8PzXIvPn_mitmproxy.png)

This will create a proxy listener using port 8080. You can then open a browser and use *FoxyProxy* to proxy your browser to port 8080 using our Burp Suite option.
Once the proxy is set up, you can once again use the target web application as it was intended.

Every request that is created from your actions will be captured by the *mitmweb proxy*. You can see the captured traffic by using a browser to visit the mitmweb web server located at http://127.0.0.1:8081.

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/cfW71QnfSeCLslFvzQgA_mitmproxybrowser.png)

Continue to explore the target web application until there is nothing left to do. Once you have exhausted what can be done with your target web app, return to the mitmweb web server and click **File > Save** to save the captured requests.

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/eEp9PvDRHedaSZsYqI8g_mitmproxySave.png)

Selecting **Save** will create a file called flows. We can use the "flows" file to create our own API documentation. Using a great tool called ==mitmproxy2swagger==, we will be able to transform our captured traffic into an Open API 3.0 YAML file that can be viewed in a browser and imported as a collection into *Postman*.

First, run the following:
`$sudo mitmproxy2swagger -i [saved-file]-o ["filename".yml] -p [prefix-target] -f flow`

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/4TryEayCTsCoLdiBTsZS_mitmproxy2swaggerStep1.png)

After running this you will need to edit the spec.yml file to see if mitmproxy2swagger has ignored too many endpoints. Checking out spec.yml reveals that several endpoints were ignored and the title of the file can be updated. You can use Nano or another tool like Sublime to edit the spec.yml.


Update the YAML file so that "ignore:" is removed from the endpoints that you want to include.
![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/site/2147573912/products/037086f-8eaa-10c-d0a1-df108333e75_Sublime2.PNG)

Make sure to **only remove "ignore:"**. Removing spacing or the "-" can result in the script failing to work. 

 Once your docs edited, make sure to run the script once more. *This second run will correct the format and spacing*. This time around you can add the "--examples" flag to enhance your API documentation.

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/y7ouQ9QSQoS0LHJr9yuT_mitmproxy2swaggerStep3.png)

After running mitmproxy2swagger successfully a second time through, your reverse-engineered documentation should be ready. You can validate the documentation by visiting [https://editor.swagger.io/](https://editor.swagger.io/) and by importing your spec file into the Swagger Editor. Use **File>Import file** and select your spec.yml file. If everything has gone as planned then you should see something like the image below.

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/site/2147573912/products/3bNj0h5nQPyQELqabkNv_crapiSwagger1.png)

 
 This is a pretty good indication of success, but to be sure we can also import this file as a *Postman Collection* that way we can prepare to attack the target API.

- To import this file as a collection we will need to open Postman. At the top left of your Postman Workspace, you can click the "Import" button. Next, select the spec.yml file and import the collection.

- Once you import the file you should see a relatively straightforward API collection that can be used to analyze the target API and exploit with future attacks.

With a collection prepared, you should now be ready to use the target API as it was designed. This will enable you to see the various endpoints and understand what is required to make successful requests.


## Scanning API's
Scanning an API involves using various tools and techniques to identify vulnerabilities, misconfigurations, and security weaknesses that could be exploited by attackers. Conducting API scans is essential for ensuring the security and robustness of APIs before they are deployed into production environments.

##### Finding Security Misconfigurations
Security misconfiguration includes but not limited to missing system patches, unnecessary features enabled, lack of secure transit encryption, weak security headers, verbose error messages, and Cross-Origin Resource Sharing (CORS) policy misconfigurations.

`When vulnerability scans are applied generically to web APIs the most common outcome is to receive false-negative results. False-negative results take place when vulnerability scans do not detect or report existing problems.`

As an API penetration tester, using the right tools can greatly enhance your ability to identify security misconfigurations and vulnerabilities in APIs. Here are some most widely used  tools used for API security testing, including detection of misconfigurations:

1. **OWASP ZAP (Zed Attack Proxy)**:
    
    - OWASP ZAP is a popular open-source web application security scanner that can be used for API security testing.
    - It supports automated scanning for common security issues including misconfigurations such as insecure headers, CORS misconfigurations, and more.
    - ZAP can *intercept* and *modify* API requests/responses, making it useful for testing various aspects of API security.
2. **Burp Suite**:
    
    - Burp Suite is a powerful toolkit for web application security testing and is widely used by penetration testers.
    - It includes features like scanning, crawling, and interception of HTTP/S traffic, which can be leveraged for API security testing.
    - Burp Suite can detect misconfigurations related to authentication mechanisms, session management, and other security controls.
3. **Postman**:
    
    - Postman is a versatile API testing tool that can be used for both *functional testing* and *security testing*.
    - It allows testers to send custom HTTP requests, including those with payloads designed to test for misconfigurations like injection vulnerabilities.
    - Postman can be integrated with security testing libraries and scripts to automate security tests against APIs.

