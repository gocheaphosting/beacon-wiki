bundle installing do_postgres gem error

First step check install postgres 

1.sudo service postgresql status
------> 9.3/main (port 5433): online

Postgres Installed in your System and next step given below 

2.sudo apt-get install postgresql-server-dev-9.3 libpq-dev

and again bundle install

Postgres not installed in your system given below use the next step 

1.sudo apt-get install postgresql-9.3 postgresql-server-dev-9.3 libpq-dev



