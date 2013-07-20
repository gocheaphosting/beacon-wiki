## Get the size of all databases

 SELECT pg_database.datname, pg_size_pretty(pg_database_size(pg_database.datname)) AS  size_of_dbs FROM pg_database;