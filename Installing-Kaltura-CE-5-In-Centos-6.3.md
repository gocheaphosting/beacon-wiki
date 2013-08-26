Kaltura provides different services like Streaming, Transcoding, Advertising, GeoIP, Syndication, Distribution, Security, Monitoring and more. You can easily integrate Kaltura with different Content Management Systems and Learning Management Systems like WordPress, Joomla, Drupal, Moodle, Sakai, Blackboard, Microsoft Sharepoint.


Now let me explain about how to install kaltura in CentOS Here i chose community edition for installtion. Before going to install Kaltura CE 5 you need to satisfy the prerequisites of Hardware and Software Requirements.

### Hardware Specifications

The Hardware requirements should vary according to the expected usage estimations. The minimum requirements to install Kaltura is as follow is


```

1 GB RAM
2GHz + Dual Core Processor (preferably Multi-Core Intel based)
5 GB Free Hard disk Space [ It depends upon the usage, but 5 GB is enough for the installation ]

```


### Software Specifications


```
Web Server ( Apache )
Database Server ( MySQL )
Mail Server ( SMTP / Postfix )
Application Packages ( Java, PHP )
Monitoring ( Xymon – This is optional)
Kaltura CE ( Main Application in tar file)
```



First we need to update the Server 

```
# yum install update

```


![1](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_001.png)


Install the gcc compiler 


```
# yum install gcc* -y
# yum install vim make rsync
```

![2](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_002.png)


### Install Apache server 


```

# yum install httpd* -y
# yum install httpd-devel -y

```


Enable the modules in Apache.conf file at follwing lines 



![4](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_004.png)




```

#  190 LoadModule rewrite_module modules/mod_rewrite.so
#  173 LoadModule headers_module modules/mod_headers.so
#  171 LoadModule expires_module modules/mod_expires.so
#  212 LoadModule filter_module modules/mod_filter.so
#  172 LoadModule deflate_module modules/mod_deflate.so
#  168 LoadModule env_module modules/mod_env.so
#  191 LoadModule proxy_module modules/mod_proxy.so

```

Save and Exit from the apache.conf 


By Default There will be no file_cache module in apache we need to install it using Compiling

To get it use this below Link


```

wget http://olex.openlogic.com/content/openlogic/apache/2.2.15/openlogic-apache-2.2.15-all-src-1.zip

```


![5](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_005.png)



Unzip it under any were 



```

# unzip openlogic-apache-2.2.15-all-src-1.zip


```


![6](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_006.png)


Open the Directory 


```

# cd apache-2.2.15-src/modules/cache/


```


![7](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_007.png)



Install the module file_cache using Below Command


```

# apxs -i -a -c mod_file_cache.c


```

![8](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_008.png)


If not Its Help to install Use the below Script to get install all gcc 


```
#!/bin/sh -e

if [ `id -u` -ne 0 ];then
	echo "You need to be root for this."
	exit 1
fi
if [ `which yum 2>/dev/null` ];then
    TOOL=yum
    DEV_PACK=httpd-devel
    APACHE_VER=`rpm -q  httpd --queryformat %{version}`
elif [ `which apt-get 2>/dev/null` ];then
    TOOL=apt-get
    DEV_PACK=apache2-dev
    APACHE_VER=`dpkg -l apache2|grep ii|awk -F " " '{print $3}'|awk -F "-" '{print $1}'`
fi
TMPDIR=/tmp/compile_mod_cache_file.$$
mkdir -p $TMPDIR
$TOOL install $DEV_PACK wget gcc
wget http://archive.apache.org/dist/httpd/httpd-$APACHE_VER.tar.gz -O $TMPDIR/httpd-$APACHE_VER.tar.gz
tar zxf $TMPDIR/httpd-$APACHE_VER.tar.gz -C $TMPDIR/
cd $TMPDIR/httpd-$APACHE_VER/modules/cache
if [ `which apxs2 2>/dev/null` ];then
	APXS_BIN=apxs2
else
	APXS_BIN=apxs
fi
$APXS_BIN -i -a -c mod_file_cache.c
echo "Should I delete $TMPDIR? [y|n]"
read ANS
if [ "$ANS" = 'Y' -o "$ANS" = 'y' ];then
	rm -rf $TMPDIR
fi
```

Save it as script.sh then Chmod 777 and Execute as ./script.sh



Verify its Added or not by Opening the httpd.conf Configuration file 

Below Line 215 as 


```
# cat /etc/httpd/conf/httpd.conf
# 216 LoadModule file_cache_module  /usr/lib64/httpd/modules/mod_file_cache.so


```


![9](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_009.png)


Restart the Apache


```

# /etc/init.d/httpd start

```


![10](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_010.png)


### PHP Installation 


PHP 5.2.x/PHP 5.3.x are supported.
Both php and php-cli must Need to Installed.
The following php extensions should be included and enabled:
fileinfo, bc, calander, date, filter, zlib, hash, mbstring, openssl, pcre, Gd, Curl, Memcache, Mysql, Mysqli, Exif, ftp, iconv, json, Session, apc (recommended), spl, DOM, SimpleXML, xml, xsl, ctype, ssh2 



```

# yum install php php-cli php-devel php-gd php-mbstring php-xml php-pecl-memcache.x86_64 php-pecl-memcached

```


![11](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_011.png)


Check Which Modules are installed For PHP Using command 


```

# php -m

```


If its not Available there we need to install it one by one manually 
As im Doing here below 


```
# yum install libtool
# yum install php-bcmath
# yum install memcached
# yum install php-mysql
# yum install ImageMagick
# yum install php-pecl-memcache
# yum install php-xml
# yum install fileinfo bc calander date filter zlib hash mbstring openssl pcre Gd Curl Memcache Mysql Mysqli
# yum install php-cli.x86_64 php-common.x86_64 php-dba.x86_64 php-devel.x86_64 php-embedded.x86_64 php-enchant.x86_64 php-fpm.x86_64 php-gd.x86_64 php-imap.x86_64 php-intl.x86_64 php-ldap.x86_64 php-mbstring.x86_64
# pecl install apc
```

![12](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_012.png)


But Need to Follow Some Steps For Enabling SSH2 and APC

Follow This Step to Get install SSH2 Module For Php

Firstly, we’re going to install the dependencies:



```
yum install gcc php-devel php-pear libssh2 libssh2-devel

```

These will allow us to build the SSH2 extension using pecl.


```
pecl install -f ssh2

```

After running that command, it should stop at a line like


```
.........done: 26,223 bytes
6 source files, building
running: phpize
Configuring for:
PHP Api Version:         20090626
Zend Module Api No:      20090626
Zend Extension Api No:   220090626
libssh2 prefix? [autodetect] :

```

All you have to do is hit Enter and it should detect the proper path. Once the install is completed, you just have to tell PHP to load the extension when it boots.

```
touch /etc/php.d/ssh2.ini
echo extension=ssh2.so > /etc/php.d/ssh2.ini
```

Now restart your webserver and test to see if the changes took effect.
And Chek The Module Installed 


```
/etc/init.d/httpd restart
php -m | grep ssh2

```


First, we need the pecl command so we can download and install APC from the repositories.

Do to so, we execute the following command:


```
# yum install pcre-devel

# yum install php-pear

```


But, this will not run on its own, we need the following package for the phpize command:


```
# yum install php-devel

```

We also need the apxs command, which is installed via the following package:

```

# yum install httpd-devel

```
Now we have all the software we need, so we install apc via the pecl command:


```

# pecl install apc 


```

Once that finishes, we need to enable apc in Apache's configuration. the following command should do this for us.


```

echo "extension=apc.so" > /etc/php.d/apc.ini


```


Then we restart Apache:


```

/etc/init.d/httpd start

```

Output Should Be Like this If not u not Done ...


```
[root@kaltura ~]# php -m
[PHP Modules]
apc
bcmath
bz2
calendar
Core
ctype
curl
date
dba
dom
enchant
ereg
exif
fileinfo
filter
ftp
gd
gettext
gmp
hash
iconv
imap
intl
json
ldap
libxml
mbstring
memcache
mysql
mysqli
openssl
pcntl
pcre
PDO
pdo_mysql
pdo_sqlite
Phar
readline
Reflection
session
shmop
SimpleXML
sockets
SPL
sqlite3
ssh2
standard
tokenizer
wddx
xml
xmlreader
xmlwriter
xsl
zip
zlib

[Zend Modules]
```


Create a File info.php under www/hmtl to see weather all the php extensions were installed Properly 


```

# vim /var/www/html/info.php

```


![14](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_014.png)



Then Add these Codes to Get all php infos 


```
<?php phpinfo(); ?>

```

Verify that the following settings within the php.ini file are set on each server (for both php and php-cli)



For PHP 5.3: Verify that request_order parameter includes C, G and P (recommended: "CGP")
In Line 682



```

# vim /etc/php.ini

# request_order = "CGP"


```

![15](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_015.png)


For PHP 5.2: Verify that variables_order parameter in php.ini includes C, G and P.



Please verify that date.timezone parameter is set to the right timezone.

Have a lot for ur location Time Zone in php


```

http://php.net/manual/en/timezones.asia.php

```

Change in Line number 946


```

date.timezone = Asia/Calcutta

```

![16](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_016.png)


And change Longitude and Latitude too With Value 


```
date.default_latitude = 28.40
date.default_longitude = 77.13

```


![17](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_017.png)


### MSQL Installation 

MySQL 5.1.37 or higher

Install mysql and its Depencies , Already many of them were installed 


```

# yum install mysql mysql-server mysql-devel

```


Please verify that mysql server character set is UTF8.

```
# vim /etc/my.cnf

```

Give The Entry in these headings


```
[mysqld]
character-set-server = utf8

[client]
default-character-set=utf8

```


The following settings should be added to the my.cnf file



```
lower_case_table_names = 1
thread_stack = 262144
open_files_limit = 20000

```

Then Restart the mysql to take effect 


```

# /etc/init.d/mysqld restart


```


Output Will be like this 


```
Stopping mysqld:                                           [  OK  ]
Initializing MySQL database:  Installing MySQL system tables...
OK
Filling help tables...
OK

To start mysqld at boot time you have to copy
support-files/mysql.server to the right place for your system

PLEASE REMEMBER TO SET A PASSWORD FOR THE MySQL root USER !
To do so, start the server, then issue the following commands:

/usr/bin/mysqladmin -u root password 'new-password'
/usr/bin/mysqladmin -u root -h kaltura.example.com password 'new-password'

Alternatively you can run:
/usr/bin/mysql_secure_installation

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

```


![18](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_018.png)


After all Mysql Entry the Conf File looks like this 


```

[mysqld]
character-set-server = utf8
lower_case_table_names = 1
thread_stack = 262144
open_files_limit = 20000
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

[client]
default-character-set=utf8

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

```

![19](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_019.png)


### Set MySQL password


```

# /usr/bin/mysqladmin -u root password 'admin123$'


```


You can login mysql by using the following command


```

# mysql -u root -p

```

These Packages Want to be installed 


```

# yum install curl
# yum install memcached
# yum install memcached libmemcached libmemcached-devel php-pecl-memcache python-memcached.noarch perl-Cache-Memcached.noarch
# yum install ImageMagick ImageMagick-perl
# yum install ImageMagick-c++ ImageMagick-c++-devel ImageMagick-devel ImageMagick-perl ImageMagick autotrace

```



### 32-bit packages required on 64-bit servers


Some of the binaries that are in use by the Kaltura Platform are available in a 32-bit compiled version only. To enable these binaries,
the following packages (or equivalents per Linus Distribution) may be required on a 64-bit server



```

# yum install glibc.i686 , ncurses-libs, zlib-1.2.x , freetype , bzip2-libs

# yum install zlib.i686

```

### Java Runtime Environment Installation



Here we can Find 


```

http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html

```


Download this Using Wget 


```

# wget http://download.oracle.com/otn/java/jdk/7u21-b11/jre-7u21-linux-x64.rpm?AuthParam=1377177239_311418c441de9add2ec7f76b92ad6904

```

Then Change the Extension using move 


```

# mv  jre-7u21-linux-x64.rpm\?AuthParam\=1377177239_311418c441de9add2ec7f76b92ad6904 jre-7u21-linux-x64.rpm

```


Then Install using rpm command 

```
# rpm -vh jre-7u21-linux-x64.rpm
```


Output Will be like this 



```
rpm: --hash (-h) may only be specified during package installation
[root@kaltura ~]# rpm -ivh jre-7u21-linux-x64.rpm 
Preparing...                ########################################### [100%]
   1:jre                    ########################################### [100%]
Unpacking JAR files...
	rt.jar...
	jsse.jar...
	charsets.jar...
	localedata.jar...

```


![20](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_020.png)


### Pentaho data integration


The pentaho data integration package (version 3.2) is required for the analytics module. It should be downloaded and extracted to the/usr/local/pentaho/pdi directory


Create the /usr/local/pentaho/ directory


```

# mkdir /usr/local/pentaho/

```


![21](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_021.png)



Change to the/usr/local/pentaho/directory



```
# cd /usr/local/pentaho/

```


Download the pentaho data-integration package.



```

#wget http://sourceforge.net/projects/pentaho/files/Data%20Integration/3.2.0-stable/pdi-ce-3.2.0-stable.tar.gz/download

```


Extract the pentaho data-integration package.



```

# tar -xvfz pdi-ce-3.2.0-stable.tar.gz -C /usr/local/pentaho


```


Rename the data-integration root directory into pdi.


```

# mv data-integration pdi


```


![22](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_022.png)


Mail Server Setup

Install MTA in server


```

# yum install postfix

```



### Xymon – Monitoring Tool



Download Xymon from sourceforge


```

# wget http://sourceforge.net/projects/xymon/files/latest/download

```

![23](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_023.png)


Xymon requires rrdtools and pcre. So install those packages before to proceed



```

# yum install rrdtool rrdtool-devel rrdtool-tcl rrdtool-ruby rrdtool-python rrdtool-php rrdtool-perl pcre pcre-static pcre-devel mingw32-pcre.noarch

```


![24](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_024.png)



Extract it using tar -zxvf 



![25](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_025.png)



After completing this installation start the xymon installation. You can install Xymon by simple steps


```

# ./configure
# make
# make install

```

![26](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_026.png)



During the installation it will prompt you many queries, give the proper answer else the installation process will exit



Disable SELinux



```

# vim /etc/sysconfig/selinux


```

Change the selinux from enforcing to disabled


```

# selinux = disabled

```

![3](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_003.png)



We Have to restart the server if selinux Disable option need to take effect 

Then after the restart to check weather the selinux was disabled use command 


```

# sestatus 

```

Then Download the kaltura CE From Official Website 



```

# wget http://www.kaltura.org/releases/kalturaCE/29044

```


Then Extract it using tar 



```

# tar -xzvf kalturaCE_v5.0.0.tgz

```

![27](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_027.png)


Navigate to the extracted Directory for installation 


```

# cd kalturaCE_v5.0.0

```

Run the php Installation Script as follow 


```
# # php install.php

```


![28](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_028.png)


	
Its Started For Setup Here u can see the Step by step below


```

Thank you for installing Kaltura Video Platform - Community Edition

A previous installation attempt has been detected, do you want to use the input you provided during you last installation? (Y/n)
> n

In order to improve Kaltura Community Edition, we would like your permission to send system data to Kaltura.
This information will be used exclusively for improving our software and our service quality. I agree (Y/n)
> y

If you wish, please provide your email address so that we can offer you future assistance (leave empty to pass)
> babinlonston@blabla.com

Please provide the following information:

The following apachectl script has been detected: /usr/sbin/apachectl. Do you want to use this script to run your Kaltura application? Leave empty to use or provide a pathname to an alternative apachectl script on your server.
> 

The following PHP binary has been detected: /usr/bin/php. Do you want to use this script to run your Kaltura application? Leave empty to use or provide a pathname to an alternative PHP binary on your server.
> 

Full target directory path for Kaltura application (leave empty for /opt/kaltura)
> 

Please enter the domain name/virtual hostname that will be used for the Kaltura server (without http://)
> 192.168.1.3

Your primary system administrator email address
> babinlonston@blabla.com

The password you want to set for your primary administrator
> admin123$

Database host (leave empty for 'localhost')
> 

Database port (leave empty for '3306')
> 

Database username (with create & write privileges)
> root

Database password (leave empty for no password)
> admin123$

The URL to your xymon/hobbit monitoring location. Xymon is an optional installation. Leave empty to set manually later
Examples:
http://www.xymondomain.com/xymon/
http://www.xymondomain.com/hobbit/
> http://192.168.1.3/xymon


Verifing prerequisites

Checking for leftovers from a previous installation

Installation is now ready to begin. Start installation now? (Y/n)
> y

Copying application files to /opt/kaltura
current working dir is /root/kalturaCE_v5.0.0
Copying binaries for linux 64bit
Replacing configuration tokens in files
Changing permissions of directories and files
Creating and initializing 'kaltura' database
Creating and initializing 'kaltura_sphinx_log' database
Creating data warehouse
Creating Dynamic Enums
Configure sphinx
Populate sphinx tables
Changing permissions of directories and files
Creating system symbolic links
Deploying uiconfs in order to configure the application
Creating the uninstaller
Running the generate script
Running the batch manager
Running the sphinx search deamon
Executing sphinx dameon 
Executing in background nohup /opt/kaltura/app/plugins/sphinx_search/scripts/watch.daemon.onprem.sh 
Executing in background chkconfig sphinx_watch.sh on 
Changing permissions of directories and files
Post installation email cannot be sent

Installation Completed Successfully.
Your Kaltura Admin Console credentials:
System Admin user: babinlonston@blabla.com
System Admin password: admin123$

Please keep this information for future use.

To start using Kaltura, please complete the following steps:
1. Add the following line to your /etc/hosts file:
	127.0.0.1 192.168.1.3
2. Add the following line to your Apache configurations file (Usually called httpd.conf or apache2.conf):
	Include /opt/kaltura/app/configurations/apache/my_kaltura.conf
3. Restart apache
4. Browse to your Kaltura start page at: http://192.168.1.3/start

```

***


![29](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_029.png)

![30](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_030.png)

![31](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_031.png)



That's it 

After installation we can see the Home page of Kaltura Like this below 


![32](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_032.png)




![33](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_033.png)



![34](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_034.png)



![35](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_035.png)



![36](https://github.com/babinlonston/Centos-Linux-Stuffs/raw/master/Installing%20Kaltura/Selection_036.png)



***



### Troubleshooting Database


IF This Error Recived For Database 

```
Issue:- ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
```

Stop the Database 

```

# /etc/init.d/mysqld stop

```

Then Skip the tables 


```

# mysqld_safe --skip-grant-tables &


```


Starting mysqld daemon with databases from /var/lib/mysql


```

# mysql -u root


```


See the Databases


```

# show databases;

```



choose the database Which we need to change 


```

# use mysql;


```


Then change the password 
Here i Have used password as admin123$


```

mysql> update user set password=PASSWORD("admin123$") where User='root';

```


Then Flush it 


```

# flush privileges;


```


Quit from the Mysql Databse 


```
\q  or quit

```


Then Restart the Mysql Service 



```

#  /etc/init.d/mysql restart

```


Then Login With Created Password  >>>> admin123$



```

# mysql -u root -p

```


Enter Password : 



Give ur password and get login 

