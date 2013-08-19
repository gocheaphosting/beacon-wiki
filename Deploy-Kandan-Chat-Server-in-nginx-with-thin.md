# get Kandan code

```
sysadmin@appserver:~$ cd
clone the repository in sysadmin home directroy
sysadmin@appserver:~$ git clone -b deploy https://github.com/m-narayan/kandan.git
sysadmin@appserver:~$ sudo mkdir -p /var/deploy/kandan/kandan-<datestamp>
sysadmin@appserver:~$ cd kandan
sysadmin@appserver:kandan$ cp -av * /var/deploy/kandan/kandan-<datestamp>
sysadmin@appserver:kandan$ cd /var/deploy/kandan/kandan-<datestamp>
```

# Bundle and kandan dependencies

```
sysadmin@appserver:/var/deploy/kandan/kandan-<datestamp>$ bundle install --deployment --without=sqlite
```

## Create user and databases for kandan

```
psql -U postgres
create user kandan password 'kandan';
CREATE DATABASE kandan_production ENCODING 'UTF8' OWNER kandan;
GRANT ALL PRIVILEGES ON DATABASE kandan_production to kandan;
\q
```

#Database configuration

```
sysadmin@appserver:/var/deploy/kandan$ vi config/database.yml
```
Update this section to reflect your Postgres server's location and authentication credentials

# Database population

```
sysadmin@appserver:/var/deploy/kandan$RAILS_ENV=production bundle exec rake db:create 
sysadmin@appserver:/var/deploy/kandan$RAILS_ENV=production bundle exec rake db:migrate 
sysadmin@appserver:/var/deploy/kandan$RAILS_ENV=production bundle exec rake kandan:bootstrap
```

# Assets pre-complie

```
sysadmin@appserver:/var/deploy/kandan$bundle exec rake assets:precompile
```
# Install Thin server
```
sysadmin@appserver:$sudo gem install thin
sysadmin@appserver:$thin -v
sysadmin@appserver:$sudo thin install
sysadmin@appserver:$sudo /usr/sbin/update-rc.d -f thin defaults
sysadmin@appserver:$sudo thin config -C /etc/thin/kandan.yml -c /var/deploy/kandan/  --servers 3 -e production
sysadmin@appserver:$ cat /etc/thin/kandan.yml

```
# Create current symlink for kandan
```
sysadmin@appserver:/var/kandan$ ln -s /var/deploy/kandan/kandan-<datestamp> /var/deploy/kandan/current

```
# configure nginx and thin for kandan
```
sysadmin@appserver:/var/kandan$ cd /opt/nginx/sites-available
sysadmin@appserver:/opt/nginx/sites-available$ sudo vi kandan 
```

add the following line to the file to redirect plain http request url to secure https url

```
upstream chat.arrivuapps{
  server 127.0.0.1:3000;
  server 127.0.0.1:3001;
  server 127.0.0.1:3002;
}
server {
  listen   80;
  server_name chat.arrivuapps.com;

            access_log /var/log/nginx/kandan.access.log;
            error_log  /var/log/nginx/kandan.error.log;
            root /var/deploy/kandan/current/public;


  location / {
    proxy_set_header  X-Real-IP  $remote_addr;
    proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header  Host $http_host;
    proxy_redirect  off;
    try_files /system/maintenance.html $uri $uri/index.html $uri.html @ruby;
  }

  location @ruby {
    proxy_pass http://chat.arrivuapps;
  }
}
```

# Enable the Kandan site in Nginx

```
sysadmin@appserver:/opt/nginx$ sudo ln -s /opt/nginx/sites-available/kandan /opt/nginx/sites-enables/kandan
```

# reload the nginx server configuration

```
sysadmin@appserver:/opt/nginx$ sudo service nginx reload 
```