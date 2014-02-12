Step 1:

Login to the postgresql

```
[root@psql ~]# psql -U postgres
psql (9.3.2)
Type "help" for help.
```

The syntax for createdb is:

```
createdb {option} ({dbname} {description})
```

Step 2:

Creating Database without Option's

```
postgres=# create database linuxddemo;
CREATE DATABASE
postgres=# 
```

Step 3:

Creating Database Using Option's
for this we don't need to login into postgres user, We can create database from any user privilage.

```
# createdb -h localhost -p 5432 -U postgres linuxddemodb;
```

Step 4:

To list the Database which Created, login to psql and use command 

```
# \l
```

Output:

```
postgres=# \l
                                    List of databases
     Name      |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
---------------+----------+----------+-------------+-------------+-----------------------
 linuxddemo    | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 linuxddemodb  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres      | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
               |          |          |             |             | postgres=CTc/postgres
 template1     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
               |          |          |             |             | postgres=CTc/postgres
(5 rows)
```

That's it...