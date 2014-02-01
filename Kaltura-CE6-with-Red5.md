#### Installation Guide (simply follow line by line):
```ruby
    yum update
    init 6
    yum install git wget dos2unix php-cli
    vim /etc/hosts 
    127.0.0.1 media.arrivuapps.com
    ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
    yum install ntp* -y
    ntpdate 0.us.pool.ntp.org 
    vim /etc/ntp.conf 
```
##### Around line 21 add the following lines:
```ruby
   server 0.us.pool.ntp.org
   server 1.us.pool.ntp.org
```
```ruby
   vim /etc/ntp/step-tickers
```
#####At the end of the file, add the following lines:
```ruby
   0.us.pool.ntp.org 
   1.us.pool.ntp.org   
````