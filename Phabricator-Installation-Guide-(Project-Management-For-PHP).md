Phabricator Installation Guide

This document contains basic install instructions to get Phabricator up and running.

Installation Requirements

Phabricator is a LAMP application suite, so you basically need LAMP:

Linux: Some flavor of Linux is required. Mac OS X is an acceptable flavor of Linux. Windows is not an acceptable flavor of Linux. Phabricator will not install or work properly on Windows. (If you want it to, send patches.) Phabricator has active contributors running it on Mac OS X, Amazon Linux, Ubuntu, RHEL and CentOS; if you run into issues on other flavors, send patches or complaints.

Apache (or nginx, or lighttpd): You need Apache 2.2.7+ (or another tested webserver). You can probably use something else, but you're on your own.

MySQL: You need MySQL.
PHP: You need PHP 5.2 or newer.


You'll probably also need a domain name and you'll certainly need a computer with a connection to the internet.



```
Here im Using IP address : 192.168.1.8 (In office server) 
Domain hostname as       : phabricator.example.com
Distribution 		 : Ubuntu 12.04 Ltd Server Edition
Database Username (mysql): root
Database Password	 : admin123$

```

If you are installing on Ubuntu or an RedHat derivative, there are install scripts available which should handle most of the things discussed in this document for you:


Here we have to setup a LAMP server For Setting up a LAMP Server Follow the Below Link if u planning to set a LAMP Manually.




If U want to Automate Everythink Using a Script in Phabricator Installation
Download the Script and Run it ..



```

# wget http://www.phabricator.com/rsrc/install/install_ubuntu.sh

```


After Downloading it Move it to Directory www


```

# mv install_ubuntu.sh /var/www/

```


Then we need to change it to executable file using 


```

# chmod 777 install_ubuntu.sh

```

Then Execute the File using by in Current Directory where the file have been saved under www


```
# pwd

  /var/www/

# ./install_ubuntu.sh

```


This Will Install PHP and APC, git, Mysql, Apache

Must want to be Execute inside the www Directory , Unfortunately if we installing from the /~ Directory , it will get install under /root/Phabricator/ if so we can access the it in browser cos its in / .


Edit the Apache site-available 


```

# vim /etc/apache2/sites-enabled/000-default

```

And change the Configuration as Below 

This is Manual Configuration we need to Do


```

<VirtualHost *>
  # Change this to the domain which points to your host.
  ServerName phabricator.example.com

  # Change this to the path where you put 'phabricator' when you checked it
  # out from GitHub when following the Installation Guide.
  #
  # Make sure you include "/webroot" at the end!
  DocumentRoot /path/to/phabricator/webroot

  RewriteEngine on
  RewriteRule ^/rsrc/(.*)     -                       [L,QSA]
  RewriteRule ^/favicon.ico   -                       [L,QSA]
  RewriteRule ^(.*)$          /index.php?__path__=$1  [B,L,QSA]
</VirtualHost>

```

If Apache isn't currently configured to serve documents out of the directory where you put Phabricator, you may also need to add a section like this:


```

<Directory "/path/to/phabricator/webroot">
  Order allow,deny
  Allow from all
</Directory>

```


After making your edits, restart Apache


Note :> The Above 2 Configartion Done and How its Looks like is Below 


```

1 <VirtualHost *:80>
  2         ServerAdmin webmaster@localhost
  3         ServerName phabricator.example.com
  4         DocumentRoot /var/www/phabricator/webroot
  5         RewriteEngine on
  6         RewriteRule ^/rsrc/(.*)     -                       [L,QSA]
  7         RewriteRule ^/favicon.ico   -                       [L,QSA]
  8         RewriteRule ^(.*)$          /index.php?__path__=$1  [B,L,QSA]
  9         <Directory "/var/www/phabricator/webroot">
 10         Order allow,deny
 11         Allow from all
 12         </Directory>
 13         <Directory />

```

 
Just we need only the first 13 lines to get changed , I have omitted the Entire Configuration file and only our needed configuration is Here . 



Configure web port


```
# vim /etc/ apache2/ports.conf

```

Add below lines

```

NameVirtualHost *:80

Listen 80

```

If its not there add this , If not leave as it is .



Enable mod_rewrite & restart apache2 (if u used to setup Lamp server using Script we dont need to do this step)
 

```

# a2enmod rewrite

```

Restart the Apache to take effect 


```

# /etc/init.d/apache2 restart


```


Configure mysql


```

./bin/storage upgrade --user root --password admin123$

./bin/config set mysql.user root

./bin/config set mysql.pass admin123$

```


Restart the Mysql to take effect 


```

# /etc/init.d/mysqld restart

```


Now, We can access http://192.168.1.8/ success. If you can’t see the login window, you my set up your environment correctly according Phabricator’s indicate.


## Login Phabricator



### Create new account


```

# ./bin/accountadmin


```


Then create your admin account step by step.

A question: what is system agent? We can’t send mail to system agent.

Should this user be a system agent? [y/N] y


### Login Phabricator


On top-left corner, phabricator will tell you how many setup issues


### Configure Server Timezone


Click “Current Settings” , and find “phabricator.timezone” Key, Click it

Input your timezone value, then “Save Config Entry”.


### Configure Base URI

This can’t modify through web, so have to use cmdline


```
# ./bin/config set phabricator.base-uri 'http://192.168.1.8/'

```

### Configure Email

Find “Mail” item, then click to enter

Back to previous page, find “PHPMailer” item, click to enter


### Install require extension


```

# dpkg -i libgd2-xpm_2.0.36~rc1~dfsg-3.1ubuntu1_amd64.deb

```


If you Face Error 

### Solution 


```
dpkg -r --force-depends libgd2-noxpm

dpkg -i libgd2-xpm_2.0.36~rc1~dfsg-3.1ubuntu1_amd64.deb

finally, install php5-gd,

dpkg -i php5-gd_5.3.2-1ubuntu4.18_amd64.deb

```


### Create normal user account


Login as admin, click top-left corner icon
Scoll down the page, then click “People”
You can “Create New User” now.



### Test email sent

Enter MetaMTA function

If you see the “Status” is “Sent”, that means email sent success.  Otherwise the status is “Void”, you can click right “view” button to find detail info.