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
	We were tasked with executing "MerryChristmas.exe" and waiting for our EDR tool to identify any events involving a query to windows registry. Shortly after executing the script, our EDR detected an event which matches the "SANDBOXDETECTED" rule and logged it to a text file dubbed 'YaraMatches.' 
