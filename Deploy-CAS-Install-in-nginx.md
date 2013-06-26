# Install sqlite dependency

We need to install sql-lite to make the bundle install work. There may be some other library has the dependency on sql-lite and the bundler option --without=sqllite also won't work!!!.

```
sudo apt-get install libsqlite3-dev
```
# get cas code

```
sysadmin@appserver:~$ sudo mkdir -p /var/cas
sysadmin@appserver:~$ sudo chown -R sysadmin /var/cas
sysadmin@appserver:~$ git clone https://github.com/m-narayan/rubycas-server.git
sysadmin@appserver:~$ cd rubycas-server
sysadmin@appserver:~/projects/cas$ cp -av * /var/cas
sysadmin@appserver:~/projects/cas$ cd /var/cas
```

# Bundle and cas dependencies

```
sysadmin@appserver:/var/cas$ bundle install --deployment --without=sqlite
```

## Create user and databases for casserver

```
psql -U postgres
create user rubycas password 'rubycas';
CREATE DATABASE casserver_production ENCODING 'UTF8' OWNER rubycas;
GRANT ALL PRIVILEGES ON DATABASE casserver_production to rubycas;
\q
```

## Grant rubycas db user the privilage to select on beacon_production.users table
```
psql -U postgres -d beacon_production
GRANT SELECT ON users TO rubycas;
\q
```
#rubycas configuration

```
sysadmin@appserver:/var/cas$ vi config.yml
```

Add the following lines and update this section to reflect your Postgres server's location and authentication credentials

```yaml

uri_path: /cas

database:
  adapter: postgresql
  database: casserver_production
  username: rubycas
  password: xxxxxxx
  host: xxx.xxx.xxx.xxx
  reconnect: true

authenticator:
  - class: CASServer::Authenticators::SQL
    database:
      adapter: postgresql     
      database: beacon_production
      username: rubycas
      password: xxxxxxx
      host: xxx.xxx.xxx.xxx
    user_table: users
    username_column: email
    password_column: encrypted_password

# By default, we use the 'simple' theme which you can find in themes/simple.
theme: simple

# The name of your company/organization. This will show up on the login page.
organization: ArrivuSystems

log:
  file: log/casserver.log
  level: DEBUG

db_log:
  file: log/casserver_db.log
  level: DEBUG

downcase_username: true

enable_single_sign_out: true
```

# configure nginx and passenger for cas
```
sysadmin@appserver:/var/cas$ cd /opt/nginx/sites-available
sysadmin@appserver:/opt/nginx/sites-available$ sudo vi cas 
```

add the following line to the file to redirect plain http request url to secure https url

```
server {
    listen      80;
    server_name cas.arrivu.corecloud.com;
    # rewrite     ^   https://$server_name$request_uri? permanent;
    return 301 https://cas.arrivu.corecloud.com$request_uri;
}
```

add the following lines to create the https site configurations

```
server  {
               listen 443;
               server_name cas.arrivu.corecloud.com;
               root /var/cas/public;
                           charset utf-8;
                           include mime.types;
                           default_type application/octet-stream;
               access_log /var/log/nginx/cas.access.log;
               error_log /var/log/nginx/cas.error.log;
               passenger_enabled on;
               rails_env production;
               ssl on;
               ssl_certificate /opt/nginx/ssl/cas_cert_nginx.crt;
               ssl_certificate_key /opt/nginx/ssl/cas_cert_nginx.key;
               ssl_session_timeout  5m;
        }

```

# Configure ssl for nginx

create the server private key, you'll be asked for a passphrase: enter the passphrase as 'arrivu'

```
sysadmin@appserver:/opt/nginx/sites-available$ cd /opt/nginx/ssl
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl genrsa -des3 -out cas_cert_nginx.key 1024
```

Create the Certificate Signing Request (CSR):

```
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl req -new -key cas_cert_nginx.key -out cas_cert_nginx.csr
```

This will as lot of questions and enter the server name as ** cas.arrivu.corecloud.com **

Remove the necessity of entering a passphrase for starting up nginx with SSL using the above private key:

```
sysadmin@appserver:/opt/nginx/ssl$ sudo cp cas_cert_nginx.key cas_cert_nginx.key.org
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl rsa -in cas_cert_nginx.key.org -out cas_cert_nginx.key
```

Finally sign the certificate using the above private key and CSR:

```
sysadmin@appserver:/opt/nginx/ssl$ sudo openssl x509 -req -days 365 -in cas_cert_nginx.csr -signkey cas_cert_nginx.key -out cas_cert_nginx.crt
```
Update Nginx configuration by including the newly signed certificate and private key:

```
sysadmin@appserver:/opt/nginx/ssl$ cd /opt/nginx/sites-available
sysadmin@appserver:/opt/nginx/sites-available$ sudo vi cas
```
make sure the following lines are in the /opt/nginx/sites-available/cas file ssl server configuration

```
ssl_certificate /opt/nginx/ssl/cas_cert_nginx.crt;
ssl_certificate_key /opt/nginx/ssl/cas_cert_nginx.key;
```
 
# Enable the cas site in Nginx

```
sysadmin@appserver:/opt/nginx$ sudo ln -s /opt/nginx/sites-available/cas /opt/nginx/sites-enables/cas
```

# reload the nginx server configuration

```
sysadmin@appserver:/opt/nginx$ sudo service nginx reload 
```