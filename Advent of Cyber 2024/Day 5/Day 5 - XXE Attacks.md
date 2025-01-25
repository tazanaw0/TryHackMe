Learning Objectives
- Understand the basic concepts related to XML
- Explore XML External Entity (XXE) and its components
- Learn how to exploit the vulnerability
- Understand remediation measures
Extensible Markup Language (XML)
	Format used for exchanging information between computers. 
	Tags label and organize information (easily create your own tags!) 
		e.g., 
			(address> phone>) - Unable to write closing tags here because they'll disappear in the note. 
	Document Type Definition (DTD)
		Rules that defines the structure of XML documents.
			Guides user with proper structure. 
		e.g., an XML document about people
	<!DOCTYPE people [
	   <!ELEMENT people(name, address, email, phone)>
	   <!ELEMENT name (#PCDATA)>
	   <!ELEMENT address (#PCDATA)>
	   <!ELEMENT email (#PCDATA)>
	   <!ELEMENT phone (#PCDATA)>
	]> 
		'Element' declares the tags or elements allowed in the file. 
		'PCDATA' = parsed plain text 
	Entities 
		placeholders that allow the insertion of large chunks of data or referencing internal or external files.
		e.g., 'ext' for external source
		<!DOCTYPE people [ 
			<!ENTITY ext SYSTEM "http://tryhackme.com/robots.txt"> 
		]>

XXE Vulnerability 
	an attack that takes advantage of how XML parsers handle external entities
	If there aren't proper security controls in place, an attacker can point external entities to any malicious source. 
	e.g, 
		<!DOCTYPE people[ 
			<!ENTITY thmFile SYSTEM "file:///etc/passwd">
		 ]>
		Once the XML is processed, the parser will try to load the passwd file. 

Practical 
	Analyze an application that allows users to view and add products to their carts and perform the checkout activity. 

Flow of the application 
	Pentesters typically start testing by analyzing the flow of an application. i.e., 
	understand how end users interact with the app. 
	For this particular website users can select products, add to cart, and add to a wishlist. Once users add to wishlist, they receive a confirmation message indicating that the data is saved with a unique id. 

Intercepting the Request
	Burp Suite 
		Route all web traffic from browser through Burp Suite so we can analyze and modify requests. 
	Settings 
		 Selected 'Allow to run without a sandbox' 
	Proxy Tab -> Open Browser
	Proxy Tab -> HTTP History 
		view all requests intercepted by Burp Suite. 
	![[Pasted image 20250123160633.png]]

Wishlist.php accepts the clients request and parses it with the following: 
	![[Pasted image 20250124141719.png]]
		`libxml_disable_entity_loader(false)` 
		allows the XML parser to load external entities (i.e., the XML input can include external file references or requests to remote servers)
		When the XML is processed by `simplexml_load_string` with the `LIBXML_NOENT` option, the web app resolves external entities, allowing attackers access to sensitive files or allowing them to make unintended requests from the server.
Preparing the Payload:
Here's what the php file would like if we were to reference external entities
	![[Pasted image 20250124141641.png]]
	<**!ENTITY payload SYSTEM "/etc/hosts">** tells the XML parser to replace the '**&payload;**' reference with the contents of the file **/etc/hosts** on the server. So once a user adds a product to their wishlist, the app will try to load contents from the file specified in the entity /etc/hosts/ path 
		'&payload' is how we reference the variable or entity.
Exploitation - XXE Attack 
	Now, we'll repeat the requests captured earlier using Burp Suite's repeater feature. 
		With repeater we can send multiple HTTP requests to a single or multiple web server/s.  
	![[Pasted image 20250124143435.png]]
		We can also use ctrl + r to send any request to the repeater. 
	Navigate to the repeater tab in the nav bar. 
		![[Pasted image 20250124143844.png]]
		Before sending multiple POST requests, I modified the XML and added a new payload. 
	Click the orange 'Send' button, to initiate the requests and execute our payload. 
		![[Pasted image 20250124144206.png]]
		As shown above, the web sever processed the payload within our requests and responded with data from the /etc/hosts/ file 
			The product ID: 127.0.0.1 localhost
			The following lines are desirable for IPv6 capable hosts
			::1 ip6-localhost ip6-loopback
			fe00::0 ip6-localnet
			ff00::0 ip6-mcastprefix
			ff02::1 ip6-allnodes
			ff02::2 ip6-allrouters
			ff02::3 ip6-allhosts
			is invalid.
			/etc/hosts/ is a plain text file that maps hosts names to ip address for a local host and other hosts on a network.
				+ we can think of it as a local dns resolver that allows our local devices to comm with each other using their host names. 

Time for Some Action
	Earlier, we discovered a page that's only accessible by admins 
		'/wishes/wish_1.txt'
	 Using this path, we just need to guess the potential absolute path of the file. Typically, web applications are hosted on `/var/www/html`. 
	 Now, let's build our new payload to read the wishes while leveraging the vulnerability.
	 ![[Pasted image 20250124145406.png]]
		We modified the payload with a new path to the wishlist items. 
	![[Pasted image 20250124145615.png]]
		To our surprise, the web server processed our XXE payload and displayed contents from the wishlist text file.
	![[Pasted image 20250124150041.png]]
		Here we demonstrate how you can continue the attack for each wishlist entry stored in their database by simply changing the number in the path. 
This vulnerability is a direct result of the developers disregard for securely handling inbound requests. 
Devs addressed the vulnerability as soon as they discovered it but here's the typical mitigation techniques:
	disabling external entity loading in their XML parser 
		`libxml_disable_entity_loader(true)` prevents XML from parsing external entities 
	+ implementing controls to validate and sanitize user input. 
		i.e., Inspect request and only process data that the web server is expecting. e.g., Deny requests w/ XXE's pointing to sensitive directories. 



![[Pasted image 20250124151724.png]]

![[Pasted image 20250124151812.png]]
Concluding thoughts
	Overall, I really enjoyed working through this room. Initially it seemed a bit overwhelming, but I'm glad I persisted with my efforts. 
	Burp Suite is a very interesting tool that can be used to inspect/modify traffic, carry out attacks and much more. XXE vulnerabilities arise when web servers fail to validate and sanitize user input. 