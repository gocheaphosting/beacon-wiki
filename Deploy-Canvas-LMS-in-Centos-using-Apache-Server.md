```
# ruby -v

# yum update

# yum install gcc g++ make automake autoconf curl-devel openssl-devel zlib-devel httpd-devel apr-devel apr-util-devel sqlite-devel

# yum groupinstall 'Development Tools'

# yum install -y openssl-devel zlib-devel gcc gcc-c++ curl-devel expat-devel gettext-devel patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel make bzip2 zlib1g

# wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

# rpm -ivh epel-release-6-8.noarch.rpm

# wget http://cache.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p484.tar.gz

# tar xvzf ruby-1.9.3-p484.tar.gz 

# cd ruby-1.9.3-p484

# ./configure 

# make

# make install

# ruby -v

# gem -v

# gem install passenger

# passenger-install-apache2-module

# yum install httpd-devel

# yum install apr-devel

# wget http://pyyaml.org/download/libyaml/yaml-0.1.4.tar.gz

# tar xvf yaml-0.1.4.tar.gz 

# cd yaml-0.1.4

# ./configure

# make

# make install


# rpm -Uvh http://yum.postgresql.org/9.3/redhat/rhel-6-x86_64/pgdg-redhat93-9.3-1.noarch.rpm

# yum install postgresql93-server postgresql93

# /etc/init.d/postgresql-9.3 initdb

```
In some cases above commands doesn't work, Then use following command.

```
# /usr/pgsql-9.3/bin/postgresql93-setup initdb
```

Above command will take some time to initialize PostgreSQL first time. PGDATA environment variable contains path of data directory.

PostgreSQL data directory Path: /var/lib/pgsql/9.3/data/


Start PostgreSQL service using following command.

```
# service postgresql-9.3 start

```
Setup PostgreSQL service to auto start on system boot.

```
# chkconfig postgresql-9.3 on

```
Verify PostgreSQL Installation

After completing, we have installed postgres 9.3 on server, Let do a basic test to verify that installation completed successfully. To verify switch to postgres user.

```
# su - postgres
```
Use psql command to access PostgreSQL prompt with admin privileges.
```
$ psql

psql (9.3.1)
Type "help" for help.

postgres=#
```
Congratulation’s! You have successfully installed PostgreSQL Server.


Refernce:

http://tecadmin.net/install-postgresql-on-centos-rhel-and-fedora/#


Create user and databases for canvas

```
psql -U postgres
create user canvas password 'canvas';
CREATE DATABASE canvas_production ENCODING 'UTF8' OWNER canvas;
CREATE DATABASE canvas_queue_production ENCODING 'UTF8' OWNER canvas;
GRANT ALL PRIVILEGES ON DATABASE canvas_production to canvas;
GRANT ALL PRIVILEGES ON DATABASE canvas_queue_production to canvas;
\q
```
Follow This Steps


[Deploy-Canvas-LMS-Install](https://github.com/m-narayan/beacon/wiki/Deploy-Canvas-LMS-Install-in-nginx)


Passenger Configuration file

# vim /etc/httpd/conf.d/passenger.conf

```
# http://www.modrails.com/documentation/Users%20guide%20Apache.html#_configuring_phusion_passenger
PassengerRoot /usr/local/lib/ruby/gems/1.9.1/gems/passenger-4.0.29
PassengerDefaultRuby /usr/local/bin/ruby
PassengerDefaultUser canvasuser
# enable "high performance mode" which is incompatible with some apache modules
# such as mod_rewrite -- we don't think we need those, so this takes some load off httpd for each request
PassengerHighPerformance on
# Increase the log-level a bit while we get familiar with Passenger
PassengerLogLevel 1
# Set idle timeouts to 0 so that all Rails apps stick around
RailsAppSpawnerIdleTime 0
RailsFrameworkSpawnerIdleTime 0
PassengerPoolIdleTime 0
# Related: Minimum/maximum number of instances to start *once the application has been accessed*
PassengerMaxPoolSize 10
# Settings below here are only supported on Passenger 3.0 and above
PassengerMinInstances 2
# PreStart URLs. Passenger will "ping" this/these URL(s) to invoke the apps when Apache starts
# (note, Passenger internally replaces the host part of the URL with 'localhost', but the URL must match the VirtualHost)
PassengerPreStart 192.241.217.62

```