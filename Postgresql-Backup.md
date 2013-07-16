# Basic Commands

psql -U postgres

\l - List all databases

\dt - List all tables in current db

\c dbname - Connect to certain databases

\du - List all users


## Backup From production server to a development server :


	pg_dump -C -h localhost -U postgres canvas_production | psql -h 192.168.1.80 -U postgres postgres

### Plain SQL format Dump and Restore :

pg_dump -U username -Fp dbname > File name

or

pg_dump -U username dbname -f filename

For restoring use psql command

psql -U username -f filename dbname

or
postgres=# \i SQL-file-name 


### Custom Format


pg_dump -Fc dbname -f filename

pg_restore -U username -d dbname filename

or
 cat tar-file.tar | psql -U username dbname


### Tar format

 pg_dump -Ft dbname -f filename

pg_restore -U username -d dbname filename


### cluster Level Dump :

pg_dumpall -p portnumber > filename

psql -f filename 


### Large db 

pg_dump dbname | gzip > filename.gz

gunzip -c filename.gz | psql dbname