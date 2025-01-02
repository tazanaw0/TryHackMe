**Installing openvpn w/ homebrew**
brew install openvpn 
	add brew binary to path in shell configuration file (.zshrc or bashrc)
		open shell config file (sudo nano .zshrc)
		add the following line 
			export PATH="//usr/local/Cellar/openvpn/2.6.12/sbin:$PATH"
		reload shell config 
			source ~/.zshrc
		test command 
			sudo openvpn pathTo.confi
- [x] confirm connection in browser by entering 10.10.10.10 or entering curl 10.10.10.10/whoami in terminal 
**Installing Kali**
Downloaded iso from kali site 
Ran through gui
	create account 
	partition drive
	booted into grub to find username
**Installing openvpn w/ apt**
sudo apt update && sudo apt upgrade 
sudo apt install openvpn 
sudo openvpn <pathtoVPNprofile.configfile>
	![[Screenshot 2025-01-02 at 2.18.27 PM 1.png]] Success. 
download freerdp
sudo apt freerdp
accessing machine via rdp from terminal 
	 xfreerdp /u:(username) /p:(password) /v:(Machine IP) /dynamic-resolution to access thm's virtual machine 
