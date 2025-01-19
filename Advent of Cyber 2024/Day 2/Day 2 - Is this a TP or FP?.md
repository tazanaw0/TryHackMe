True Positives or False Positives (TPs and FPs)
SIEM's aggregate logs and info from various devices to one central console. 
Rules are defined to identify malicious or suspicious activity from events. 
Alerts triggered if event matches rule/s.

SOC analysts can contact the user associated with the event to gather more information about the alert. 
	 + If any changes might trigger an alert, the user may be required to create a change request and have it approved through their orgs IT change management process. 
		 analysts can ask users for change request details - verify if their request was approved
	Contacting the user may not be as effective for the following:
		- Organization may not have a change request process in place.
		- The performed activity is outside the scope of the change request or is different from what's been approved.
		- A user performed a malicious activity via social engineering from a threat actor.
Thus, we must analyze the context of the event and make correlations between the past and present. e.g., look at the user's past behaviors, occurrence of the event throughout the org, etc. 
	Create a hypothesis and find events to support 
		e.g, 
		Hypothesis: the user downloaded malware from a spoofed domain. 
		Event: The website used a spoofed domain name, and a certain file was downloaded from that website. User activity found in proxy logs.
Identify whether the malware executed through some vulnerability in an application or a user intentionally executed the malware.
	Look at parent process of the malware and the command line parameters used to execute the said malware. 
		If the parent process is File Explorer, we can assume the user executed the malware intentionally (or they might have been tricked into executing it via social engineering) 
		but if the parent process is a web browser or a word processor, we can assume that the malware was not intentionally executed, but was executed because of an unknown vulnerability. 

Working w/ Elastic's SIEM
![[Pasted image 20241202191725.png]]
	Set the timeframe under 'absolute,' so it narrows the results to the exact timeframe
![[Pasted image 20241202192618.png]]
Add filters to only include the fields related to the investigation. 
-  `host.hostname` outputs the hostname 
- `user.name` displays users who performed the activity. 
-  `event.category` to ensure we are looking at the correct event category.
- `process.command_line` displays cmds ran by PowerShell
- `event.outcome` tells us if the activity succeeded. 

12/02 Taking a break, will resume later :) 
	504 error when trying to access Elastic SIEM
01/18
	Returning to complete this now that I have some time!!

To begin, I signed into Elastic, navigated to the discovery page, specified the timeframe of the event and selected the appropriate filters. 
	After taking a look at the logs, I notice there's a generic admin account responsible for a majority of the events across different hosts. Also, there's a successful authentication prior to each execute of a PowerShell command. 
Next, I selected the source.ip filter to identify where the events originated from. 
Then, I changed the start date of the timeframe to gather more information about the user's activity. 
	After changing the start date to 3 days earlier, I noticed there's 6,000 + more events which is very concerning and a large portion of the logs are authentication events. 
To reduce the search, we filtered (select + sign next to value) the username and source.ip field with the admin account and 10.0.11.11 ip address. 
	The results?
	All the events are coming from the same user and source IP. 
I filtered out the IP responsible for each successful event to see if I can identify which hosts are responsible for the traffic spike. 
	10.0.255.1 has several failed authentication attempts from the same generic admin account 
While looking at the logs I notice after the 10.0.11.11 address was successfully authenticated, the authentication attempts from the 10.0.255.1 stopped completely. 
	This suggests someone successfully ran a brute-force attack because as soon as they were authenticated on a different host they stopped their login attempts. Additionally, immediately after they signed in, they ran PowerShell commands. 
	So, this is a true positive!
Next, I changed the filters with the event.category: process, so I could inspect the PowerShell commands. 
	The commands are encoded. 
	Tip: PowerShell commands generally Base64 Encoded. 
		+ Since the command may have sensitive information, it's best to use tools hosted on your local machine. 
The results? 
	Someone came in to help the SOC!
	A script used to updated many windows machines had outdated credentials and someone brute-forced the systems to fix the credentials. 

![[Pasted image 20250118165058.png]]
![[Pasted image 20250118165147.png]]
	



