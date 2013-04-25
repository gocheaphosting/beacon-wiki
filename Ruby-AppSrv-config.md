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

** Configure Gem not to install RI and RDoc **

- Add the following two lines to /etc/gemrc

install: --no-rdoc --no-ri 
update:  --no-rdoc --no-ri


**Install Bundler**

gem install bundler