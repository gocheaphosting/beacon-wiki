First Install Ubuntu 12.04 Lts 
 
While installing Choose US as the Time server .

After Reboot 

Change the IP to Any One of the Static IP Range Between 192.168.1.75 to 192.168.1.254

```
auto lo
iface lo inet loopback
auto eth0
iface eth0 inet static
        address 192.168.1.78
        netmask 255.255.255.0
        gateway 192.168.1.1
        network 192.168.1.0
        broadcast 192.168.1.255
        dsn-nameservers 8.8.8.8 192.168.1.1

```

After that Change the Host name to system xx to system xx in 
```
#/etc/hosts

#vi /etc/hosts

127.0.0.1       localhost
192.168.1.78    system3 3

```
Setting up Hostname in /etc/hostname file

```

echo "hostname here" > /etc/hostname
hostname -F /etc/hostname

```

Then change the Nameserver under resolv.conf

```
# Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
#     DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN
nameserver 192.168.1.1
nameserver 8.8.8.8


```

If the resolv.conf Overwritten After the System Restart to its Default 
Edit the Base file of resolvconf 

Add the name servers here then it own't Overwritten after the Restart 
```
#vi /etc/resolvconf/resolv.conf.d/base

nameserver 192.168.1.1
nameserver 8.8.8.8

```

Restart the Network Using Command 

```
#sudo etc/init.d/networking restart

```

             (Or)

Restart the Machine Once 
 
--------------------------------------------------

Install vim 

```
#apt-get install vim 
```

Configure Apt Cacher NG by adding a file called 02proxy

```
# vim /etc/apt/apt.conf.d/02proxy

```

Add the Apt Cacher Server's IP Address in that created Proxy file 

```
Acquire::http {Proxy "http://192.168.1.30:3142"; };

```
------------------------------------------------------------------------------------
Install Cinnamon 1.6.4 in Ubuntu to Change the Desktop Appearance

```
#sudo apt-get update

Install Supporting packages

#apt-get install build-essential

#sudo apt-get install python-software-properties

Get the repository by using command 

#sudo add-apt-repository ppa:gwendal-lebihan-dev/cinnamon-stable

#sudo apt-get update

#sudo apt-get install cinnamon

```

Install Google Chrome from the Reporositry

3rd Repository: Google Chrome

This repository is available for: Stable Title: Chrome browser in Google repos Description:

Google Linux repository on dl.google.com. Daily Build: no

Setup key with:


```
#wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -

Setup repository with:

#sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'

Setup package with:

#sudo apt-get update

#sudo apt-get install google-chrome-stable

```
where is the name of the package you want to install.

--------------------------------------------------------------------------------------------

Install Liber Office 4.0

To install LibreOffice 4 you will need to remove all previous versions. Run:

```
#sudo apt-get remove --purge libreoffice-core libreoffice-common

#sudo apt-get autoremove --purge

#sudo add-apt-repository ppa:libreoffice/libreoffice-4-0

#sudo apt-get update

#sudo apt-get install libreoffice

```
----------------------------------------------------------------------------------------------

Manually Install the JDK 1.7

```
#java -version

#sudo mkdir -p /usr/lib/jvm

#sudo mv jdk-7u21-linux-i586.tar.gz /usr/lib/jvm

#cd /usr/lib/jvm

#sudo tar zxvf jdk-7u21-linux-i586.tar.gz

#sudo rm jdk-7u21-linux-i586.tar.gz

#ls -l

#jdk1.7.0_21

#sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.7 .0_21/bin/javac" 1

#sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.7.0 _21/bin/java" 1

#sudo update-alternatives --set "javac" "/usr/lib/jvm/jdk1.7.0_21/bin/javac"

#sudo update-alternatives --set "java" "/usr/lib/jvm/jdk1.7.0_21/bin/java"

#sudo vi /etc/profile

Add the following entries to the bottom of your /etc/profile file:

#JAVA_HOME=/usr/lib/jvm/jdk1.7.0_21 PATH=$PATH:$JAVA_HOME/bin export JAVA_HOME export PATH

#. /etc/profile

#java -version


```

-----------------------------------------------------------------------------------------

Install Postersql 9.2


```
#sudo add-apt-repository ppa:pitti/postgresql

#sudo apt-get update

#sudo apt-get install postgresql-9.2

```

--------------------------------------------------------------------------------------------

Install Pgadmin 1.16 for managing PostgreSQL in GUI

```
#sudo apt-add-repository ppa:voronov84/andreyv

#sudo apt-get update

#sudo apt-get install pgadmin3

```

--------------------------------------------------------------------------------------------

Install Git 

```
#sudo add-apt-repository ppa:git-core/ppa

#sudo apt-get update

#sudo apt-get -y install git git-man git-svn


```

--------------------------------------------------------------------------------------------

Install Sublime Editor


```
#sudo add-apt-repository ppa:webupd8team/sublime-text-2

#sudo apt-get update && sudo apt-get install sublime-text

```


--------------------------------------------------------------------------------------------

Install byobu terminal 

```
#sudo apt-get install byobu


```

--------------------------------------------------------------------------------------------
Install Htop

```

#sudo apt-get install htop


```

--------------------------------------------------------------------------------------------

Install Ruby 1.9.3

Ruby packages for Ubuntu

```
#sudo apt-get install python-software-properties

#sudo apt-add-repository ppa:brightbox/ruby-ng

#sudo apt-get update

To install Ruby 1.9.3.

#sudo apt-get install ruby1.9.3


```

--------------------------------------------------------------------------------------------

Setup geminabox


```
#sudo apt-get install -y zlib1g-dev libxml2-dev libmysqlclient-dev \
libxslt1-dev imagemagick libpq-dev nodejs \
libxmlsec1-dev libcurl4-gnutls-dev libxmlsec1

#edit ~/.gemrc and /etc/gemrc

Add this Content in these above 2 files 

---
:update_sources: true
:sources:
- http://192.168.1.30:8808/
- http://gems.rubyforge.org/
:benchmark: false
:bulk_threshold: 1000
:backtrace: false
:verbose: true
gem: --no-ri --no-rdoc
install: --no-rdoc --no-ri 
update: --no-rdoc --no-ri

#gem install bundler

#mkdir ~/dev


```

--------------------------------------------------------------------------------------------

Mount the Samba Share to Client machine from File server 192.168.1.15

To Mount a Samba Share Permanently in client machines 

```

#sudo mkdir /home/sysadmin/public

#apt-get install smbfs

#sudo mount -t cifs //192.168.1.15/public /home/sysadmin/public

Fstab Entry For Mount point 

#//192.168.1.15/public /home/sysadmin/public     cifs    username=sysadmin,password=admin123$,_netdev    0       0

```

--------------------------------------------------------------------------------------------