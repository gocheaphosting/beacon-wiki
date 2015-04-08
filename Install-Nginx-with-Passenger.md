
Note : - ** We need to install passenger 4.0 and currently it is a release candidate 6. **
## install Nginx to work with Passenger

 Since we’ll be using Nginx for serving our application, we’re going to install it using the Passenger installer. Nginx modules need to be compiled into nginx, unlike Apache, so we can’t just install the package from the Ubuntu Package management using apt-get.
Adding our APT repository

1.Install our PGP key. Packages are signed by "Phusion Automated Software Signing (auto-software-signing@phusion.nl)", fingerprint 1637 8A33 A6EF 1676 2922 526E 561F 9B9C AC40 B2F7.
 
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 561F9B9CAC40B2F7

2.Add HTTPS support for APT. Our APT repository is stored on an HTTPS server.

    sudo apt-get install apt-transport-https ca-certificates

3.Create a file /etc/apt/sources.list.d/passenger.list and insert one of the following lines, depending on your distribution.
Open source

##### !!!! Only add ONE of these lines, not all of them based on  your ubuntu version  !!!! #####
    # Ubuntu 14.04
    deb https://oss-binaries.phusionpassenger.com/apt/passenger trusty main
    # Ubuntu 12.04
    deb https://oss-binaries.phusionpassenger.com/apt/passenger precise main
    # Ubuntu 10.04
    deb https://oss-binaries.phusionpassenger.com/apt/passenger lucid main
    # Debian 7
    deb https://oss-binaries.phusionpassenger.com/apt/passenger wheezy main
    # Debian 6
    deb https://oss-binaries.phusionpassenger.com/apt/passenger squeeze main

Secure passenger.list and update your APT cache:

    sudo chown root: /etc/apt/sources.list.d/passenger.list
    sudo chmod 600 /etc/apt/sources.list.d/passenger.list
    sudo apt-get update

## setup a script to control Nginx from Linux service

Get the scripts from Linode and install as init.d command

```
wget -O init-deb.sh http://library.linode.com/assets/660-init-deb.sh
sudo mv init-deb.sh /etc/init.d/nginx
sudo chmod +x /etc/init.d/nginx
sudo /usr/sbin/update-rc.d -f nginx defaults
```

```
sudo apt-get install nginx-extras passenger

```

** now We can control Nginx with this script. To start and stop the server manually, run: **
```
sudo /etc/init.d/nginx stop
sudo /etc/init.d/nginx start
```

## Nginx server configuration

### create Nginx server folders 

```
sysadmin@appserver:/var/canvas$ cd /opt/nginx/conf
sysadmin@appserver:/opt/nginx/conf$ sudo mkdir include.d
sysadmin@appserver:/opt/nginx$ sudo mkdir ssl
sysadmin@appserver:/opt/nginx$ sudo mkdir sites-available
sysadmin@appserver:/opt/nginx$ sudo mkdir sites-enabled
```

### Edit the nginx.conf

Edit the file `/opt/nginx/conf/nginx.conf` and repalace all the lines with the following lines. 
We can change the worker_processes to 4 in production

```
user www-data;
worker_processes  1;

error_log  /var/log/nginx/error.log;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {

    include       mime.types;
    default_type  application/octet-stream;

    access_log  /var/log/nginx/access.log;

    sendfile  on;
    tcp_nopush on;
    tcp_nodelay on;

    keepalive_timeout  65;

    gzip  on;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_proxied any;
    gzip_vary off;
    gzip_types text/plain text/css application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript application/json;
    gzip_min_length  1000;
    gzip_disable     "MSIE [1-6]\.";

    server_names_hash_bucket_size 64;
    types_hash_max_size 2048;
    types_hash_bucket_size 64;

    include /opt/nginx/conf/include.d/*.conf;
    include /opt/nginx/sites-enabled/*;
}
```

### Edit the passenger.conf

```
   sysadmin@appserver:/var/canvas$ cd /opt/nginx/include.d
   sysadmin@appserver:/opt/nginx/include.d$ vi passenger.conf    
```

- Add the following passenger config entries to the passenger.conf

```
passenger_root /usr/local/lib/ruby/gems/1.9.1/gems/passenger-4.0.0.rc6;
passenger_ruby /usr/local/bin/ruby;
passenger_max_pool_size 6;
passenger_spawn_method smart-lv2;
passenger_buffer_response on;
passenger_min_instances 1;
passenger_max_instances_per_app 0;
passenger_pool_idle_time 300;
passenger_max_requests 0;
```

## Verifying that Passenger is running

```
passenger-memory-stats
```


## passenger tools
* passenger-config            
* passenger-memory-stats
* passenger-install-apache2-module  
* passenger-status
* passenger-install-nginx-module 

# how to check passenger install dir 

    passenger-config --root


## Nginx Disabling passenger without uninstalling

To temporarily unload (disable)  Passenger from the web server, without uninstalling the Passenger files
Connect out the following three lines in the passenger.conf file

```
# passenger_root /somewhere/passenger-x.x.x;
# passenger_ruby /usr/bin/ruby;
# passenger_max_pool_size 10;
```