## Installing Postgres
Follow [Postgresql install ](https://github.com/m-narayan/beacon/wiki/Install-Postgresql) install to make sure that the database is ready

## Create user and databases for canvas

```
psql -U postgres
create user canvas password 'canvas';
CREATE DATABASE canvas_production ENCODING 'UTF8' OWNER canvas;
CREATE DATABASE canvas_queue_production ENCODING 'UTF8' OWNER canvas;
GRANT ALL PRIVILEGES ON DATABASE canvas_production to canvas;
GRANT ALL PRIVILEGES ON DATABASE canvas_queue_production to canvas;
\q
```

## Install Ruby and Nginx with Passenger  

Follow [Nginx-with-Passenger install ](https://github.com/m-narayan/beacon/wiki/Install-Nginx-with-Passenger) install to configure the server

## Code installation

We need to put the Canvas code in the location where it will run from. 

    sysadmin@appserver:~$ sudo mkdir -p /var/canvas
    sysadmin@appserver:~$ sudo chown -R sysadmin /var/canvas
    sysadmin@appserver:~$ cd canvas
    sysadmin@appserver:~/canvas$ ls
    app     db   Gemfile  log     Rakefile  spec  tmp
    config  doc  lib      public  script    test  vendor
    sysadmin@appserver:~/canvas$ cp -av * /var/canvas
    sysadmin@appserver:~/canvas$ cd /var/canvas
    sysadmin@appserver:/var/canvas$ ls
    app     db   Gemfile  log     Rakefile  spec  tmp
    config  doc  lib      public  script    test  vendor
    sysadmin@appserver:/var/canvas$


## Bundler and Canvas dependencies

    sysadmin@appserver:/var/canvas$ sudo gem install bundler
    sysadmin@appserver:/var/canvas$ bundle install --path vendor/bundle --without=sqlite

## Canvas default configuration

    sysadmin@appserver:/var/canvas$ for config in amazon_s3 database \
      delayed_jobs domain file_store outgoing_mail security external_migration
    do cp config/$config.yml.example config/$config.yml; done

## Database configuration

  
        sysadmin@appserver:/var/canvas$ vi config/database.yml
  

Update this section to reflect your Postgres server's location and authentication credentials. 

## Outgoing mail configuration

   
    sysadmin@appserver:/var/canvas$ nano config/outgoing_mail.yml
    

Find the **production** section and configure it to match your SMTP provider's settings. Note that the *domain* and *outgoing_address* fields are not for SMTP, but are for Canvas. *domain* is required, and is the domain name that outgoing emails are expected to come from. *outgoing_address* is optional, and if provided, will show up as the address in the *From* field of emails Canvas sends.

## URL configuration

In many notification emails, and other events that aren't triggered by a web request, Canvas needs to know the URL that it is visible from. For now, these are all constructed based off a domain name. Please edit the **production** section of *config/domain.yml* to be the appropriate domain name for your Canvas installation. For the *domain* field, this will be the part between `http://` and the next `/`. Instructure uses *canvas.instructure.com*.

   
    sysadmin@appserver:/var/canvas$ nano config/domain.yml
    

## Database population

   
    sysadmin@appserver:/var/canvas$ RAILS_ENV=production bundle exec rake db:initial_setup
   

## File Generation

   
    sysadmin@appserver:/var/canvas$ bundle exec rake canvas:compile_assets
   

## Canvas ownership

### Making sure Canvas can't write to more things than it should

   
    sysadmin@appserver:~$ cd /var/canvas
    sysadmin@appserver:/var/canvas$ sudo adduser --disabled-password --gecos canvas canvasuser
    sysadmin@appserver:/var/canvas$ sudo mkdir -p log tmp/pids public/assets public/stylesheets/compiled
    sysadmin@appserver:/var/canvas$ sudo touch Gemfile.lock
    sysadmin@appserver:/var/canvas$ sudo chown -R canvasuser config/environment.rb log tmp public/assets \
                                      public/stylesheets/compiled Gemfile.lock config.ru

  

### Making sure other users can't read private Canvas files

There are a number of files in your configuration directory (`/var/canvas/config`) that contain passwords, encryption keys, and other private data that would compromise the security of your Canvas installation if it became public. These are the *.yml* files inside the *config* directory, and we want to make them readable only by the *canvasuser* user.

```
sysadmin@appserver:/var/canvas$ sudo chown canvasuser config/*.yml
sysadmin@appserver:/var/canvas$ sudo chmod 400 config/*.yml
```

# configure nginx and passenger for canvas
```
sysadmin@appserver:/var/beacon$ cd /opt/nginx/sites-available
sysadmin@appserver:/opt/nginx/sites-available$ sudo vi canvas 
```

add the following line to the file to redirect plain http request url to secure https url

```
server {
    listen      80;
    server_name lms.arrivu.corecloud.com;
    # rewrite     ^   https://$server_name$request_uri? permanent;
    return 301 https://lms.arrivu.corecloud.com$request_uri;
}
```

add the following lines to create the https site configurations

```
server  {
               listen 443;
               server_name lms.arrivu.corecloud.com;
               root /var/beacon/public;
                           charset utf-8;
                           include mime.types;
                           default_type application/octet-stream;
               access_log /var/log/nginx/canvas.access.log;
               error_log /var/log/nginx/canvas.error.log;
               passenger_enabled on;
               rails_env production;
               ssl on;
               ssl_certificate /opt/nginx/ssl/canvas_cert_nginx.crt;
               ssl_certificate_key /opt/nginx/ssl/canvas_cert_nginx.key;
               ssl_session_timeout  5m;
        }

```

# Configure ssl for nginx

create the server private key, you'll be asked for a passphrase: enter the passphrase as 'arrivu'

```
sysadmin@appserver:/opt/nginx/sites-available$ cd /opt/nginx/ssl
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl genrsa -des3 -out canvas_cert_nginx.key 1024
```

Create the Certificate Signing Request (CSR):

```
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl req -new -key canvas_cert_nginx.key -out beacon_cert_nginx.csr
```

This will as lot of questions and enter the server name as ** lms.arrivu.corecloud.com **

Remove the necessity of entering a passphrase for starting up nginx with SSL using the above private key:

```
sysadmin@appserver:/opt/nginx/ssl$ sudo cp beacon_cert_nginx.key canvas_cert_nginx.key.org
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl rsa -in canvas_cert_nginx.key.org -out beacon_cert_nginx.key
```

Finally sign the certificate using the above private key and CSR:

```
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl x509 -req -days 365 -in canvas_cert_nginx.csr -signkey beacon_cert_nginx.key -out beacon_cert_nginx.crt
```
Update Nginx configuration by including the newly signed certificate and private key:

```
sysadmin@appserver:/opt/nginx/ssl$ cd /opt/nginx/sites-available
sysadmin@appserver:/opt/nginx/sites-available$ sudo vi canvas
```
make sure the following lines are in the /opt/nginx/sites-available/beacon file ssl server configuration

```
ssl_certificate /opt/nginx/ssl/canvas_cert_nginx.crt;
ssl_certificate_key /opt/nginx/ssl/canvas_cert_nginx.key;
```
 
# Enable the beacon site in Nginx

```
sysadmin@appserver:/opt/nginx$ sudo ln -s /opt/nginx/sites-available/canvas /opt/nginx/sites-enables/canvas
```

# reload the nginx server configuration

```
sysadmin@appserver:/opt/nginx$ sudo service nginx reload 
```

## Cache configuration