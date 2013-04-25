**- Install postgres client**

apt-get install postgresql-client

**- Install apt dependencies and other supporting lib packages**

 apt-get install zlib1g-dev libxml2-dev libmysqlclient-dev libxslt1-dev   imagemagick libpq-dev nodejs libxmlsec1-dev libcurl4-gnutls-dev bison openssl libxmlsec1 

**- Get make and build related packages**

apt-get install build-essential libreadline6 libreadline6-dev zlib1g libssl-dev libyaml-dev autoconf libc6-dev ncurses-dev  

**-  Java Install **

-Avoid using open-jdk for stability [Install-Oracle-JDK-on-Linux](https://github.com/m-narayan/beacon/wiki/Install-Oracle-JDK-on-Linux)

**- Get the ruby source tar file**

curl -O ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p392.tar.gz

tar -xvf ruby-1.9.3-p392.tar.gz

cd ruby-1.9.3-p392

- run the following commands

./configure  
make  
make install 

**Configure Gem not to install RI and RDoc**

- Add the following two lines to /etc/gemrc

install: --no-rdoc --no-ri 
update:  --no-rdoc --no-ri


**Install Bundler**

gem install bundler


**Install Nginx with Passenger**

**Install Passenger gem **

Note : - We need to install passenger 4.0 and currently it is a release candidate 6. 

-To get the RC package run the command 

gem install passenger --pre

-Once this gem is released fully , we have to run the following command

gem install passenger 

- Now install Nginx to work with Passenger

_Since we’ll be using Nginx for serving our application, we’re going to install it using the Passenger installer. Nginx modules need to be compiled into nginx, unlike Apache, so we can’t just install the package from the PPA._

passenger-install-nginx-module

# Choose "download, compile, and install Nginx for me"
# Accept defaults for any other questions it asks


**setup a script to control Nginx**

- We’re going to grab the script from Linode:

wget -O init-deb.sh http://library.linode.com/assets/660-init-deb.sh

sudo mv init-deb.sh /etc/init.d/nginx

sudo chmod +x /etc/init.d/nginx

sudo /usr/sbin/update-rc.d -f nginx defaults

- now We can control Nginx with this script. To start and stop the server manually, run:

sudo /etc/init.d/nginx stop

sudo /etc/init.d/nginx start

**how to configure a Nginx server **

Edit the file /opt/nginx/conf/nginx.conf 