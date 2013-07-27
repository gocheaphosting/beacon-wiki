## Postgresql Install

```
sudo apt-get update
sudo apt-get -y install python-software-properties
sudo add-apt-repository ppa:pitti/postgresql
sudo apt-get update
 
sudo apt-get -y install postgresql-9.2 postgresql-client-9.2 postgresql-contrib-9.2
sudo apt-get -y install postgresql-server-dev-9.2 libpq-dev
```

## Change the user authentication method

```
sudo passwd postgres
su - postgres
psql -c"alter user postgres with password 'postgres';"
 
sudo vi /etc/sysctl.conf
"""
kernel.shmmax=8589934592   (8G * 1024 * 1024 * 1024)
"""
 
/sbin/sysctl -p
 
sudo vi /etc/postgresql/9.2/main/postgresql.conf
"""
#data_directory = '/var/lib/postgresql/9.2/main'
data_directory = '/mnt/postgresql/9.2/main'
listen_addresses = '*'
unix_socket_directory = '/var/run/postgresql'
shared_buffers = 4096MB    # < kernel.shmmax
"""
 
sudo vi /etc/postgresql/9.2/main/pg_hba.conf
'''
local all postgres              peer
host  all all 127.0.0.1/32      md5
host  all all 192.168.1.0/24    md5
host  all all 10.200.13.221/32  md5
host  all all 10.200.0.117/32   md5
host  all all ::1/128           md5

```

 `sudo vi /etc/postgresql/9.2/main/pg_hba.conf`

 - local      all     postgres     peer 
to
 - local      all     postgres     trust

# provide access to the db from the computers in the sub-net
 - host all all 192.168.1.0/24 trust

## Change the Postgres to listn for network connections

```
sudo vi /etc/postgresql/9.2/main/postgresql.conf
```

-change the Connection Settings 

 - listen_addresses = 'localhost' 
to
 - listen_addresses = '*' 

- restart the postgresql

 `sudo /etc/init.d/postgresql restart`

# test the postgresql connection 

 `psql -U postgres`

 `\q`


## Install the adminpack

Try locate adminpack. But first, run `updatedb` to make sure the locate database is up to date.

````
sudo updatedb
locate adminpack
```

The output is:

```

/usr/lib/postgresql/9.2/lib/adminpack.so
/usr/share/postgresql/9.2/extension/adminpack--1.0.sql
/usr/share/postgresql/9.2/extension/adminpack.control

```

In PostgreSQL 9.2 and later, extensions can be installed via the CREATE EXTENSION command:

```

sudo -u postgres psql
CREATE EXTENSION "adminpack";

```

## Create user and databases for canvas

```
psql -U postgres
create user canvas password 'canvas';
CREATE DATABASE canvas_production ENCODING 'UTF8' OWNER canvas;
CREATE DATABASE canvas_queue_production ENCODING 'UTF8' OWNER canvas;
GRANT ALL PRIVILEGES ON DATABASE canvas_production to canvas;
GRANT ALL PRIVILEGES ON DATABASE canvas_queue_production to canvas;
\q

```

## Create user and databases for beacon

psql -U postgres

create user beacon password 'beacon';

CREATE DATABASE beacon_production ENCODING 'UTF8' OWNER beacon;

GRANT ALL PRIVILEGES ON DATABASE beacon_production to beacon;

\q

## Create user and databases for cas
**need to find a way to give read-only access to the beacon_production.users table.**

psql -U postgres

create user rubycas password 'rubycas';

\q

## remove a PG database

psql -U postgres -c "drop database canvas_queue_production"