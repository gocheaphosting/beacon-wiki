## Install Passenger via gem

Note : - ** We need to install passenger 4.0 and currently it is a release candidate 6. **

To install the pre-release passenger, run the command 

```
gem install passenger --pre
```

-Once this gem is released fully , we have to run the following command

```
gem install passenger 
```

- Now install Nginx to work with Passenger

** Since we’ll be using Nginx for serving our application, we’re going to install it using the Passenger installer. Nginx modules need to be compiled into nginx, unlike Apache, so we can’t just install the package from the Ubuntu Package management using apt-get. **

- Run the command 

```
passenger-install-nginx-module
```

- Choose "download, compile, and install Nginx for me"
- Accept defaults for any other questions it asks


## setup a script to control Nginx from Linux service

Get the scripts from Linode and install as init.d command

```
wget -O init-deb.sh http://library.linode.com/assets/660-init-deb.sh
sudo mv init-deb.sh /etc/init.d/nginx
sudo chmod +x /etc/init.d/nginx
sudo /usr/sbin/update-rc.d -f nginx defaults
```

** now We can control Nginx with this script. To start and stop the server manually, run: **
```
sudo /etc/init.d/nginx stop
sudo /etc/init.d/nginx start
```

## Nginx server configuration

create Nginx server folders 

```
sysadmin@appserver:/opt/nginx$ sudo mkdir include.d
sysadmin@appserver:/var/canvas$ cd /opt/nginx
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


## Nginx configuration

### Disabling passenger without uninstalling

To temporarily unload (disable)  Passenger from the web server, without uninstalling the Passenger files
Connect out the following three lines in the 

# passenger_root /somewhere/passenger-x.x.x;
# passenger_ruby /usr/bin/ruby;
# passenger_max_pool_size 10;
## passenger tools