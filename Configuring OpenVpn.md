use curl to install homebrew
brew install openvpn 
download configuration pack from thm for nearest vpn server 
connect to THM over openvpn 
	sudo openvpn ~/Downloads/username.ovpn (path to config file)
		username = THM username
confirm connection in browser by entering 10.10.10.10 or entering curl 10.10.10.10/whoami in terminal 

