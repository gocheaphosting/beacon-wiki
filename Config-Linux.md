## How to rename the Ubuntu Server

- open terminal
 - sudo vi /etc/hostname

 change it...

  - update thehostname in /etc/hosts if it's there

  - sudo vi /etc/hosts

- sudo service hostname start 

## Change Ubuntu Server from DHCP to a Static IP Address

- sudo vi /etc/network/interfaces

For the primary interface, which is usually eth0, you will see these lines:

```
auto eth0
iface eth0 inet dhcp
```
change the lines to as below

```
auto eth0
iface eth0 inet static
address 192.168.1.100
netmask 255.255.255.0
network 192.168.1.0
broadcast 192.168.1.255
gateway 192.168.1.1
dns-nameservers 192.168.1.1 8.8.8.8 8.8.4.4
```

- Now add in the DNS settings by editing the resolv.conf file:

 - To add more entries to /etc/resolv.conf, create a /etc/resolvconf/resolv.conf.d/base and add them there.

sudo vi /etc/resolvconf/resolv.conf.d/base

```
nameserver 192.168.1.1
nameserver 8.8.8.8
```

- now tell resolvconf to regenerate resolv.conf.

sudo resolvconf -u


- now remove the dhcp client or might need to remove dhcp-client3 instead

sudo apt-get remove dhcp-client
sudo apt-get remove dhcp-client3

- restart the networking service
sudo /etc/init.d/networking restart

## Create a new Linux User

```
sudo useradd elf
passwd elf
```

## Add a user to sudoers

Open Sudoers file

sudo visudo

will open the /etc/sudoers file in GNU nano

if not try export EDITOR="nano" and try again sudo visudo

add the below line to the end of file

username ALL=(ALL) ALL change the user name before you issue the commands

Then perform WriteOut with the CTRL+O

Editor will ask you for the file name to write into

default will be /etc/sudoers.tmp

File Name to Write: /etc/sudoers.tmp

Change that to /etc/sudoers

File Name to Write: /etc/sudoers

A prompt will be dispalyed

File exists, OVERWRITE ?

or

Save file under DIFFERENT NAME ?

In both cases, press Y

Quit the nano editor with CTRL+X

Done!




Find out who is monopolizing or eating the CPUs

Finally, you need to determine which process is monopolizing or eating the CPUs. Following command will displays the top 10 CPU users on the Linux system.

```
# ps -eo pcpu,pid,user,args | sort -k 1 -r | head -10
OR
# ps -eo pcpu,pid,user,args | sort -r -k1 | less
```

