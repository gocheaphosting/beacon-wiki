How Configure a SSH Secure Login to Remote Servers.

Here we can see how to change the ssh login Port and Restrict some Privileges for securing server's

Note: If root is disabled then Only we can login into remote system using a local user, And we can switch to root user in RHEL and Centos.
By Default we can't switch user in Ubuntu, So we need to add the local user to sudo group while a local user creation.


1.First we need to Create a User 

```
Syntx :

In RHEL, Centos

# useradd {user-name}
```

Command Execution:

```
# useradd sysadmin

```

Output:

```
[root@dnsserver ~]# useradd sysadmin
[root@dnsserver ~]# passwd sysadmin
Changing password for user sysadmin.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

In Ubuntu, Debian

```
# useradd -m -s /bin/{shell} {user-name}

```

In Ubuntu by Default it Won't add the home Directory so we need mention -m to Create Home Directory

Command Execution:

```
# sudo useradd -m -s /bin/bash sysadmin

```

Output:

```
root@ubuntu:~$ sudo useradd -m -s /bin/bash sysadmin
root@ubuntu:~$ sudo passwd sysadmin
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
```

1 .Edit the sshd_config file for more 


In RHEL, Centos

```
# vim/etc/ssh/sshd_config

```

In Ubuntu, Debian

```
# sudo vim/etc/ssh/sshd_config
```

2. Then Edit the Port 22 to 2222 (Here im using the port as 2222)
   (In Line Number 13 in RHEL & Centos, If its Ubuntu Line 5) 

In RHEL and Centos Uncomment the # and change the port 22 to 2222

```
# Port 2222

```

In Ubuntu 

Just change the 22 to 2222 in Line 5


3. Then Change the settings for grace time and Root Login Restriction 
   (In Line number 26 & 27 )
If the Grace time was low to 30 seconds, if we trying to login we need to login with in 30 seconds if not session will end.
Restrict the root Login, While Loging into ssh we don't need to enter into root, so what at first step we have created a local user

In RHEL and Centos 
Line 41 to 45 Uncomment the Hashes(#)

```
LoginGraceTime 1m
PermitRootLogin no
StrictModes yes
MaxAuthTries 3
MaxSessions 10
```

In Ubuntu 

```
# 26 LoginGraceTime 30
# 27 PermitRootLogin no
```

4. The Restart the SSH Service by using command 

In RHEL, centos.

```
# Service sshd restart

# /etc/init.d/sshd restart 
 
```


In Ubuntu & Debian 

```
# sudo /etc/init.d/ssh restart 

```

5. Then Try to Log in from any Machine's using the IP and Port Number Mentioned as Below
   Here we have used the sysadmin user to login to the remote server, Because root user login has been disabled,
   After Login we can switch to root user using su - in Centos or RHEL systems (or) sudo su in Ubuntu or Debian System

  Here -p mention the Port Number to ssh login

```
# ssh sysadmin@192.168.1.200 -p 2222

```