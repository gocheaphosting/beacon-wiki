
Here we can see how to check logs and maintain the server daily 

First of all we need to check the logs daily 

To check the logs we need to login as a root user and check the log, for example im using the ip 192.168.1.100 Here to Describe how to check the logs in VPS.

Note :  If there is a ssh Port change we need to mention the port while we login to the server 
if we need to mention the port use -p (small p not a Capital) And follow by the Port Number 


```

eg : ssh sysdmin@192.168.1.100 -p 2002

```


Provide the password of VPS While asking for the password 
And in Ubuntu server We or in centos Based Server we need to switch the account to the root user to check the log , if not we cant access the Logs , So we need to switch it to the root user by sudo su in ubuntu Linux

Then Navigate to the 

```

cd /var/log/

``` 

And Use the command tail -f to view the current Logs 

```

eg : tail -f /var/log/auth.log

```

If not instead of -f we can use 20 or even 50 that will output us the Last 20 Logs or 50  line of Log file

we need to check the following Logs in Our Server's Regularly

```
tail -f /var/log/auth.log
tail -f /var/log/syslog
tail -f /var/log/fail2ban.log
tail -f /var/log/monit.log
tail -f /var/log/message
``` 

Here i have mentioned the regular path to check the logs , if you are in current directory as /var/log/
you need to check using tail -f message, tail -f auth.log like this .

If u found there is some Authenication failure, send password Wrong mentioned in Auth.log
You Have to Inform ASAP

And check for the service are running in monit log if there is any failure need to infom 
If that service restarted its not a issue 

Check for the syslog , if there is any kernel error or hardware error u will get the error there , And All Cron job Process and logs will be displayed here .

Fail2Ban logs need to check Which ip banned and unbanned , Inform ASAP if there is a IP banned , Some time Our Employes too type the Password Wrong and gets Banned for an Hour , If So Check the IP if the IP starts with 117 will be listed in South india Datacentre IP .


## Check the Monit 


By Placing IP in Browser using 


```

eg : 192.168.1.100:2812


```

Using Login check the processes and RAM usage and Disk space 

The RAM Usage Don't want to Exceed  80% of Usage .
Disk Space too want to be with in 80 % 
if its Exceed more than that Inform ASAP 






