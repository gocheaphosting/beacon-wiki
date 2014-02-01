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
#####Configure the Auto Installer script (this is a CentOS/RHEL shell script to automate most of the install steps, if you're running on other distros, consider rewriting the shell scripts or following the manual install guide in the README file)
```
   cd ~/ce-packager/git-repositories/ce-configurations/auto_install
   vim user_input.ini
```
##### Edit the following settings:

* Set TIME_ZONE to your Time Zone - http://www.php.net/manual/en/timezones.php
* Set KALTURA_FULL_VIRTUAL_HOST_NAME to your FQDNS (e.g. kaltura.mydomain.com without protocol. FQDNS: http://www.linfo.org/fqdn.html)
* Set NFS_SERVER, DWH_HOST, RED5_HOST and SPHINX_DB_HOST to 127.0.0.1
* Set ADMIN_CONSOLE_ADMIN_MAIL and TEST_PARTNER_EMAIL to your email
* Set DB1_USER to root
* Set DB1_PASS to your desired root MySQL password (the install will auto-generate pass for the kaltura user)
* Make sure that at the bottom: WORK_MODE=http (or https if you have valid SSL certificate installed)
#####Run the Kaltura install packager script
```
   cd ~/ce-packager/packaging/
   php package.php /tmp/kalturaCEinstaller false CE v6.2.0 dev
   rsync -av ~/ce-packager/git-repositories/ce-configurations/auto_install/ /tmp/kalturaCEinstaller
```