Login to the postgres user 

```
[root@psql ~]# psql -U postgres
psql (9.3.2)
Type "help" for help.

postgres=# 
```

List the Database's :

```
postgres=# \l
                                    List of databases
     Name      |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
---------------+----------+----------+-------------+-------------+-----------------------
 linux         | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 linuxdatabase | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres      | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
               |          |          |             |             | postgres=CTc/postgres
 template1     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
               |          |          |             |             | postgres=CTc/postgres
 unixdb        | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
(6 rows)
```

Here I want to select the linuxdatabase Using \c to select the particular database

```
postgres=# \c linuxdatabase
You are now connected to database "linuxdatabase" as user "postgres".
linuxdatabase=#
```

That's it
