NFS allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files.

Step 1:

Install the NFS server package Using command 

```
# sudo apt-get install nfs-kernel-server
```

Step 2: 

Create Directories Which we going to Share from Server
Here I'm Creating Three Directories
And I'm Sharing my Home Directory too
 
```
# sysadmin@ubuntu:/$ sudo mkdir nfsshare
# sysadmin@ubuntu:/$ sudo mkdir usersfiles
```

Step 3:

Edit the NFS Configuration File 

```
# sysadmin@ubuntu:~$ sudo vim /etc/exports
```

Add the Directories Which need to share and Its Rights as Read/Write                     

```
/home           *(rw,sync,subtree_check,no_root_squash)
/nfsshare       192.168.1.99(rw,sync,subtree_check,no_root_squash)
/usersfiles     *(ro,sync,subtree_check, no_root_squash)
```

Step 4:

Create Some files Inside the Directory which we going to share 

```
#mkdir linux
#touch linux{1..5}

```

Here is the Output 

```
sysadmin@ubuntu:/nfsshare$ ll
total 12
drwxr-xr-x  3 root root 4096 Nov 14 14:20 ./
drwxr-xr-x 25 root root 4096 Nov 14 14:14 ../
drwxr-xr-x  2 root root 4096 Nov 14 14:19 linux/
-rw-r--r--  1 root root    0 Nov 14 14:20 linux1
-rw-r--r--  1 root root    0 Nov 14 14:20 linux2
-rw-r--r--  1 root root    0 Nov 14 14:20 linux3
-rw-r--r--  1 root root    0 Nov 14 14:20 linux4
-rw-r--r--  1 root root    0 Nov 14 14:20 linux5
```

Step 5:


Restar start the NFS Service using command 

```
# sudo /etc/init.d/nfs-kernel-server start

sysadmin@ubuntu:/nfsshare$ sudo /etc/init.d/nfs-kernel-server start
 * Exporting directories for NFS kernel daemon...                                [ OK ] 
 * Starting NFS kernel daemon                                                    [ OK ] 
```

Client Side Configuration:

Install the Client package 

```
# sudo apt-get install nfs-common
```

Create the Directory Were we need to get Mount the NFS share 

```
# sudo mkdir -p /home/sysadmin/nfsshare
sysadmin@system99:~$ sudo mkdir -p /home/sysadmin/nfsshare
sysadmin@system99:~$ cd /home/sysadmin/nfsshare/
sysadmin@system99:~/nfsshare$ 
sysadmin@system99:~/nfsshare$ pwd
/home/sysadmin/nfsshare
```

Mount the NFS Share from Server to Our Local System Using Mount Command 

```
# sysadmin@system99:~$ sudo mount 192.168.1.160:/nfsshare /home/sysadmin/nfsshare
```

List the Files in Our Local System Which was Created in NFS server

```
sysadmin@system99:/$ ls -la /home/sysadmin/nfsshare/
total 12
drwxr-xr-x  3 nobody   nogroup  4096 Nov 14 14:20 .
drwxr-xr-x 45 sysadmin sysadmin 4096 Nov 14 14:39 ..
drwxr-xr-x  2 nobody   nogroup  4096 Nov 14 14:19 linux
-rw-r--r--  1 nobody   nogroup     0 Nov 14 14:20 linux1
-rw-r--r--  1 nobody   nogroup     0 Nov 14 14:20 linux2
-rw-r--r--  1 nobody   nogroup     0 Nov 14 14:20 linux3
-rw-r--r--  1 nobody   nogroup     0 Nov 14 14:20 linux4
-rw-r--r--  1 nobody   nogroup     0 Nov 14 14:20 linux5                

```