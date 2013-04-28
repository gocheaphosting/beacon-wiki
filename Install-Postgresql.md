## Postgresql Install

`sudo apt-get install postgresql-9.1`

## Install postgresql-contrib packages

`sudo apt-get install postgresql-contrib`

## Change the user authentication method

 `sudo vi /etc/postgresql/9.1/main/pg_hba.conf`

 - local      all     postgres     peer 
to
 - local      all     postgres     trust

# provide access to the db from the computers in the sub-net
 - host all all 192.168.1.0/24 trust

## Change the Postgres to listn for network connections

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

/usr/lib/postgresql/9.1/lib/adminpack.so
/usr/share/postgresql/9.1/extension/adminpack--1.0.sql
/usr/share/postgresql/9.1/extension/adminpack.control

```

In PostgreSQL 9.1 and later, extensions can be installed via the CREATE EXTENSION command:

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