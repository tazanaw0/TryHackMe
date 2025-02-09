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
		WPS created so users connect to their wifi using an 8 digit pin as opposed to a complex password. Attack made by ==initiating a WPS handshake with the router and capturing the router's response== (contains info about pin + psk), and vulnerable to brute-force. 
WPA/WPA2 cracking
	Attackers start by sending de-auth packets to active users. Once they disconnect, they'll try and reconnect which will start a 4-way handshake with the router. 
	The 4-way handshake
		First, an attacker places their wireless adapter into monitor mode to scan for networks, then targets a specific network to capture the 4-way handshake. Once the handshake is captured, the attacker may run brute-force or dictionary attacks offline, using a tool like aircrack-ng to attempt to match a wordlist against the passphrase.
	- **Router sends a challenge:** The router (or access point) sends a challenge" to the client, asking it to prove it knows the network's password without directly sharing it.
	- **Client responds with encrypted information:** The client takes this challenge and uses the PSK to create an encrypted response that only the router can verify if it also has the correct PSK.
	- **Router verifies and sends confirmation:** If the router sees the client’s response matches what it expects, it knows the client has the right PSK. The router then sends its own confirmation back to the client.
	- **Final check and connection established:** The client verifies the router's response, and if everything matches, they finish setting up the secure connection.
11:57

The Vulnerability 
	Attackers can listen for incoming connections while users try to initiate wpa/wpa2 connections. 
The Practical
	run 'iw dev' to show available wireless devices and their configs
		![[Pasted image 20250207121036.png]]
	MAC/BSSID = Basic SSID for access point (AP)
	type 'managed' = Standard mode where device act as client, connecting to an AP. 
	run 'sudo iw dev wlan2 scan' where we specify the ap (dev wlan2) and scan tells iw to search the area for wifi networks
		![[Pasted image 20250207121307.png]]
	BSSID = 02:00:00:00:02:00
	SSID = MalwareM_AP - This suggets the ap is discoverable for local clients. 
	Robust Security Network(RSN, pat of WPA2 standard used to define encryption and auth settings)
		Group and Pairwise ciphers = Counter Mode with Cipher Block Chaining Message Authentication Code Protocol (CCMP) is the encryption method used. 
		 'Auth suite' lets us know a pre-shared key is required for access to MalwareM_AP. 
		DS Parameter Set = Channel 6, the channel refers to a specific frequency range. 
			most common Wi-Fi channels are 2.4 GHz and 5GHz. In the 2.4 GHz band, channels 1, 6, and 11 are commonly used because they don’t overlap, minimising interference. In the 5 GHz band, there are many more channels available, allowing more networks to coexist without interference.
	Monitor mode (Type)
		Primarily used for network analysis and auditing. 
		- The AP passively captures all traffic on a specific channel regardless of whether the traffic is intended for the AP. (Done w/ out joining a network)
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
		