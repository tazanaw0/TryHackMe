Learning Objectives
- Learn about WebSockets and their vulnerabilities.
- Learn how WebSocket Message Manipulation can be done
Purpose of WebSockets
	Maintaining an open line of communication 
		live chats/streams
		multiplayer gaming
		+ any other data where we require constant updates
Traditional HTTP request/response vs WebSocket
	HTTP required clients to make a request every time they'd access data. 
	Once the server responds, the connection is closed and it's an ongoing cycle of request/response and closing the communication channel. 
	With websockets we can 'keep the 'door open,' so both parties can continue to update or communicate with each other. 
WebSocket vulnerabilities
	Just as if we were to leave our home door open, there's an inherent risk associated with leaving a communication channel open for an extended period of time.
		Weak Authentication and Authorization 
			Solution: SSL/TLS
		Message Tampering
			Solution: SSL/TLS, ensure each packet has the correct sequence number and MAC. 
		Cross-Site WebSocket Hijacking(CSWSH)
			User's browser is tricked into opening a WebSocket connection to another site. 
		Denial of Service(DoS)
			Solution: Rate-limiting
Practical 
	Stopped here, website wouldn't load, so I couldn't move forward with analyzing requests/responses in burp suite. 