The following lab involves 

Objectives of defensive security: 
1. Preventing intrusions from occurring
2. Detecting intrusions when they occur and responding properly
Often accomplished by members of a blue team. 

Task 2, Areas of Defensive Security:
	- Security Operations Center (SOC), where THM covers Threat Intelligence:
	- Define SOC + main focus:
		A _Security Operations Center_ (SOC) is a team of cyber security professionals that ==monitors the network and its systems to detect malicious cyber security events==. Some of the main areas of interest for a SOC are:
		==Vulnerabilities: ==Whenever a system vulnerability (weakness) is discovered, it is essential to fix it by installing a proper update or patch. When a fix is unavailable, the necessary measures should be taken to prevent an attacker from exploiting it. Although remediating vulnerabilities is vital to a SOC, it is not necessarily assigned to them. 
		==Policy violations:== A security policy is a set of rules required to protect the network and systems. For example, it might be a policy violation if users upload confidential company data to an online storage service. 
		==Unauthorized activity:== Consider the case where a user’s login name and password are stolen, and the attacker uses them to log into the network. A SOC must detect and block such an event as soon as possible before further damage is done.
		==Network intrusions:== No matter how good your security is, there is always a chance for an intrusion. An intrusion can occur when a user clicks on a malicious link or when an attacker exploits a public server. Either way, when an intrusion occurs, we must detect it as soon as possible to prevent further damage.
	Define threat intelligence: KNOW YOUR ATTACKER, IDENTIFY TRAITS & CREATE SOLUTIONS
		Gathering information about an organizations adversaries (current and potential), which includes the methods and motives of gaining unauthorized access to enable a threat informed defense. 
			Adversaries may include hacktivists, organized crime members, script kiddies, shadow IT, nation-state, and insider threats. 
		Often accomplished by the use of network logs, forums, CVE lists, OSINT, etc. 
	Digital Forensics and Incident Response (DFIR)
		-Digital Forensics: 
			Analyzing evidence of an attack and those responsible. 
			Focus: 
				Images (digital copy) of file systems: allow us to identify what and why an incident occurred. (E.g. overwritten files, source of malware, inbound/outbound traffic to a c&c server)
				Images of system memory: Identify malware that was ran in memory using a system memory image. 
				System Logs: Analyze to understand what occurred, even if the attacker happens to modify the logs. 
				Network logs: Use protocols like netflow to identify source/destination of incident and any traffic that traversed the network. 
	Incident Response: 
		A well documented process for handling any incident that may arise within your organization. Overall, the goal is to detect (whether it be before, during or after), respond, and review (the affects & process) to improve for future attacks. 
		4 Major phases: 
			Preparation 
				Accomplished through well documented policies and training (test the plan!). 
				Everyone in the organization knows their role 
			Detection and Analysis 
				Many tools used to identify incidents (e.g. EDR/XDR's, NetFlow, Anti-malware, HIPS, DLP agents, etc.)
				Analyze logs and reports - Understand what and why 
			Containment, Eradication & Recovery
				Isolate (contain) the infected system - typically placed into sandbox environment (container or vm)
				Remove (eradicate) it from the system 
				Restore (recovery) from a known good backup or (if necessary) reimage the device
			Post-Incident Activity (Meeting)
				Analyze reports once systems are operational. 
				Make any necessary changes to incident response plan. 
				Share with other orgs within your industry. 
	Malware Analysis
		Types 
			Virus
				Code that attaches itself to an existing program. 
				Alters and deletes files once executed.
				As a result, system can be slow or inoperable. 
			Trojan Horse 
				An application that masquerades as a legitimate program
				Malicious code executed once you run the program. 
			Ransomware 
				Malicious code that restricts user from accessing content until an attackers demand (ransom) is met. 
			Many more out there (spyware, adware, etc.)
	Security teams can analyze malware by using the following: 
			Static analysis 
				Running scans and tests on the application code without executing the program
				Identify vulnerabilities + any malicious code 
					E.g. Static application security test (SAST)
			Dynamic analysis 
				Running the malware in a controlled (sandboxed) environment to identify how it could affect your systems. 
				What changed? Are there any critical files missing? Any security configs altered?
					E.g. Fuzzing 
	Completing task 2
		![[Pasted image 20241109180053.png]]
				
Task 3: Practical Example of DefSec
	'Inspect the alerts in your SIEM dashboard. Find the malicious IP address from the alerts, make a note of it, and then click on the alert to proceed.'
	![[Pasted image 20241115130506.png]]
	Checking the IP address with a 3rd party. 
	![[Pasted image 20241115130552.png]]
	The scanner informs us that the IP is malicious 
	![[Pasted image 20241115130729.png]]
	I went back to the first step and realized there was a successful attempt with the malicious IP, so this requires us to escalate the event to the team lead. They'll be able to grant us just-in-time permissions to add the IP to our block list. 
	![[Pasted image 20241115131040.png]]
	![[Pasted image 20241115131532.png]]
	Flag confirming the ip was added to the orgs block list. 
	![[Pasted image 20241115131557.png]]