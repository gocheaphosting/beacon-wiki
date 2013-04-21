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

auto eth0

iface eth0 inet dhcp

change the lines to as below

auto eth0

iface eth0 inet static

address 192.168.1.100

netmask 255.255.255.0

network 192.168.1.0

broadcast 192.168.1.255

gateway 192.168.1.1


- Now add in the DNS settings by editing the resolv.conf file:

sudo vi /etc/resolv.conf

nameserver 192.168.1.1
nameserver 8.8.8.8

- now remove the dhcp client or might need to remove dhcp-client3 instead

sudo apt-get remove dhcp-client
sudo apt-get remove dhcp-client3

- restart the networking service
sudo /etc/init.d/networking restart




