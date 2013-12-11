There are Tow Types of Groups 

I.  Primary Group
II. Secondary Group or Supplementary Group

There are two commands we going to use are useradd for new user adding, If a Existing User use command usermod.

The User information's were Stored Under 

```
 # /etc/passwd
```

The Password Information of Users are Stored Under 

```
# /etc/shadow
```

The Group Informations were stored under 

```
# /etc/group 
```

The password information of groups were stored under 

```
# /etc/gshadow
```

The Default user add Informations and groups were stored under 

```
# /etc/defualt/useradd
```


This Directory containing default files.

```
# /etc/skel/
   
```
        
This file Contain Shadow password suite configuration.

```
# /etc/login.defs
           
```

Let we see some of the commands and what it does while execution

1. Useradd

If there is a user as linuxuser, And need to add a new group for the exisiting User Use command.

Here linuxuser is user and group is sysadmin

```
syntax:

useradd -G (group-name) username

# useradd -G sysadmin linuxuser
```

Output:

```
linuxuser:x:1000:1000:sysadmin
```

2. Adding Single Group to a Single User

-a Add the user to the supplementary group's, Use only with the -G option

-G A list of supplementary groups which the user is also a member of.

```
Syntax:
usermod -a -G (group-name) User
```

3. Adding Multiple Groups for a Single User

```
Syntax:

usermod -a -G (group1,group2) User

usermod -a -G sysadmin,ftp linuxuser
```

Output:

```
linuxuser:x:1001:sysadmin,ftp
```

4.To Remove a Group


```
# groupdel linuxuser

```

This will Remove the linuxuser group from the linux system

5. To know the Group's in Linux Systems

```
# cat /etc/group
 
```