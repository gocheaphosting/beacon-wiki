```
#!/bin/sh
set -x

sudo touch /etc/apt/sources.list.d/pgdg.list

echo "deb http://apt.postgresql.org/pub/repos/apt/ wheezy-pgdg main" > /etc/apt/sources.list.d/pgdg.list

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - -y

sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get install postgresql-9.3 pgadmin3 -y

exit
```