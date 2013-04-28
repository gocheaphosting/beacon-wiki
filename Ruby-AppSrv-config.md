## Install git

```
sudo apt-get install git-core
```

## Install postgres client 

```
apt-get install postgresql-client
```

## Install apt dependencies and other supporting lib packages

```
 apt-get install zlib1g-dev libxml2-dev libmysqlclient-dev libxslt1-dev   imagemagick libpq-dev nodejs libxmlsec1-dev libcurl4-gnutls-dev bison openssl libxmlsec1 
```

## Install build related packages

```
apt-get install build-essential libreadline6 libreadline6-dev zlib1g libssl-dev libyaml-dev autoconf libc6-dev ncurses-dev  

```

## Install Java

**Avoid using open-jdk for stability. **
 [Install-Oracle-JDK-on-Linux](https://github.com/m-narayan/beacon/wiki/Install-Oracle-JDK-on-Linux) 

## Ruby 1.9.3 install from source

get the ruby code from the ruby website

```
curl -O ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p392.tar.gz
```

untar the ruby source 

```
tar -xvf ruby-1.9.3-p392.tar.gz
cd ruby-1.9.3-p392
```
complie and build ruby by running the following commands

```
./configure  
make  
make install 
```

## Configure Gem not to install RI and RDoc

Add the following two lines to /etc/gemrc and ~/.gemrc of the root account and sysadmin account

```
install: --no-rdoc --no-ri 
update:  --no-rdoc --no-ri
```

## Install Bundler

```
gem install bundler
```

## Install rake

```
gem install rake
```

