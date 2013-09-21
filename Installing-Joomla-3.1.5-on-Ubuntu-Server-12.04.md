Here im Using ubuntu 12.04 Server 
Using the IP  192.168.1.100


Step 1. Install Web Server (LAMP)

Joomla developed using php script, database MySQL and running on apache web server. So, before install joomla 3.1.5 in ubuntu server you need to install LAMP Server (Linux Apache MySQL PHP). Login to ubuntu server machine via ssh then install it.


Update the server using 

```
# sudo apt-get update

```


![1](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_002.png)



or you can install lamp server using command tasksel then select LAMP Server Using Installation Media 


```

# sudo apt-get install tasksel

```


![2](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_003.png)


Start Task selection


```

# sudo tasksel


```


![3](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_004.png)



![4](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_005.png)



If not Install it Manually By Following Link 


[Settingup a LAMP Server in Ubuntu 12.04](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/wiki/Settingup-a-LAMP-Server-in-Ubuntu12.04)

Step 2. Configure Apache and Create Database

On this step, you need create apache config for joomla in directory apache webserver.


```

# sudo cp /etc/apache2/sites-available/default /etc/apache2/sites-available/joomla


```

![6](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_006.png)

then enable the Joomla website and restart apache web-server with following commands:


```

# sudo a2ensite joomla
# sudo service apache2 restart

```


![7](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_007.png)


Next, Log in to mysql server as root user



```

# mysql -u root -p


```



![8](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_008.png)



Create database for joomla with command below,in case we’ll create database with name “dbjoomla”



```

# CREATE DATABASE dbjoomla;


```

![9](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_009.png)



Create a new user of username joomlauser, with following command below:


```

# CREATE USER joomlauser;

```

![10](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_010.png)


Create password “joomlapwd” for user “joomlauser”;


```


# SET PASSWORD FOR joomlauser = PASSWORD("admin123$");


```


Grant user joomlauser all permissions on the database.


```

GRANT ALL PRIVILEGES ON dbjoomla.* TO joomlauser@localhost IDENTIFIED BY 'admin123$';

```

![11](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_011.png)


Reload the settings for Mysql 


```


# FLUSH PRIVILEGES;

```


![12](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_012.png)


Log out from mysql server.


```
# exit

```

Now joomla ready to install on ubuntu server




Step 3. Download and Installing Joomla 3.1.5

Download joomla 3.1.5 from joomla official website. then create directory joomla  in the default apache folder /var/www


```

# wget http://joomlacode.org/gf/download/frsrelease/17574/76732/Joomla_3.1.5-Stable-Full_Package.zip


```
Create a Directory Named Joomla inside /var/www/


```
# sudo mkdir /var/www/joomla

```

Extract joomla package has beed downloaded in to directory /var/www/joomla


```

# sudo unzip -q Joomla_3.1.5-Stable-Full_Package.zip -d /var/www/joomla

```


![14](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_014.png)


Change access /var/www/joomla to apache user and goup (www-data) and set permission /var/www/joomla 775


```
# sudo chown -R www-data.www-data /var/www/joomla/
# sudo chmod -R 777 /var/www/joomla/

```



![15](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_015.png)


![16](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_016.png)


Now you can install joomlo 3.0 from your favorite browser by typing 192.168.1.100/joomla


```

# 192.168.1.100/joomla

```

This will Bring this page for the installations 



![17](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_017.png)



![18](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_018.png)


![20](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_020.png)



![21](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_021.png)



![23](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_023.png)


![24](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Joomla%20Installation/Selection_024.png)


That's it



