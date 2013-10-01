
Installating and Configuring Samba Share in ubuntu 12.04, 13.04


Install Samba Sahre in ubuntu step by step 


This Below Command will install the samba Main configuration 

```

# sudo apt-get  install  samba samba-common

```


Install some dependencies 



```

# sudo apt-get install python-glade2

```


Installing Samba Server configuration Tool


```

# sudo apt-get install system-config-samba

```


Add User for Filesharing 


```

# useradd samba

```


Give a Password for the user 


```

# passwd samba

```


Create a Samba Password for the Samba User we have Created 
This Password is Used for File sharing 



```

#sudo smbpasswd -a samba 

```


That's it...

Share a Folder by right clikc and choosing sharing option With permission or Without permission ...
