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

set mailserver smtp.gmail.com port 587
    username "alertforarrivusystems" password "admin123$"
    using tlsv1


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

   > Add services in /etc/monit/monitrc

```
# Host Load Average, CPU, Memory
#
  check system www.jigsawacademy.net
    if loadavg (1min) > 4 then alert
    if loadavg (5min) > 2 then alert
    if memory usage > 75% then alert
    if swap usage > 25% then alert
    if cpu usage (user) > 70% then alert
    if cpu usage (system) > 30% then alert
    if cpu usage (wait) > 20% then alert

#Nginx

        check process nginx with pidfile /var/run/nginx.pid
        start program = "/etc/init.d/nginx start"
        stop program = "/etc/init.d/nginx stop"

##Postgresql

        check process postgresql-9.2 with pidfile /var/run/postgresql/9.2-main.pid
        group database
        start program = "/etc/init.d/postgresql start"
        restart program = "/etc/init.d/postgresql restart"
        start program = "/etc/init.d/postgresql stop"
        if failed host localhost  port 5432 then restart
        stop program = "/etc/init.d/postgresql stop"

#redis

check process redis-server  with pidfile /var/run/redis/redis-server.pid 
   start program  "/etc/init.d/redis-server start"
   stop program  "/etc/init.d/redis-server stop"
   if 5 restarts within 5 cycles then timeout



#SSH Monitoring 

check process sshd with pidfile /var/run/sshd.pid
   start program  "/etc/init.d/ssh start"
   stop program  "/etc/init.d/ssh stop"
   if failed port 2002 protocol ssh then restart
   if 5 restarts within 5 cycles then timeout

#fail2ban Monitoring

check process fail2ban with pidfile /var/run/fail2ban/fail2ban.pid
  group services
  start program = "/etc/init.d/fail2ban start"
  stop  program = "/etc/init.d/fail2ban stop"
  if 5 restarts within 5 cycles then timeout

# Delayed Jobs Monitoring 

        check process delayed_job with pidfile /var/deploy/capistrano/jigsaw/current/tmp/pids/delayed_jobs_pool.pid
      start program = "/etc/init.d/canvas_init start"
      stop program = "/etc/init.d/canvas_init stop"

#Check DevicE

        check device disk1 with path /dev/vda
	start = "/bin/mount /dev/vda"
	stop = "/bin/umount /dev/vda"
	if space usage > 90% then alert
	if space usage > 99% then stop
	if inode usage > 90% then alert
	if inode usage > 99% then stop
	alert alertforarrivusystems@gmail.com

```