How to Install and setup a Ampache Streaming Video & Audio Media Server server in CentOS 6.3

Ampache is a a web based audio/video streaming software and file manager. It allows you to access your music & videos from anywhere, using almost any internet enabled device. It isn’t a media organizer, it just use the existing organized media files in a useful way. So you must be aware of managing and organizing the media files.


Here im using IP 192.168.1.200


Starts From Here : 


Install Apache


```

# yum install httpd -y

```

Start Apache and make it to start automatically on every reboot.


```

# /etc/init.d/httpd start

```

Make it Running While in booting 



```

# chkconfig httpd on


```


Test Apache:

Open your Browser and Enter 192.168.1.200 Now you will see the Apache home page.






Install MySQL


```

# yum install mysql mysql-server -y


```

Start the MySQL service and make to start automatically on every reboot.


```

# /etc/init.d/mysqld start

```

Make it Running in Booting 


```

# chkconfig mysqld on


```

Setup MySQL root password


```

# /usr/bin/mysql_secure_installation


```


Install PHP


```

# yum install php -y

```

Restart Apache server


```

# /etc/init.d/httpd restart


```

Test PHP:


Create a sample testphp.php file in Apache document root folder and append the lines as shown below.


```

# vim /var/www/html/testphp.php

```


Give this code in the PHP file



```

<?php
phpinfo();
?>


```


Now open the testphp.php file in browser using http://192.168.1.200/testphp.php. It will display the details about php package.


If you wanna to get MySQL support in your PHP, you should install “php-mysql” package. If you want to install all php modules just you use the command 


```

# yum install php


```

Then insatall php-mysql module 


```

# yum install php-mysql -y


```

Now open the phptest.php file in your browser using http://192.168.1.200. Scroll down and you will see the mysql module will be presented there.


Install phpMyAdmin


phpMyAdmin is a free open source web interface tool, used to manage your MySQL databases. By default phpMyAdmin is not found in CentOS official repositories. So let us install it using EPEL repository.

Download and install EPEL Repository first.


```

# wget http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm


```
Install it using RPM command


```

# rpm -ivh epel-release-6-8.noarch.rpm

```

Now install phpMyAdmin


```

# yum install phpmyadmin -y

```

Configure phpMyAdmin

Open the phpmyadmin.conf file and make the changes as shown below.


```

# vim /etc/httpd/conf.d/phpMyAdmin.conf

```

This is the code What we can see in phpMyAdmin.conf file


```
Alias /phpMyAdmin /usr/share/phpMyAdmin
Alias /phpmyadmin /usr/share/phpMyAdmin

## Comment the following Section ##
#<Directory /usr/share/phpMyAdmin/>
#   <IfModule mod_authz_core.c>
#     # Apache 2.4
#     <RequireAny>
#       Require ip 127.0.0.1
#       Require ip ::1
#     </RequireAny>
#   </IfModule>
#   <IfModule !mod_authz_core.c>
#     # Apache 2.2
#     Order Deny,Allow
#     Deny from All
#     Allow from 127.0.0.1
#     Allow from ::1
#   </IfModule>
#</Directory>

```

Open the “config.inc.php” file and change from “cookie” to “http” to change the authentication in phpMyAdmin.


```
# cp /usr/share/phpMyAdmin/config.sample.inc.php /usr/share/phpMyAdmin/config.inc.php 

# vim /usr/share/phpMyAdmin/config.inc.php 

```

Change the Cookie to httpd in this code 


```

/* Authentication type */
$cfg['Servers'][$i]['auth_type'] = 'http';

```


Restart the Apache service.


```

# /etc/init.d/httpd restart

```

Now you can access the phpmyadmin console using http://192.168.1.200/phpmyadmin/


Enter your MySQL username and password which you have given in previous steps. In my case its “root” and “admin123$”.


Now you will get the phpmyadmin page as shown below. 


Thats it. Now the LAMP server is up and running.


And install the following PHP modules too..


```

# yum install php-mcrypt php-pdo php-cli php-mysql php-gd php-xml libtool-ltdl mhash mcrypt -y

```

Create Database and User for Ampache


Here i am going to create a database called videodb and database user called root with password admin123$ . 



```

# mysql -u root -p
# create database videodb;
# GRANT ALL ON videodb.* TO root@localhost IDENTIFIED BY 'admin123$'; 
# flush privileges;
# \q

```


Downloading Ampache



Download the latest version of Ampache enter the following command to download it from your terminal.


```

# wget https://codeload.github.com/ampache/ampache/tar.gz/3.6-alpha6

```

Extract the downloaded file using command:



```

# tar -zxvf 3.6-alpha6

```


Move the contents to apache web root.


```

# mv ampache-3.6-alpha6/ /var/www/html/ampache

```


Set the write permissions to ampache/config directory.


```

# chmod -R 777 /var/www/html/ampache/config/


```

Begin Installation


Open up browser and type http://192.168.1.200/ampache in the address bar.
The following should appear. Click Start configuration.


Follow the Steps ...


Thats it Start Uploading videos and audio and streaming .

