### Step 1:

Add the key using Following command

```
# wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

```

### Step 2:

Enable the Repository using Echo Command to the source list

```
# sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" >> /etc/apt/sources.list.d/postgresql.list'
```

### Step 3:

Update the Repo using 

```
# sudo apt-get update
```

### Step 4:

Install the postgresql-9.3

```
# sudo apt-get install postgresql-9.3 
```

### Step 5:

If we need pgadmin to manage database from GUI install pgadmin 

```
# sudo apt-get install pgadmin3
```

That it we have Done.