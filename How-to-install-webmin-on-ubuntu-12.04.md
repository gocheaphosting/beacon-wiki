### open terminal install the following packages
```
sudo apt-get install perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions python
```
###download webmin deb package using the following command
```
 wget http://prdownloads.sourceforge.net/webadmin/webmin_1.580_all.deb
```
### install downloaded package using following command
```
sudo dpkg -i webmin_1.580_all.deb

```

### Login 

```
https://ipaddress:10000
```
  -Webmin will allow any user who has this sudo capability to login with full root privileges.
  - Reference link : http://www.ubuntugeek.com/how-to-install-webmin-on-ubuntu-12-04-precise-server.html