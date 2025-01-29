The Peculiar Case of Care4Wares’ Dry Funds
	A nonprofit organization hosted a fundraiser to raise money for gifts.
	 They promoted the event with a digital flyer hosted via S3. 
	 After the second day of sharing the flyer, they realized no more donations were received. 
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
	command line for parsing, filtering and transforming JSON data. 
	Useful for investigating large sets of data. 
	`.`  tells JQ we are accessing the current input. 
	 access the array of values stored in our JSON (with the `[]`). 
	So, our filter is =  `.[]`.
![[Pasted image 20250129163800.png]]
	I created a JSON file, used JQ to output the contents, and lastly, wrote a command that only displays book titles. If we want output for a specific value, we must call for it using its key name. 
		e.g., JQ command that only outputs 'Genre'
			jq '.[] | .json' bookstore.json

	