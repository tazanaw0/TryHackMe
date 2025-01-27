Learning Objectives
- Analyze malware behavior using sandbox tools.
- Explore how to use YARA rules to detect malicious patterns.
- Learn about various malware evasion techniques.
- Implement an evasion technique to bypass YARA rule detection.
Detecting Sandboxes
	Attackers use sandboxes to analyze malicious code and based on their findings, make necessary revisions. 
	Additionally, malware developers often include a function to check if their target machine is running on a sandbox. If sandbox environment is detected, it won't execute. 
	E.g., They'll search for 'C:\Program Files' by querying HKLM\\Software\\Microsoft\\Windows\\CurrentVersion
		Program files is often unavailable in sandboxes. 
	![[Pasted image 20250125170406.png]]

Can YARA Do It?
	YARA uses patterns to identify and classify malware. Analysts can create custom YARA rules to search for and process headers, strings, etc.
![[Pasted image 20250125170757.png]]
	Pictured is a YARA rule that executes every time a user tries to access the registry. 
	They've declared a variable in 'strings' with a value the rule will search for. Condition defines values in which an alert will be triggered for. 

![[Pasted image 20250126144101.png]]
	We were tasked with executing "MerryChristmas.exe" and identifying any events involving a query to windows registry in our EDR logs. Shortly after executing the script, our EDR detected an event which matches the "SANDBOXDETECTED" rule and logged it to a text file dubbed 'YaraMatches.' 

Adding More Evasion Techniques
	Obfuscation helps attackers remain stealthy and lessens the likelihood of detection by encoding code with 
Obfuscated version of the registry check script (encoded in base64)
	![[Pasted image 20250127123734.png]]
	registryCheck checks the systems registry key for information about the program files dir. 
![[Pasted image 20250127124202.png]]
	char variable decoded in CyberChef. 

Beware of Floss
	Tools like 'Floss,' inspect malware binaries for obfuscation. 
	I ran the tool in PowerShell and redirected the output (results of floss scan) to a text file. 
	Command
		C:\Tools\FLOSS> floss.exe C:\Tools\Malware\MerryChristmas.exe |Out-file C:\tools\malstrings.txt
	Results 
		![[Pasted image 20250127134428.png]]
			FLOSS extracts all the following string types:
			1. static strings: "regular" ASCII and UTF-16LE strings
			2. stack strings: strings constructed on the stack at run-time
			3. tight strings: a special form of stack strings, decoded on the stack
			4. decoded strings: strings decoded in a function
		Based on the scan, there seems to be many matches for obfuscated code. MerryChristmas binary may not be as friendly or safe as the name suggests. 
	![[Pasted image 20250127140428.png]]
		I found the hidden flag by using the search function (ctrl + f). 

Using YARA Rules on Sysmon Logs
	YARA rules can be designed to search through sysmon logs. 
	![[Pasted image 20250127143057.png]]
	The EDR logged each detection with its event ID and record id derived from sysmon. 
Using the event record ID we can apply a custom filter to sysmon logs and search for any additional details left behind. 
![[Screenshot 2025-01-27 at 2.38.34 PM.png]]

![[Pasted image 20250127145038.png]]
	The event captured provides a good bit of information for our investigation.
	ParentImage shows us which parent process is responsible for triggering the event. 
	ParentProcessID & ProcessId helps us correlate single events with other processes.
	User tells us which user (privileges) triggered the event.
	CommandLine presents us with the users command line input. 
	UtcTime helps us create timeframes.

Concluding thoughts
	Another interesting and action packed room. I gained experience in static analysis testing, sysmon, working with an EDR and creating custom firewall rules via YARA. 

	