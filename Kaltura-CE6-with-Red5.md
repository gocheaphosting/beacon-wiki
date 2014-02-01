#### Installation Guide (simply follow line by line):
```
    yum update
    init 6
    yum install git wget dos2unix php-cli
    vim /etc/hosts 
```
##### Add at the bottom:
```
    127.0.0.1 media.arrivuapps.com
```
```
    ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
    yum install ntp* -y
    ntpdate 0.us.pool.ntp.org 
    vim /etc/ntp.conf 
```
##### Around line 21 add the following lines:
```
   server 0.us.pool.ntp.org
   server 1.us.pool.ntp.org
```
```
   vim /etc/ntp/step-tickers
```
#####At the end of the file, add the following lines:
```
   0.us.pool.ntp.org 
   1.us.pool.ntp.org   
````
##### Then run:
```
   service ntpd start
   chkconfig ntpd on
```
##### Clone the ce-packager repository and fix the git submodules to checkout the known latest working branches:
```
   cd ~/   
   git clone https://github.com/kaltura/ce-packager.git  
   cd ~/ce-packager/  
   git submodule update --init  
   cd ~/ce-packager/git-repositories/KalturaServer  
   git reset --hard origin/falcon  
   cd ~/ce-packager/  
   git submodule update git-repositories/ce*  
   find ~/ce-packager/git-repositories/ce* -type d -name .git -print -execdir git pull origin master \;  
   cd ~/ce-packager/git-repositories/ce-branding  
   git checkout f79d56cd9026e2474ef82f2c7a39ee8f43a2c7ee  
```