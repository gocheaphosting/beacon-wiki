ICTFAX Step by Step Installation 


Here My Server IP is 192.168.1.125

ICTFAX is a Open Source Multiuser and web based Software solution for service providers 


ICTFAX - Installation Guide - CentOS

ICT-FAX is a unique and complete faxing solution with billing featuring T.38 support, Email2Fax, Web2Fax, Fax2Email and Billing.


```

# cd /usr/src


```

Download the ICTFAX from Below Link 


```
# wget http://downloads.sourceforge.net/project/ictfax/ictfax-2.1.1.zip

```

Extract it using Unzip command 


```
# unzip ictfax-2.1.1.zip

```

Install Basic System Requirements


```
1. CentOs 6
2. Apache 2
3. MySQL 5
4. PHP 5.3.3
5. php-mysql
6. php-gd
7. php-curl
8. php-imap
9. perl
10. perl-DBD-mysql
11. libtiff
12. ghostscript
13. ImageMagick
14. poppler-utils
15. curl
16. mysql-devel
17. sendmail
```

To install above requirements issue following commands at shell prompt


```
# yum -y install httpd mysql-server mysql mysql-devel
# yum -y install php php-common php-cli php-gd php-imap php-curl php-mbstring php-xml php-mysql
# yum -y install perl perl-DBD-mysql
# yum -y install ghostscript ImageMagick poppler-utils curl sendmail sendmail-cf
```

following dependencies are required for Freeswitch installations

```
# yum -y install wget git make gcc-c++ libevent-devel kernel-devel

```

Then install yudit for text to pdf support


Navigate to Directory 

```
# cd /usr/src

```

Then Download the yudit from Below Link 


```
# wget "http://www.yudit.org/download/yudit-2.9.2.tar.gz"

```
Extract it using  tar command 

```
# tar xzf yudit-2.9.2.tar.gz

```

Navigate to extraced Directory 


```
cd yudit*

```

Install it using configure and Install 


```
# ./configure --prefix=/usr/local
# make
# make install

```


before continue, make sure that MySQL and Apache is running on centos command prompt:


```
# chkconfig httpd on
# chkconfig mysqld on
# service httpd start
# service mysqld start
```


3: Freeswitch Installation


ICTFax is based on Plivo Framework.
So you need to setup and run freeswitch provided by Plivo.org.
Instructions on how to install Freeswitch are given
at http://www.plivo.org/get-started/ and repeated here for your convenience:

* Download and run the FreeSWITCH installer on your system
Note: Currently, this installer is tested on CentOS > 5.5 and Debian-based distros.


```
# wget --no-check-certificate https://github.com/plivo/plivoframework/raw/master/freeswitch/install.sh

```

Edit a new file and save the script in it

Then Run the script using


```
# bash install.sh


```


* Run FreeSWITCH


Run in Foreground

```
# /usr/local/freeswitch/bin/freeswitch

```

Run in Background

```
# /usr/local/freeswitch/bin/freeswitch -nc

```


Setup and Run Plivo Framework

Download ictfax from sourceforge or git. Then extract the downloaded folder and locate the folder "plivo-devel" in the extracted ICTFax directory.

1. Stop plivo service (if any) and clear /usr/local/plivo

2. Copy and Paste plivo-devel folder in /usr/

3. Go to /usr/plivo-devel and Run plivo_install.sh using following command:

```
#./plivo_install.sh /usr/local/plivo

```

5. Go to /usr/local/plivo/bin directory and Run plivo service using the following command:

```
#./plivo start

```

NOTE: There may be some errors while starting plivo cache server. But make sure that plivo default server is running.


Plivo Configurations:

1. Go to /usr/local/plivo/etc/plivo/default.conf
2. Enable EXTRA_FS_VARS by removing # before it.
3. Set variable in plivo config as EXTRA_FS_VARS = variable_duration
4. Set Incoming DEFAULT_ANSWER_URL, DEFAULT_HANGUP_URL

```
DEFAULT_ANSWER_URL = http://192.168.1.125/ictfax/index.php?q=ictfax/receive_fax
DEFAULT_HANGUP_URL = http://192.168.1.125/ictfax/index.php?q=ictfax/receive_fax_billing

```

Modify above urls according to your installation settings.
Don't forget to remove “#” sign before DEFAULT_HANGUP_URL and EXTRA_FS_VARS.


4: ICTFAX Installation
4.1: Database

* Database Installation:
Create "ictfax" database in mysql (Run 'CREATE DATABASE ictfax' query on mysql)


```
CREATE DATABASE ictfax;

```

4.2: Frontend / Web GUI

Download ictfax from sourceforge or git. Then extract the downloaded folder and locate the folder "wwwroot" in the extracted ICTFAX directory.
Rename this folder to ictfax and copy-paste it to /usr directory.

1. Create a symbolic link for /usr/ictfax in /var/www/html


```

# ln -s /usr/ictfax /var/www/html/ictfax


```


2. Now visit http://192.168.1.125/ictfax and follow the installation
instructions for ICTFax (drupal based) front end installation.

3. Once you are done with installation, visit the website and login as site administrator with username and password that you provided during installation.

4. Locate the folder "ictpbx" in the extracted ICTFAX directory. Copy it and move it to /usr/ictfax/sites/all/modules.

5. Now comeback to Web GUI and go to Modules menu and enable all modules in "ICTPBX System" Package.

6. Now you'll see menu item Fax Account, ICTPBX System and others in your Navigation Menu.


Note:Following contributed modules should be enabled.
1.Chaos Tools
2.Features
3.Feeds
4.Countries
5.Mailhandler (All are required)
6.Job Scheduler
7.Strongarm
8.Views


5: Email to FAX / FAX to Email service (optional)
1. Make sure that your desired domain's MX records are properly configured for email2fax server.
2. install sendmail service and enable sendmail service at startup.
2a. Also make sure you have created linux user "freeswitch".
3. enable sendmail to listen on public ip address look for following line in /etc/mail/sendmail.mc


```

DAEMON_OPTIONS(`Port=smtp,Addr=127.0.0.1, Name=MTA')dnl

```

4. and change line mentioned above into


```
DAEMON_OPTIONS(`Port=smtp, Addr=0.0.0.0, Name=MTA')dnl

```

5. apply changes


```
m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf

```

6. Add freeswitch to list of trusted user


```

echo "freeswitch" >> /etc/mail/trusted-users

```

7. Add your domain name in allowed local domain list to let sendmail receive mails for that domain


```

echo "192.168.1.125" >> /etc/mail/local-host-names

```

8. route all mails for none-existing addresses into freeswitch mailbox so we can receive emails for addresses like xyz_number@192.168.1.125


```
echo '@192.168.1.125 freeswitch' >> /etc/mail/virtusertable
makemap hash /etc/mail/virtusertable < /etc/mail/virtusertable

```


9. grant proper permission to apache user on mail folder



```
chmod +t /var/spool/mail

```


10. restart sendmail service so changes can take affect


```
service sendmail restart


```


11. login at ictfax web interface as admin (ictfax)


12. goto administrator => mailhandler => Add Mailbox and set following fields

E-mail address: fax@192.168.1.125
Folder: /var/spool/mail/freeswitch
POP3 or IMAP Mailbox: local Mbox
Mailbox domain: *** must be empty ***
Security: Require password (leave empty if you haven't set already)
Delete messages after they are processed?: TICK / Yes

13. setup cronjob so incoming email can be processed after every 5 minutes



```
echo 'MAILTO=""' > /tmp/freeswitch_cron.txt
echo "*/5 * * * * wget -O /dev/null 'http://192.168.1.125/ictfax/cron.php?cron_key=oxFMExanuyoxI4R3SvMYUb9zsEiLge0yGU6zYLXPOFE' 2>/dev/null" >> /tmp/freeswitch_cron.txt
crontab -l >> /tmp/freeswitch_cron.txt
crontab /tmp/freeswitch_cron.txt

```


14. You can find your cron url by logging in at your web interface as admin. Go to Reports -> Status Reports.
Copy your Cron URL and paste at the above URL and then run above lines at the command prompt.


NOTE: make sure that /etc/hosts.allow is properly configured for accepting mails, and
smtp port (25) is not blocked by firewall. if so Add following line to
/etc/sysconfig/iptables above the last reject/drop rule:


-A INPUT -m state --state NEW -m tcp -p tcp --dport 25 -j ACCEPT


Also DO NOT enable CLEAN URLS, because plivo has been configured to use default URLS.



15. Create a content type "fax" with three additional fields "to" of type text, "from" of type text" and "file" of type file.


Go to Admin => Structure => Feeds Importer => Mailhandler nodes. Click Override and then in Processor field make sure Fax Processor is selected. Click Mapping in fax processor. Make sure that your to, from and file fields are correctly mapped to toaddress, fromaddress and attachments respectively.


16. Create a new MailHandler source . Go to Admin => Content => Add new => Mailhandler Source .Insert any name and select the mailbox you created in step 12 and save it.


17. Now you are ready to send faxes through your email. See Admin/User Guide for further details.


Done ...