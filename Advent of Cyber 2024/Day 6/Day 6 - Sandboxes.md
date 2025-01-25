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
	YARA searches for patterns to identify and classify malware. Analysts can create custom rules for YARA to search for and process such as particular strings, headers, etc.
![[Pasted image 20250125170757.png]]
	Mayor Malware's wrote a script to test the effectiveness of YARA's detection rules pictured above. 

	