# get beacon code

```
sysadmin@appserver:~$ sudo mkdir -p /var/deploy/beacon
sysadmin@appserver:~$ sudo chown -R sysadmin /var/deploy/beacon
sysadmin@appserver:~$ git clone https://github.com/m-narayan/beacon.git
sysadmin@appserver:~$ cd beacon
sysadmin@appserver:~/projects/beacon$ cp -av * /var/deploy/beacon
sysadmin@appserver:~/projects/beacon$ cd /var/deploy/beacon
```

# Bundle and beacon dependencies

```
sysadmin@appserver:/var/beacon$ bundle install --path vendor/bundle --without=sqlite
```

## Create user and databases for beacon

```
psql -U postgres
create user beacon password 'beacon';
CREATE DATABASE beacon_production ENCODING 'UTF8' OWNER beacon;
GRANT ALL PRIVILEGES ON DATABASE beacon_production to beacon;
\q
```

#Database configuration

```
sysadmin@appserver:/var/beacon$ vi config/database.yml
```
Update this section to reflect your Postgres server's location and authentication credentials

# Database population

```
sysadmin@appserver:/var/beacon$ RAILS_ENV=production bundle exec rake db:migrate
sysadmin@appserver:/var/beacon$ RAILS_ENV=production bundle exec rake db:populate
sysadmin@appserver:/var/beacon$ RAILS_ENV=production bundle exec rake db:seed
```

# Assets pre-complie

```
sysadmin@appserver:/var/beacon$bundle exec rake assets:precompile
```

# 

```
sysadmin@appserver:/var/beacon$  sudo mkdir -p log tmp/pids public/assets public/stylesheets/compiled
sysadmin@appserver:/var/beacon$  sudo chown -R canvasuser config/environment.rb log tmp public/assets \
                                  public/stylesheets/compiled Gemfile.lock config.ru
```

# configure nginx and passenger for beacon
```
sysadmin@appserver:/var/beacon$ cd /opt/nginx/sites-available
sysadmin@appserver:/opt/nginx/sites-available$ sudo vi beacon 
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

create the server private key, you'll be asked for a passphrase: enter the passphrase as 'arrivu'

```
sysadmin@appserver:/opt/nginx/sites-available$ cd /opt/nginx/ssl
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl genrsa -des3 -out beacon_cert_nginx.key 1024
```

Create the Certificate Signing Request (CSR):

```
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl req -new -key beacon_cert_nginx.key -out beacon_cert_nginx.csr
```

This will as lot of questions and enter the server name as ** beacon.arrivu.corecloud.com **

Remove the necessity of entering a passphrase for starting up nginx with SSL using the above private key:

```
sysadmin@appserver:/opt/nginx/ssl$ sudo cp beacon_cert_nginx.key beacon_cert_nginx.key.org
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl rsa -in beacon_cert_nginx.key.org -out beacon_cert_nginx.key
```

Finally sign the certificate using the above private key and CSR:

```
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl x509 -req -days 365 -in beacon_cert_nginx.csr -signkey beacon_cert_nginx.key -out beacon_cert_nginx.crt
```
Update Nginx configuration by including the newly signed certificate and private key:

```
sysadmin@appserver:/opt/nginx/ssl$ cd /opt/nginx/sites-available
sysadmin@appserver:/opt/nginx/sites-available$ sudo vi beacon
```
make sure the following lines are in the /opt/nginx/sites-available/beacon file ssl server configuration

```
ssl_certificate /opt/nginx/ssl/beacon_cert_nginx.crt;
ssl_certificate_key /opt/nginx/ssl/beacon_cert_nginx.key;
```
 
# Enable the beacon site in Nginx

```
sysadmin@appserver:/opt/nginx$ sudo ln -s /opt/nginx/sites-available/beacon /opt/nginx/sites-enables/beacon
```

# reload the nginx server configuration

```
sysadmin@appserver:/opt/nginx$ sudo service nginx reload 
```