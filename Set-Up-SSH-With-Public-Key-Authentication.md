## Installing SSH On The Server

```
apt-get install ssh
```

##Installing SSH client on the desktop
```
apt-get install openssh-client
```

Now Switch back to your normal user ( sysadmin ). Then type these commands in order:

```
mkdir ~/.ssh
chmod 700 ~/.ssh
cd ~/.ssh
```

Now generate the key-pair, a public-key and a private-key. The public-key will be placed on the server, and users will log in with their private-key. When asked, type your passphrase (it'll be needed for future logins, so remember it!):

```
ssh-keygen -t rsa -C "A comment... usually an email is enough here..."
```

Nowlog in with SSH, and copy the public key to its right place:

```
ssh sysadmin@remotehost
mkdir ~/.ssh
chmod 700 ~/.ssh
cat id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
mv id_rsa.pub ~/.ssh
logout
```
Delete the public key on the desktop, because otherwise the SSH client mayn't allow to log in to the server. So, type this command:

```
rm id_rsa.pub
```

And then we log back:

```
ssh sysadmin@remotehost
```

This will ask for the passphrase. Type it, then and correct passphrase should take the command prompt to  SSH-environment!




