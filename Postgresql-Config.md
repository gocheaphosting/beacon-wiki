## Postgresql Install

sudo apt-get install postgresql-9.1

## Install postgresql-contrib packages

sudo apt-get install postgresql-contrib


## Change the user authentication method

- sudo vi /etc/postgresql/9.1/main/pg_hba.conf

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

- sudo /etc/init.d/postgresql restart

# test the postgresql connection 

- psql -U postgres
- \q

## Create user and databases for canvas

- psql -U postgres
- create user canvas password 'canvas';
- CREATE DATABASE canvas_production ENCODING 'UTF8' OWNER canvas;
- CREATE DATABASE canvas_queue_production ENCODING 'UTF8' OWNER canvas;
- GRANT ALL PRIVILEGES ON DATABASE canvas_production to canvas;
- GRANT ALL PRIVILEGES ON DATABASE canvas_queue_production to canvas;
- \q

## Create user and databases for beacon

- psql -U postgres
- create user beacon password 'beacon';
- CREATE DATABASE beacon_production ENCODING 'UTF8' OWNER beacon;
- GRANT ALL PRIVILEGES ON DATABASE beacon_production to beacon;
- \q

## Create user and databases for cas

- psql -U postgres
- create user rubycas password 'rubycas';
- **need to find a way to give read-only access to the beacon_production.users table.**
- \q


