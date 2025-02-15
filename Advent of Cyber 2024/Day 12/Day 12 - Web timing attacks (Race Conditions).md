Learning objectives 
- Understand the concept of race condition vulnerabilities
- Identify the gaps introduced by HTTP2
- Exploit race conditions in a controlled environment
- Learn how to fix the race
Typical Timing Attacks
	In their simplest form, attackers can extract info from web apps by reviewing how long it takes to process their request. 
		Attackers modify requests (contents or method of deliver) and by observing the response time they can access unauthorized info. 
	Race conditions 
		Force app to perform unintended processes on the attackers behalf. 
	HTTP/2
		Faster and several features that eliminate limitations from HTTP/1.1. 
			Single-packet multi-request
				Stack multiple requests into the same TCP packet.
					 time differences can be attributed to different processing times for the requests
Two main categories 
	Information Disclosures
		Leverage difference in response delays to uncover sensitive info. 
	Race Conditions - Time-of-Check to Time-of-Use (TOCTOU)
		Forcing an application to perform unintended actions. e.g., sending the same coupon (requests) several times might allow you to apply it more than once. 
		(TOCTOU)
		- Time between when servers last checked their system  and when you used the results. (e.g., two user w/ access to one bank account, sending money to the same account)
Practical 
	Log into a banking application + use Burp Suite to intercept requests
		Burp suite
		- Settings -> Allow browser to run without sandbox option
			![[Pasted image 20250213161117.png]]
		Start browser w/ Burp Suite's proxy 
			Proxy -> Intercept -> open browser 
				![[Pasted image 20250213161407.png]]
	Application Scanning
		One key step in identifying race conditions is to ==validate functions involving multiple transactions or operations that interact with shared resources==, such as transferring funds between accounts, reading and writing to a database, updating balances inconsistently, etc.
	- Transferring funds, so we can inspect our request and understand how the server processes our request. 
		![[Pasted image 20250213162349.png]]
		![[Pasted image 20250213162611.png]]
Initiating the race condition attack by resending our transfer (post) request 
	![[Pasted image 20250213162654.png]]
	![[Pasted image 20250213162907.png]]
	We have the option to change any value within the request prior to querying the server w/ the same request. Also, duplicate the requests w/ ctrl + r so we can send multiple requests simultaneously. 
	![[Pasted image 20250213163612.png]]
	I added each duplicate request together into a single group dubbed 'funds.'
	![[Pasted image 20250213163741.png]]
		After sending 10 requests, I checked 110's account and their balance reflects the exploitation of Wareville's race condition vulnerability. 
	- In a secure banking system, the app should process the first request, lock the database and process the remaining request individually. 
Practical 
	Preventative measures
		- **Use Atomic Transactions**: The developer should have implemented atomic database transactions to ==ensure that all steps of a fund transfer== (deducting and crediting balances) are performed as a single unit. This would ensure that  ==either all steps of the transaction succeed or none do==, preventing partial updates that could lead to an inconsistent state.
		- **Implement Mutex Locks**: By using Mutex Locks, the developer could have ensured that ==only one thread accesses the shared resource==(such as the account balance) at a time. This would ==prevent multiple requests from interfering with each other during concurrent transactions.==
		- **Apply Rate Limits**: The developer should have implemented rate limiting on critical functions like funds transfers and withdrawals. This would limit the number of requests processed within a specific time frame, reducing the risk of abuse through rapid, repeated requests.
	