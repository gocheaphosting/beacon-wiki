By Default in distribution we can only possible to get download postgresql 8
For latest version we need to add a repository

Step 1:

First of all we need to add the yum repository for postgres

```
# yum install http://yum.postgresql.org/9.3/redhat/rhel-6-x86_64/pgdg-redhat93-9.3-1.noarch.rpm
```

Step 2:

Install the Postgresql server Using below command

```
# yum install postgresql93-server postgresql93-contrib
```

Output :

```
Dependencies Resolved
================================================================================
 Package                   Arch        Version                Repository   Size
================================================================================
Installing:
 postgresql93-contrib      x86_64      9.3.2-1PGDG.rhel6      pgdg93      482 k
 postgresql93-server       x86_64      9.3.2-1PGDG.rhel6      pgdg93      4.0 M
Installing for dependencies:
 postgresql93              x86_64      9.3.2-1PGDG.rhel6      pgdg93      1.0 M
 postgresql93-libs         x86_64      9.3.2-1PGDG.rhel6      pgdg93      190 k
 uuid                      x86_64      1.6.1-10.el6           base         54 k

Transaction Summary
================================================================================
Install       5 Package(s)

Total download size: 5.7 M
Installed size: 23 M
Is this ok [y/N]: y
```

After Installing Postgresql we need to initialize for first Use.
If not we will get the following error

```
/var/lib/pgsql/9.3/data is missing. Use "service postgresql-9.3 initdb" to initialize the cluster first.
                                                           [FAILED]
```

Step 3:

Initialize for first Use using Following command

```
# service postgresql-9.3 initdb
```

Step 4:

Make the Postgresql to run in Runlevels

```
# chkconfig postgresql-9.3 on
```

Step 5:

Check the psql login
Before that we need to give permission for the local user's to get connect 
if not we will get this following error while using su -U postgres

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

Step 6:

Then restart the postgresql service to take effect the modification

```
# /etc/init.d/postgresql-9.3 restart
```

Step 7:

Then Login to the Posgres using psql -U postgres

```
[root@psql ~]# psql -U postgres
psql (9.3.2)
Type "help" for help.

postgres=# 
```

That's it...