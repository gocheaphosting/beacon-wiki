Nmap Commands for System Admins and Network Admins 


Nmap [Network Mapper] is a free and open source utility for network discovery and security auditing. Many systems and network administrators also find it useful for tasks such as network inventory, managing service upgrade schedules, and monitoring host or service uptime. 

Here from this Post we can learn most of the useful command with option of nmap to scan the network and network ranges , single host , Os information and much more 


It will Show which machines are up in network 
It will show which machine has which Operating system
It will show which ports all opened 
We can Scan for a Range if networks 


How to install nmap Ubuntu/Debain systems

sudo apt-get install nmap 


This will install the nmap package ..



1. Scan a Single IP Address 

# scan a single IP Address

nmap 192.168.1.99


output 


root@ubuntu:~# nmap 192.168.1.99

Starting Nmap 5.21 ( http://nmap.org ) at 2013-10-26 11:12 IST
Nmap scan report for 192.168.1.99
Host is up (0.00057s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
MAC Address: 00:24:1D:B8:7B:8D (Giga-byte Technology Co.)

Nmap done: 1 IP address (1 host up) scanned in 0.24 seconds



# scan a single host 


nmap ubuntu.example.com


root@ubuntu:~# nmap ubuntu.example.com

Starting Nmap 5.21 ( http://nmap.org ) at 2013-10-26 11:13 IST
Nmap scan report for ubuntu.example.com (192.168.1.160)
Host is up (0.0000070s latency).
Not shown: 996 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
389/tcp open  ldap
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 0.12 seconds



# scan a hostname for more information about the host 


nmap -v ubuntu.example.com


Here is the Output


root@ubuntu:~# nmap -v ubuntu.example.com

Starting Nmap 5.21 ( http://nmap.org ) at 2013-10-26 11:14 IST
Initiating SYN Stealth Scan at 11:14
Scanning ubuntu.example.com (192.168.1.160) [1000 ports]
Discovered open port 80/tcp on 192.168.1.160
Discovered open port 443/tcp on 192.168.1.160
Discovered open port 22/tcp on 192.168.1.160
Discovered open port 389/tcp on 192.168.1.160
Completed SYN Stealth Scan at 11:14, 0.07s elapsed (1000 total ports)
Nmap scan report for ubuntu.example.com (192.168.1.160)
Host is up (0.000021s latency).
Not shown: 996 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
389/tcp open  ldap
443/tcp open  https

Read data files from: /usr/share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.16 seconds
           Raw packets sent: 1000 (44.000KB) | Rcvd: 2004 (84.176KB)


2. Scan Multiple IP Address

# Scanning multiple IPs

nmap 192.168.1.77 192.168.1.99

          or
nmap 192.168.1.77,99


Output is Here 


root@ubuntu:~# nmap 192.168.1.77 192.168.1.99

Starting Nmap 5.21 ( http://nmap.org ) at 2013-10-26 11:15 IST
Nmap scan report for 192.168.1.77
Host is up (0.00086s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
443/tcp  open  https
3000/tcp open  ppp
5432/tcp open  postgresql
MAC Address: 00:24:7E:00:ED:79 (USI)

Nmap scan report for 192.168.1.99
Host is up (0.00073s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
MAC Address: 00:24:1D:B8:7B:8D (Giga-byte Technology Co.)

Nmap done: 2 IP addresses (2 hosts up) scanned in 6.16 seconds



# Scanning Range of IP using Wildcard (*)

nmap 192.168.1.*


Output is too long so avoided it here 


# Scanning Entire Subnet 255.255.255.0


nmap 192.168.1.0/24


# Scanning for range of IPs


nmap 192.168.1.33-99


Here is the Output


3. To Find the OS & Version of remote Hosts Using Nmap


nmap -A 192.168.1.77


This will output more information about a Host and its Ports and Operating systems and Version 


nmap -v -A 192.168.1.77


Show more information with Vebrosity


4. To scan a Particular Ports 

Syntax : 

nmap -o port_number hostname(or)IPaddress

# scanning for particular Port number 

nmap -p 22 192.168.1.99



Output is here 


root@ubuntu:~# nmap -p 22 192.168.1.99

Starting Nmap 5.21 ( http://nmap.org ) at 2013-10-26 11:18 IST
Nmap scan report for 192.168.1.99
Host is up (0.00056s latency).
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: 00:24:1D:B8:7B:8D (Giga-byte Technology Co.)

Nmap done: 1 IP address (1 host up) scanned in 0.16 seconds



# scanning for TCP ports 443

nmap -p T:443 192.168.1.99


# scanning for UDP ports 82

nmap -p U:82 192.168.1.99


# scanning multiple ports 

nmap -p 443,82 192.168.1.99


#scanning for all ports using wildcard(*)

nmap -p "*" 192.168.1.99


5. To Watch the all packets send and reciving 


nmap --packet-trace 192.168.1.77


6. Know Whether a Host is Protected by Firewall or not :


# To scan firewall protcted for a host

nmap -PN 192.168.1.99


# To scan firewall protected for a Network

nmap -sA 192.168.1.77


7. To know the interface and Route 

nmap --iflist


Output is here 

root@ubuntu:~# nmap --iflist

Starting Nmap 5.21 ( http://nmap.org ) at 2013-10-26 11:19 IST
************************INTERFACES************************
DEV  (SHORT) IP/MASK          TYPE     UP MAC
lo   (lo)    127.0.0.1/8      loopback up
eth0 (eth0)  192.168.1.160/24 ethernet up 00:15:5D:01:11:39

**************************ROUTES**************************
DST/MASK      DEV  GATEWAY
192.168.1.0/0 eth0
0.0.0.0/0     eth0 192.168.1.1


8. scan a List of hosts and networks from a text file 


cat > /home/sysadmin/ips.txt


File contain IPs and Hosts are here 

192.168.1.77
192.168.1.99
192.168.1.0/24
ubuntu.example.com


To scan the network using created file use command 


nmap -iL /home/sysadmin/ips.txt


9. While scanning a Bunch of hosts , If we need to Drop any one of the host or multiple hosts

# Excluding Single host 

nmap 192.168.1.10-100 --exclude 192.168.1.77

# Excluding Multiple Hosts

nmap 192.168.1.10-100 --exclude 192.168.1.77,192.168.1.95,192.168.1.99


10. To Perform a Fast scan 


nmap -F 192.168.1.77


Output Here


11.Use command man

man nmap 

This will show all the available options for nmap and u can learn more about nmap 






