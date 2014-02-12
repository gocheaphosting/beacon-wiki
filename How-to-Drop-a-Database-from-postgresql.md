
Here we can see how to remove, or delete, or drop a database from PostgreSQL which one is already exist.

Syntax:

```
DROP DATABASE DATABASE NAME;
```
Step 1:

First list the database which we need to drop

```
postgres=# \l
                                    List of databases
     Name      |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
---------------+----------+----------+-------------+-------------+-----------------------
 linux         | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 linuxdb       | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 linuxdbdb     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres      | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
               |          |          |             |             | postgres=CTc/postgres
 template1     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
               |          |          |             |             | postgres=CTc/postgres
(6 rows)
```

Step 2:

Here we are going to drop the database linuxdb

```
postgres=# drop database linuxdb;
DROP DATABASE
```

Step 3:

We can use a Parameter IF EXISTS While Droping a Database:
Do not throw an error if the database does not exist, A Notification will be Displayed

Here i have Droped the Existing database linux, it there available so it's getting dropped.

```
postgres=# DROP DATABASE IF EXISTS linux;
DROP DATABASE
```

Here I'm trying to drop the database which one is not available:
So it will give us Notification, Without throwing a error.

```
postgres=# DROP DATABASE IF EXISTS linux;
NOTICE:  database "linux" does not exist, skipping
DROP DATABASE
```

To Drop a Database from Terminal without login to postgres user :

```
[root@psql ~]# dropdb -h localhost -p 5432 -U postgres linuxdb
```

This will remove the database.

Step 4:

Here we can see the list of available database's

```
postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(3 rows)
```

That's it...