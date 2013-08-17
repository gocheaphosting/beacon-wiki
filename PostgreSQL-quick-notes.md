#### Get the size of all databases

```
 SELECT pg_database.datname, pg_size_pretty(pg_database_size(pg_database.datname)) AS  size_of_dbs FROM pg_database;

```

#### To Find File Locations

```

SELECT name, setting FROM pg_settings WHERE category = 'File Locations';

```

####Postgresql.conf

	-how much memory to allocate
	-default storage locations of new database
	-which ip POSTGRESQL listens
	-where logs are stored and so

####pg_hba.conf

	-controls security
	-which users can login to which databases
	-which ip or group of ip connected the authentication scheme

####pg_ident.conf
	
	-mapping file that maps an authenticated OS login to a POSTGRESQL user


