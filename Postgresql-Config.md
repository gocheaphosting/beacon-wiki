## Postgresql Install

# sudo apt-get install postgresql-9.1

## Change the user authentication method

- sudo vi /etc/postgresql/9.1/main/pg_hba.conf

 - local      all     postgres     peer 
to
 - local      all     postgres     trust

## Change the Postgres to listn for network connections

-change the Connection Settings 

 - listen_addresses = 'localhost' 
to
 - listen_addresses = '*' 

- restart the postgresql

# sudo /etc/init.d/postgresql restart

# test the postgresql connection 

- psql -U postgres


