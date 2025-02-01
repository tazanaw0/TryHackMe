The Peculiar Case of Care4Wares’ Dry Funds
	A nonprofit organization hosted a fundraiser to raise money for gifts.
	 They promoted the event with a digital flyer hosted via S3. 
	 After the second day of sharing the flyer, they realized a drop in donations. 
	Donor shows proof of transaction made 3 days after link was sent and the account number on the flyer is incorrect. 
	The link hasn't been altered which suggests someone may have accessed the s3 bucket and modified the flyer. 

CloudWatch
	Monitoring of system and application metrics + configuration of alarms for each metric. 
	Aggregated logs (e.g., Route 53, CloudTrail)
	An agent must be installed on an EC2 instance for the metrics to be captured. 

Log Events
	Records of individual application events
Log streams 
	Collections of events from a source (e.g., log stream from s3 bucket)
Log groups
	Collection of log streams (e.g., log streams from IAM hosted on many servers)

CloudTrail
	Monitor actions by users with JSON Events. 
	Event history 
		By default, AWS monitors past 90 days of activity. (+set your own custom trail)
	Trails
		Time period for events captured and stored. 
	Similar to CloudWatch, CloudTrail can be used as single AP for aggregaetd logs and can forward logs to CloudWatch. 
JQ 
	command line for parsing, filtering and transforming JSON data. (functionally similar to grep)
	Useful for investigating large sets of data. 
	`.`  tells JQ we are accessing the current input. 
	 access the array of values stored in our JSON (with the `[]`). 
	So, our filter is =  `.[]`.
![[Pasted image 20250129163800.png]]
	I created a JSON file, used JQ to output the contents, and lastly, wrote a command that only displays book titles. If we want output for a specific value, we must call for it using its key name. 
		e.g., JQ command that only outputs 'Genre'
			jq '.[] | .genre' bookstore.json

Completing the investigation
	![[Pasted image 20250130143352.png]]
	Explains elements listed in the typical CloudTrail logs. 
	![[Pasted image 20250130144028.png]]
	Explains jq command used to filter logs. 
![[Pasted image 20250130144517.png]]
	User 'Glitch' initiated a session around 3PM on the 28th of November. They ran "ListObjects" function to list every object stored in S3 from a Linux machine. Lastly, we see s3 logged the ip address associated with the event as 53.94.201.69. 
![[Pasted image 20250130145255.png]]
	In addition to running 'ListObjects,' t ran 'PutObject' indicating they successfully uploaded a file to wareville's s3 bucket. There's information regarding the device and browser used to execute this function + the contents of which objects were 'Put' or uploaded into a bucket dubbed 'wareville-care4wares.'
![[Pasted image 20250131132448.png]]
![[Pasted image 20250131132504.png]]
	Here we've filtered the logs to only display 6 fields. 
	![[Screenshot 2025-01-31 at 1.31.46 PM.png]]
	![[Pasted image 20250131132910.png]]
	Based on our findings, it's very likely glitch is responsible for warevilles missing donations. McSkidy doesn't recall createing 'glitch,' so we'll have to investigate further and review the account's activity. 
	![[Pasted image 20250131133930.png]]
	Filtering out any information unrelated to Glitch. 
	'ConsoleLogin' event informs us that Glitch accessed the AWS Management Console. 
	![[Pasted image 20250201125949.png]]
	Included the user_agent field, which will give us information about the OS and browser behind each event. 
	Glitch access the AWS management console via Google Chrome from an Apple device. 
	The other value beginning with 'S3Console/0.4....' is the userAgent string for the internal console used in AWS. 
	
Looking into who created the account
	Filtering for IAM related events
- What Event Names are included in the log entries?
- What user executed these events?
- What is this user’s IP?
![[Pasted image 20250201131022.png]]
	First, we notice the source_ip for mcskidy is the exact same as glitch. Additionally, we notice mcskidy invoked the CreateUser action and AttatchUserPolicy action. 
![[Pasted image 20250201131500.png]]
	Filtering the query to show information regarding the CreateUser event. 
	We've confirmed McSkidy created the glitch account on the 22nd of October at 3:21 PM. 
What permissions does the glitch have?
	We'll run the same command but change the eventName field to 'AttachUserPolicy.' AttachUserPolicy applies access policies to users, defining the extent of access to the account.
	![[Pasted image 20250201132053.png]]
	Wow. McSkidy granted admin access as shown in the 'policyArn' field. It can't be McSkidy because uses a Windows machine and all of her previous activity originated from a different source IP address. Someone must have gained access to her account. 
![[Pasted image 20250201132452.png]]
	Mayor malware accessed McSkidy's s3 bucket from the same IP as the glitch on several occasions. Hmm. 
	Lets view each user's activity before the attack and see who truly works from that IP address. 
![[Pasted image 20250201133459.png]]
	Prior to the attack, McSkidy's login activity came from a different IP, 31.210.15.79. We've also verified McSkidy works with Windows OS. So, this leaves mayor_malware as a possible suspect for carrying out the phishing attack. 
Breakdown of events
	Unusual login with McSkidy's account originating from 53.94.201.69 
	53.94.201.69 creates glitch account shortly after, then grants admin access. 
	53.94.201.69 (glitch) accessed warevilles bucket and replaced the donation flyer with a new one. 
	The IP and user_agent used to access every account was the same throughout the event. 
	++ Confirmed McSkidy's activity outside of this timeframe are from a different agent and ip address. 

Definite Evidence
	McSKidy reached out to the bank and they were able to provide us with their RDS logs stored from CloudWatch. 
	![[Pasted image 20250201135108.png]]
	We can see the exact moment when mayor malware updated the flyer and care4wares stopped receiving donations. Additionally, we see that all donations intended for McSkidy's fundraiser are sent to Mayor Malware's bank account. 

![[Pasted image 20250201141054.png]]
We've gained valuable experience working with JQ, AWS CloudTrail and CloudWatch. JQ enables efficient log analysis and we were able to create a timeline of events by narrowing down our queries to only include whatever's pertinent to the investigation. 