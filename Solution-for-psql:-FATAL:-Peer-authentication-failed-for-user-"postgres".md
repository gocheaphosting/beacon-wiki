
if we get this following error while using su -U postgres

```
psql: FATAL: Peer authentication failed for user "postgres"
```

For avoiding this error we need to edit the below file and change the permission to the localhost in line number 59

```
# vim /var/lib/pgsql/9.3/data/postgresql.conf
```

Then Edit the file pg_hba.conf and change the ident to trust in line number 80 & 82

```
# vim /var/lib/pgsql/9.3/data/pg_hba.conf
```

Then restart the postgresql service to take effect the modification

```
# /etc/init.d/postgresql-9.3 restart
```