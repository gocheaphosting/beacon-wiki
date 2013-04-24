- Install apt dependencies 

- Install postgres client
apt-get install postgresql-client

- Instal other supporting lib packages

 apt-get install zlib1g-dev libxml2-dev libmysqlclient-dev libxslt1-dev   imagemagick libpq-dev nodejs libxmlsec1-dev libcurl4-gnutls-dev bison openssl libxmlsec1 

# build related pagakages
apt-get install build-essential libreadline6 libreadline6-dev zlib1g libssl-dev libyaml-dev autoconf libc6-dev ncurses-dev  

-  Java Install 

-Avoid using open-jdk for stability

[Install-Oracle-JDK-on-Linux](https://github.com/m-narayan/beacon/wiki/Install-Oracle-JDK-on-Linux)

./configure  
make  
make install 