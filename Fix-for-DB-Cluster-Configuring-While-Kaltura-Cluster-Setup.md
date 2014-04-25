```
# mysql -u root -p

db_password_here

use mysql

GRANT ALL PRIVILEGES ON *.* TO root@'%' IDENTIFIED BY 'db_password_here' WITH GRANT OPTION;
```