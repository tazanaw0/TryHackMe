Learning Objectives:
- Grasp the fundamentals of writing shellcode
- Generate shellcode for reverse shells
- Executing shellcode with PowerShell
Key Terms
	Shellcode: A piece of code usually used by malicious actors during exploits like buffer overflow attacks to inject commands into a vulnerable system. 
	PowerShell: attackers often use PowerShell as a post-exploitation tool because of its deep access to system resources and ability to run scripts directly in memory, avoiding disk-based detection mechanisms.
	Windows Defender: A built-in security feature that detects and prevents malicious scripts, including PowerShell-based attacks, by scanning code at runtime.
	Windows API: Allows programs to interact with the underlying operating system, giving them access to essential system-level functions such as memory management, file operations, and networking. Serves as bridge between the application and the OS, enabling efficient resource handling. 
	Accessing Windows API through PowerShell Reflection: Windows API via PowerShell Reflection is an advanced technique that enables dynamic interaction with the Windows API from PowerShell. Instead of relying on precompiled binaries, PowerShell Reflection allows attackers to call Windows API functions directly at runtime. 
	Reverse Shell: A type of connection in which the target initiates a connection back to your attacking machine.

Generating Shellcode w/ msfvenom
	![[Pasted image 20250202144056.png]]
	Msfvenom creates the payload based on the flags we've specified.
	-p windows/x64/shell_reverse_tcp tells msfvenmo what type of payload to create. In this case, we want msfvenom to create the payload(-p) for reverse shell on a Windows machine(windowx/../..tcp)
	LHOST requires the ip address of the machine you want to connect back to. 
	LPORT is the port number on the machine that will listen for the reverse shell connection. Port number MUST match with what your listener is set to. 
	-f powershell (-f) specifies the format. We want the payload to be generated in PowerShell. 