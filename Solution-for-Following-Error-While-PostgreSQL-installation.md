
After Installing Postgresql we need to initialize for first Use.
If not we will get the following error

```
/var/lib/pgsql/9.3/data is missing. Use "service postgresql-9.3 initdb" to initialize the cluster first.
                                                           [FAILED]
```

Initialize for first Use using Following command

```
# service postgresql-9.3 initdb
````

Then Restart the Service