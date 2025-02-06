Learning Objectives
- Understand what Wi-Fi is
- Explore its importance for an organisation
- Learn the different Wi-Fi attacks
- Learn about the WPA/WPA2 cracking attack

Wi-Fi Attacks
	Evil Twin Attack:
		Typically begins by sending victims de-authentication packets, which disconnects them from their trusted access point(ap). Attacker creates fake ap with an ssid similar to the victims (typosquatting, e.g., FrostburgStaff-> Fr0stburgStaff) Ideally, after multiple attempts reconnecting to the trusted ap, the attacker expects the victim to try their fake ap, so they can listen to all of the victims traffic. 
	Rogue Access Point:
		Similar to Evil Twin, the attacker configures a rogue access point near their targets and waits for them to accidentally join, so they can intercept traffic. 
	Wi-Fi Protected Setup Attack (WPS):
		WPS created so users connect to their wifi using an 8 digit pin as opposed to a complex password. Attack made by ==initiating a WPS handshake with the router and capturing the router's response== (contains info about pin + psk), and vulnerable to brute-force. 
WPA/WPA2 cracking
	Attackers start by sending de-auth packets to active users. Once they disconnect, they'll try and reconnect which will start a 4-way handshake with the router. 
	The 4-way handshake
		First, an attacker places their wireless adapter into monitor mode to scan for networks, then targets a specific network to capture the 4-way handshake. Once the handshake is captured, the attacker runs a brute-force or dictionary attack using a tool like aircrack-ng to attempt to match a wordlist against the passphrase.