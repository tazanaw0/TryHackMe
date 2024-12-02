Objectives: 
- Learn how to investigate malicious link files.
- Learn about OPSEC and OPSEC mistakes.
- Understand how to track and attribute digital identities in cyber investigations.
Investing Youtube to MP3 converter website 
	Many converter sites are very risky and may contain: 
	- **Malvertising**: Many sites contain malicious ads that can exploit vulnerabilities in a user's system, which could lead to infection.
	- **Phishing scams**: Users can be tricked into providing personal or sensitive information via fake surveys or offers.
	- **Bundled malware**: Some converters may come with malware, tricking users into unknowingly running it.
![[Pasted image 20241201194151.png]]
	Navigated to site using IP
![[Pasted image 20241201194327.png]]
	Entered link for conversion, prompted with message assuring security. 
![[Pasted image 20241201194823.png]]
	Extracted the download and presented with two files when I'm only expecting a single file.. SUS
![[Pasted image 20241201195041.png]]
	ran file to analyze contents of each file
	somg.mp3 is a Microsoft shortcut (.lnk) file masked as an MP3 file. 
		Lnk files are executable and used to link another file, folder, or app. 


used ExifTool to inspect lnk file, which will reveal any embedded commands and attributes
	![[Pasted image 20241201195648.png]]
	-ep Bypass -nop flags disables PowerShell restrictions, allowing scripts to execute regardless of security settings or user profiles
	.DownloadFile method pulls a PowerShell script from fake github repository dubbed 'IS.ps1' and saves it into the ProgramData directory
	iex command executes the s.ps1 script 
![[Pasted image 20241201200407.png]]
	Inspecting the link, we see the attacker wrote a script to identify any crypto wallets stored on the machine + saved credentials and write the results to a 'stolen_info.txt' file that's auto forwarded to the attackers remote server. 
Searching the source 'Created by the one and only M.M'
![[Pasted image 20241201201407.png]]
	Searched for the text in the script and found a user who may have more info. 
After analyzing the attackers github discussion, I noticed they left a trail for us which included their username and their disposition throughout the discussion 

![[Pasted image 20241201202953.png]]
	Caught him SLIPPING lol.

![[Pasted image 20241201203426.png]]
	Successfully completed day one. 




	
