# get beacon code

```
sysadmin@appserver:~$ sudo mkdir -p /var/beacon
sysadmin@appserver:~$ sudo chown -R sysadmin /var/beacon
sysadmin@appserver:~$ git clone https://github.com/m-narayan/beacon.git
sysadmin@appserver:~$ cd beacon
sysadmin@appserver:~/projects/beacon$ cp -av * /var/beacon
sysadmin@appserver:~/projects/beacon$ cd /var/canvas
```

# Bundle and beacon dependencies

```
sysadmin@appserver:/var/beacon$ sudo gem install bundler
sysadmin@appserver:/var/beacon$ bundle install --path vendor/bundle --without=sqlite
```

#Database configuration

```
sysadmin@appserver:/var/beacon$ vi config/database.yml
```
Update this section to reflect your Postgres server's location and authentication credentials

# Database population

```
sysadmin@appserver:/var/beacon$ bundle exec rake db:migrate
sysadmin@appserver:/var/beacon$ bundle exec rake db:populate
sysadmin@appserver:/var/beacon$ bundle exec rake db:seed
```

# Assets pre-complie

```
sysadmin@appserver:/var/beacon$bundle exec rake assets:precompile
```

# configure nginx and passenger for beacon
```
sysadmin@appserver:/var/beacon$ cd /opt/nginx/sites-available
sysadmin@appserver:/opt/nginx/sites-available$ vi beacon 
```

add the following line to the file to redirect plain http request url to secure https url

```
server {
    listen      80;
    server_name beacon.arrivu.corecloud.com;
    # rewrite     ^   https://$server_name$request_uri? permanent;
    return 301 https://beacon.arrivu.corecloud.com$request_uri;
}
```

add the following lines to create the https site configurations

```
server  {
               listen 443;
               server_name beacon.arrivu.corecloud.com;
               root /var/beacon/public;
                           charset utf-8;
                           include mime.types;
                           default_type application/octet-stream;
               access_log /var/log/nginx/beacon.access.log;
               error_log /var/log/nginx/beacon.error.log;
               passenger_enabled on;
               rails_env production;
               ssl on;
               ssl_certificate /opt/nginx/ssl/beacon_cert_nginx.crt;
               ssl_certificate_key /opt/nginx/ssl/beacon_cert_nginx.key;
               ssl_session_timeout  5m;
        }

```



# Configure ssl for nginx



