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

Preparing the payload 
	