# get beacon code

```
sudo mkdir -p /var/beacon
sudo chown -R sysadmin /var/beacon
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
sysadmin@appserver:/var/beacon$bundle exec rake db:migrate
sysadmin@appserver:/var/beacon$bundle exec rake db:populate
sysadmin@appserver:/var/beacon$bundle exec rake db:seed
```

# Assets pre-complie

```
sysadmin@appserver:/var/beacon$bundle exec rake assets:precompile
```



