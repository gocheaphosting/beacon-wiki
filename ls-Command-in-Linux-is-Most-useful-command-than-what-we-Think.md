ls Command in Linux is Most useful command than what we Think 


For Listing any files or folders we need to use the ls command in linux systems


1.ls command 

Short Listing   


root@ubuntu:~# ls
Spark               spark_2_6_3.tar.gz.1  spark_2_6_3.tar.gz.3
spark_2_6_3.tar.gz  spark_2_6_3.tar.gz.2  spark_2_6_3.tar.gz.4


2.ls -la command 

Listing with long all


root@ubuntu:~# ls -la
total 219544
drwx------  4 root root     4096 Oct 26 10:58 .
drwxr-xr-x 23 root root     4096 Oct 26 11:51 ..
-rw-------  1 root root     3099 Oct 25 17:45 .bash_history
-rw-r--r--  1 root root     3106 Apr 19  2012 .bashrc
-rw-r--r--  1 root root       62 Oct 25 17:02 .install4j
-rw-r--r--  1 root root      140 Apr 19  2012 .profile
drwxr-xr-x  3 root root     4096 Oct 25 17:43 Spark


3.ll command 

Long listing

root@ubuntu:~# ll
total 219544
drwx------  4 root root     4096 Oct 26 10:58 ./
drwxr-xr-x 23 root root     4096 Oct 26 11:51 ../
-rw-------  1 root root     3099 Oct 25 17:45 .bash_history
-rw-r--r--  1 root root     3106 Apr 19  2012 .bashrc
-rw-r--r--  1 root root       62 Oct 25 17:02 .install4j
-rw-r--r--  1 root root      140 Apr 19  2012 .profile
drwxr-xr-x  3 root root     4096 Oct 25 17:43 Spark/


4.ls -li command 

listing with inode number for all files


root@ubuntu:~# ls -li
total 219504
6032959 drwxr-xr-x 3 root root     4096 Oct 25 17:43 Spark
6032958 -rw-r--r-- 1 root root 44952405 Oct 25 16:59 spark_2_6_3.tar.gz
6032977 -rw-r--r-- 1 root root 44952405 Oct 25 17:08 spark_2_6_3.tar.gz.1
6032936 -rw-r--r-- 1 root root 44952405 Oct 25 17:12 spark_2_6_3.tar.gz.2


5.ls -lh command 

listing with human readeable format for file size's

root@ubuntu:~# ls -lh
total 215M
drwxr-xr-x 3 root root 4.0K Oct 25 17:43 Spark
-rw-r--r-- 1 root root  43M Oct 25 16:59 spark_2_6_3.tar.gz
-rw-r--r-- 1 root root  43M Oct 25 17:08 spark_2_6_3.tar.gz.1
-rw-r--r-- 1 root root  43M Oct 25 17:12 spark_2_6_3.tar.gz.2
-rw-r--r-- 1 root root  43M Oct 25 17:30 spark_2_6_3.tar.gz.3
-rw-r--r-- 1 root root  43M Oct 25 17:43 spark_2_6_3.tar.gz.4


6.ls -g command 

Same as listing, But this Don't Display the Owner 

root@ubuntu:~# ls -g
total 219504
drwxr-xr-x 3 root     4096 Oct 25 17:43 Spark
-rw-r--r-- 1 root 44952405 Oct 25 16:59 spark_2_6_3.tar.gz
-rw-r--r-- 1 root 44952405 Oct 25 17:08 spark_2_6_3.tar.gz.1
-rw-r--r-- 1 root 44952405 Oct 25 17:12 spark_2_6_3.tar.gz.2


7.ls -s command 

Sort file by file size 

root@ubuntu:~# ls -s
total 219504
    4 Spark               43900 spark_2_6_3.tar.gz.1  43900 spark_2_6_3.tar.gz.3      0 test1
43900 spark_2_6_3.tar.gz  43900 spark_2_6_3.tar.gz.2  43900 spark_2_6_3.tar.gz.4      0 test.txt


8.ls -1 command 

Sort by 1 file per line 

root@ubuntu:~# ls -1
Spark
spark_2_6_3.tar.gz
spark_2_6_3.tar.gz.1
spark_2_6_3.tar.gz.2
spark_2_6_3.tar.gz.3
spark_2_6_3.tar.gz.4
test1
test.txt


9.lsusb command 

Show the USB devices In our System 

root@ubuntu:~$ lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 001 Device 002: ID 03f0:5307 Hewlett-Packard 


10.lscpu command 

Show the CPU Information in our system


root@ubuntu:~# lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                1
On-line CPU(s) list:   0
Thread(s) per core:    1
Core(s) per socket:    1
Socket(s):             1
NUMA node(s):          1
Vendor ID:             AuthenticAMD
CPU family:            21
Model:                 1
Stepping:              2
CPU MHz:               3299.443
BogoMIPS:              6598.88
Hypervisor vendor:     Microsoft
Virtualization type:   full
L1d cache:             16K
L1i cache:             64K
L2 cache:              2048K
L3 cache:              8192K
NUMA node0 CPU(s):     0


11.lsb_release command 

This command will show us the Operating system we using currently

root@ubuntu:~# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 12.04 LTS
Release:	12.04
Codename:	precise


12.lsbpci command 

This command will give us all information about the PCI Devices 


root@ubuntu:~# lspci
00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (AGP disabled) (rev 03)
00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 01)
00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 02)
00:08.0 VGA compatible controller: Microsoft Corporation Hyper-V virtual VGA


13.lsblk command 

list's the block devices

root@ubuntu:~# lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sr0     11:0    1  1024M  0 rom  
sda      8:0    0   127G  0 disk 
├─sda1   8:1    0   126G  0 part /
├─sda2   8:2    0     1K  0 part 
└─sda5   8:5    0  1021M  0 part [SWAP]



14.lsblk -f command 

List the file type in block device 


root@ubuntu:~# lsblk -f
NAME   FSTYPE LABEL MOUNTPOINT
sr0                 
sda                 
├─sda1 ext4         /
├─sda2              
└─sda5 swap         [SWAP]


15.lsattr command 

This will show the attribute of files or Directory

root@ubuntu:~# lsattr
-------------e- ./spark_2_6_3.tar.gz
-------------e- ./Spark
-------------e- ./test.txt
-------------e- ./spark_2_6_3.tar.gz.3
-------------e- ./spark_2_6_3.tar.gz.2
-------------e- ./spark_2_6_3.tar.gz.4
-------------e- ./test1
-------------e- ./spark_2_6_3.tar.gz.1



16.lshw command 

Give the Hardware information About the system


The output will be in very long listing 

17.lshal command 

If this command need to work we need to install a small package hal (Hardware Abstraction Layer)

# sudo apt-get install hal 

lshal will display all hardware abstraction layer devices and the list will be very long 

For Getting a scrolled Listing use less option as follow


# lshal | less


18.lsmod command 

using this command we can see the modules which are installed in the kernel 


root@ubuntu:~# lsmod
Module                  Size  Used by
vesafb                 13844  1 
rfcomm                 47604  0 
des_generic            21415  0 
bnep                   18281  2 
md4                    12595  0 
bluetooth             180104  10 rfcomm,bnep
nls_utf8               12557  1 
joydev                 17693  0 
cifs                  287273  2 
hid_hyperv             13164  0 
hid                    99559  1 hid_hyperv
psmouse                87603  0 
mac_hid                13253  0 
serio_raw              13211  0 
i2c_piix4              13301  0 
lp                     17799  0 
parport                46562  1 lp
hv_netvsc              22939  0 
hv_storvsc             17896  2 
hv_utils               13540  0 
floppy                 70365  0 
hv_vmbus               34543  4 hid_hyperv,hv_netvsc,hv_storvsc,hv_utils


There are more ls commands available if we install some utilites Such as 

lspcmcia, lswn, lsdvd, lsmbox, lscgroup, lspst


To know More About the ls command use command 

# man ls

