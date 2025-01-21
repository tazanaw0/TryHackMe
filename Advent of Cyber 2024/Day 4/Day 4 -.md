Learning Objectives
- Learn how to identify malicious techniques using the MITRE ATT&CK framework
	Adversarial Tactics, Techniques, and Common Knowledge (ATT&CK)
	a collection of tactics, techniques, and procedures (TTPs) used by threat actors. 
- Learn about how to use Atomic Red Team tests to conduct attack simulations.
	a collection of red team test cases that are mapped to the MITRE ATT&CK framework 
	used by blue teams to test for detection gaps 
- Understand how to create alerting and detection rules from the attack tests.
Cyber Kill Chain
	![The process flow of the Unified Kill chain.](https://tryhackme-images.s3.amazonaws.com/user-uploads/6093e17fa004d20049b6933e/room-content/ddd4bddb2db2285c3e42eef4c35b6211.png)
	Ideally we'd like to detect and block all threats in the earlier phases of the kill chain. However, if the attackers are skilled, they will inevitably adapt which is why organizations have to persist with their detection efforts at all phases of the cyber kill chain. 

The room requires us to initiate a Atomic Red attack, then search through event viewer to see if our system detected it. 
	T1566.001 Spearphishing 
		Simulate an attack where a user clicks a phishing link that downloads a macro enabled document
		Tool
			Sysmon: information on process creation, network connections, and changes to file creation time.
	Remove all files and logs in sysmon from previous executions of the attack. 
	Run the attack in PowerShell: 
		Invoke-AtomicTest T1566.001 -TestNumbers 1
	Refresh logs in event viewer and organize by chronological order. 
	Analyze (indicators of compromise) events captured in sysmon operational logs. 
		![[Pasted image 20250121140747.png]]
	Clear logs 
		Invoke-AtomicTest T1566.001-1 -cleanup
	Configure custom alerts to mitigate against future spearphishing attacks. 
		![[Screenshot 2025-01-21 at 2.14.05 PM.png]]
		"Invoke-WebRequest" "$url = /localhost/Phishing.xlsm" "Phishing.xlsm"
			Here's what a sigma rule would like if we used the indicators of compromise. 
			![[Pasted image 20250121142000.png]]
# Challenge
Identify the correct atomic test to run that will take advantage of **a command and scripting interpreter**, conduct the test, and extract valuable artefacts that would be used to craft a detection rule.
![[Pasted image 20250121144025.png]]


![[Pasted image 20250121143950.png]]
Additional Resources
	https://attack.mitre.org/techniques/T1059/003/
Today's my first time running a simulated ransomware attack and viewing a live attack is pretty cool. 