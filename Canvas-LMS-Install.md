## Installing Postgres
Follow [Postgresql ](https://github.com/m-narayan/beacon/wiki/Postgresql-Config) install to make sure that the database is ready

## Install Ruby and Nginx with Passenger  

Follow [Ruby AppSrv ](https://github.com/m-narayan/beacon/wiki/Ruby-AppSrv-config) install to configure the server

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

## Canvas default configuration

    sysadmin@appserver:/var/canvas$ for config in amazon_s3 database \
      delayed_jobs domain file_store outgoing_mail security external_migration
    do cp config/$config.yml.example config/$config.yml; done
