
## Create Rails staging Env

- Add a new environment in conf/environments/
- Add a new database configuration (optional)

 The new stating environment should match the production environment we can start by copying the production environment:

```
cp config/environments/production.rb config/environments/staging.rb
```

The staging environment to have full stack traces. Edit ` config/environments/staging.rb ` and change the following:

` config.action_controller.consider_all_requests_local = false `

to

` config.action_controller.consider_all_requests_local = true `

If you want a separate database for the staging area, Edit config/database.yml to set one up, just like the others Env. 

If you don't need a separate database for your staging environment, you can just add following line to the config/database.yml :

staging:
  production

now verify that your new environment is working by starting the console:

` script/console staging `

### Passenger setup for Staging

Change the rails_env to Staging

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
               **rails_env staging;**
               ssl on;
               ssl_certificate /opt/nginx/ssl/beacon_cert_nginx.crt;
               ssl_certificate_key /opt/nginx/ssl/beacon_cert_nginx.key;
               ssl_session_timeout  5m;
        }
```