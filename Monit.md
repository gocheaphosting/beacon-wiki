## About :

Monit is a utility for managing and monitoring, processes, files, directories and devices on a UNIX system. Monit conducts automatic maintenance and repair and can execute meaningful causal actions in error situations.

### How to Install monit in ubuntu :

	sudo apt-get install monit

### How to Configure :

	Default configuration file located at **/etc/monit/monitrc**
	
	The default port of monit is **2812.**

	Start Monit in the background and check services at every one minute
		
		set daemon 60
	Set syslog logging with the 'daemon' facility.

		set logfile /var/log/monit.log
	
	Set the location of the Monit id file which stores the unique id for the  Monit instance

		set idfile /var/lib/monit/id

	uncomment the following lines

		set httpd port 2812 and

		    #use address localhost  # only accept connection from localhost
		    #allow localhost        # allow localhost to connect to the server and
		    allow admin:monit      # require user 'admin' with password 'monit'
		    allow @monit           # allow users of group 'monit' to connect (rw)
		    allow @users readonly  # allow users of group 'users' to connect readonly

	### Start monit 

		sudo /etc/init.d/monit start

	Now navigate to http://localhost:2812/ from your browser. Enter the username as admin and password as monit. 

	Adding additional configuration parts from other files or directories.

 		include /etc/monit/conf.d/*

	### Checking syntax

		sudo monit -t

	### Monit Details

		sudo monit status
	


### Adding services 

   > Add services in monitrc.conf file
Postgresql

	check process postgresql-9.2 with pidfile /var/run/postgresql/9.2-main.pid
	group database
	start program = "/etc/init.d/postgresql start"
	restart program = "/etc/init.d/postgresql restart"
	start program = "/etc/init.d/postgresql stop"
	if failed host 192.168.1.77 port 5432 then restart
	if 5 restarts within 5 cycles then timeout


Nginx

	check process nginx with pidfile /var/run/nginx.pid
	    start program = "/etc/init.d/nginx start"
	    stop program = "/etc/init.d/nginx stop"


Redis

check process redis with pidfile /var/run/redis/redis-server.pid
  start program = "/etc/init.d/redis-server start"
  stop program = "/etc/init.d/redis-server stop"
  group redis


Disk Usage :

check device disk1 with path /dev/vda
 start = "/bin/mount /dev/vda"
 stop = "/bin/umount /dev/vda"
  if space usage > 90% then alert
 if space usage > 99% then stop
  if inode usage > 90% then alert
  if inode usage > 99% then stop
   alert alfred@arrivusystems.com