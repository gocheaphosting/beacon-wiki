Server Name : arrivuLMS

Server IP   : 192.241.221.xxx
_______________________________________________

###Database :
_______________________________________________

Database - PostgreSQL-9.2.4 

list of databases and owners
  
	-lms_production - lmsuser
	-lms_queue_production - lmsuser

List of all users

	-postgres  - Superuser
	-backupuser -Superuser
	-lmsuser
 
How to start/stop/restart/reload PostgreSQL

 $ sudo /etc/init.d/postgresql start  or sudo service postgresql start

 $ sudo /etc/init.d/postgresql stop  or sudo service postgresql stop

 $ sudo /etc/init.d/postgresql restart  or sudo service postgresql restart

 $ sudo /etc/init.d/postgresql reload  or sudo service postgresql reload


Locations :

Data_directory : /var/lib/postgresql/9.2/main/

Config_files  :  /etc/postgresql/9.2/main/

______________________________________________________________________________

### Nginx
_______

server name : arrivuapps.com

 root /var/deploy/lms/current/public; 

How to stop/start/restart nginx :

 $ sudo /etc/init.d/nginx start or sudo service nginx start

 $ sudo /etc/init.d/nginx stop or sudo service nginx stop

 $ sudo /etc/init.d/nginx restart or sudo service nginx restart

______________________________________________________________________________

### Monit 
______


What is monit 

	Monit is a utility for managing and monitoring, processes, files, directories and devices on a UNIX system. Monit conducts automatic maintenance and repair and can execute meaningful causal actions in error situations.


How to install monit in Ubuntu 

	sudo apt-get install monit

How to Configure :

  configuration file located in /etc/monit/monitrc


 $ sudo vi /etc/monit/monitrc

	

	 Start Monit in the background (run as a daemon):

		set daemon 120            # check services at 2-minute intervals(default 4-minutes)

	 set logfile syslog facility log_daemon   
                    
  		set logfile /var/log/monit.log

	Set the location of the Monit id file which stores the unique id for the Monit instance.

		set idfile /var/lib/monit/id

	Set the location of the Monit state file which saves monitoring states on each cycle.

 		set statefile /var/lib/monit/state

	set the base directory where events will be stored

		set eventqueue
        		basedir /var/lib/monit/events
			 slots 100 

	Monit has an embedded web server which can be used to view status

			 set httpd port 2812 and
			   #use address localhost  # only accept connection from localhost
			   #allow localhost        # allow localhost to connect to the server and
			   allow admin:monit23 # require user 'admin' with password 'monit'
			    allow @adminarrivu  # allow users of group 'monit' to connect (rw)
			    allow @users readonly  # allow users of group 'users' to connect readonly

		

	What are the services we are monitoring in our server :

		-Nginx

		-Postgresql

		-redis

		-File System

	How to monit

		-adding services to monitrc 
		
		-sudo vi /etc/monit/monitrc


		##Nginx

			check process nginx with pidfile /var/run/nginx.pid
			    start program = "/etc/init.d/nginx start"
			    stop program = "/etc/init.d/nginx stop"

		##Postgresql

			check process postgresql-9.2 with pidfile /var/run/postgresql/9.2-main.pid
			group database
			start program = "/etc/init.d/postgresql start"
			restart program = "/etc/init.d/postgresql restart"
		##Redis
			check process redis with pidfile /var/run/redis/redis-server.pid
			  start program = "/etc/init.d/redis-server start"
			  stop program = "/etc/init.d/redis-server stop"
			  group redis

		##Check DevicE

			check device disk1 with path /dev/vda

			 start = "/bin/mount /dev/vda"

			 stop = "/bin/umount /dev/vda"

			  if space usage > 90% then alert

			 if space usage > 99% then stop

			  if inode usage > 90% then alert

			  if inode usage > 99% then stop
			   alert alfred@arrivusystems.com


How to start/stop/restart monit :

 $ sudo /etc/init.d/monit start or sudo service monit start

 $ sudo /etc/init.d/monit stop or sudo service monit stop

 $ sudo /etc/init.d/monit restart or sudo service monit restart


How to check the monit syntax :

 $ sudo monit -t 

________________________________________________________________________________________________________________________________________

### Backup Gem 
__________________

what is backup gem

	Backup is a system utility for Linux and Mac OS X, distributed as a RubyGem, that allows you to easily perform backup operations. 

How to install backup gem

	sudo gem install backup 

	-Do not add gem backup to another application's Gemfile.

How to generate a backup model for postgresql database :

	$ backup generate:model --trigger lms_queue_production_db --archives --databases='postgresql' --compressors=gzip \
          --storages='local' synchers='rsync_local' --notifiers='mail'

	-The above generator will provide us with a backup model file (located in ~/Backup/models/lms_queue_production_db.rb)

	Options: 

		--trigger(alias -t) - specifies which backup model you wish to run.

		--archives - Archives are created using the archive command

		     -its look like as follows
			
			Backup::Model.new(:lms_queue_production_db, 'lms_queue_production_db') do
			  archive :my_archive do |archive|
			    # Run the `tar` command using `sudo`
			    archive.use_sudo
			    # add a file
			    archive.add '/path/to/a/file.rb'
			    # add a folder (including sub-folders)
			    archive.add '/path/to/a/folder/'
			    # exclude a file
			    archive.exclude '/path/to/a/excluded_file.rb'
			    # exclude a folder (including sub-folders)
			    archive.exclude '/path/to/a/excluded_folder'
			  end
			end

		--databases 
                	
			-used to take database backup it will looks like 

				 database PostgreSQL do |db|
				    # To dump all databases, set `db.name = :all` (or leave blank)
				    db.name               = "lms_queue_production"
				    db.username           = "xxxxxxxx"
				    db.password           = "yyyyyyyy"
				    db.host               = "localhost"
				    db.port               = 5432
				  end
		--compressors

			-Backup includes a Gzip and Bzip2 Compressor.
			-It also includes a Custom Compressor to support any other compressor.
			  
				 compress_with Gzip

		--storages

			-we are storing locally and keeping 10 copies


				 store_with Local do |local|
				    local.path       = "~/db_backups/"
				    local.keep       = 10
				  end

		--notifiers 
			-used to send notifiers			
			-we are notifing through mail

				 notify_by Mail do |mail|
				  mail.on_success           = true
				    mail.on_warning           = true
				    mail.on_failure           = true

				    mail.from                 = "alfredarrivusystems@gmail.com"
				    mail.to                   = "alfredarrivusystems@gmail.com"
				    mail.address              = "smtp.gmail.com"
				    mail.port                 = 587
				    mail.domain               = "www.gmail.com"
				    mail.user_name            = "alfredarrivusystems@gmail.com"
				    mail.password             = "**********"
				    mail.authentication       = "plain"
				    mail.encryption           = :starttls
				  end

		--synchers 

			-we are doing local first and send to remote server

				 sync_with RSync::Local do |rsync|
				    rsync.path     = "~/rsync_backups"
				    rsync.mirror   = true

				    rsync.directories do |directory|
				       directory.add "~/db_backups/lms_queue_production_db/../"
				    end
				  end

List of all backup scripts

 location : /home/sysadmin/Backup/models

	arrivu_lms_auth_logs.rb 
	arrivu_lms_nginx_logs.rb   
	arrivu_lms_redis_logs.rb  
	lms_production_db.rb
	arrivu_lms_current_logs.rb
  	arrivu_lms_postgresql_logs.rb
  	arrivu_lms_sys_logs.rb
    	lms_file_store_backup.rb
  	lms_queue_production_db.rb



What are the apllications we are currently taking backups using backup gem

	-auth_logs
	-nginx_logs
      	-redis_logs
	-lms_current_logs
 	-postgresql_logs
  	-sys_logs
   	-lms_file_store_backup
        -lms_queue_production_database
	-lms_production_database

How to perform all these backups

	sudo backup perform -t arrivu_lms_postgresql_logs

	sudo backup perform -t arrivu_lms_nginx_logs

	sudo backup perform -t arrivu_lms_redis_logs

	sudo backup perform -t arrivu_lms_auth_logs

	backup perform -t arrivu_lms_sys_logs

	sudo backup perform -t lms_file_store_backup

	backup perform -t lms_production_db

	backup perform -t lms_queue_production_db

	backup perform -t arrivu_lms_current_logs

	sudo backup perform -t arrivu_lms_nginx_logs

Daily Backups

	sudo backup perform -t lms_file_store_backup
	backup perform -t lms_production_db 
	backup perform -t lms_queue_production_db
	backup perform -t arrivu_lms_current_logs
	sudo backup perform -t arrivu_lms_nginx_logs


Weekly Backups
 
	sudo backup perform -t arrivu_lms_postgresql_logs

	sudo backup perform -t arrivu_lms_redis_logs
	
	sudo backup perform -t arrivu_lms_auth_logs
	
	backup perform -t arrivu_lms_sys_logs


Ref :https://github.com/meskyanichi/backup

Automatic Backups(using whenever gem)

 Whenever gem
___________________

	-a Ruby Gem that allows you to write elegant syntax for managing the crontab.

 How to install 

	sudo gem install whenever

 How to configure

	$ mkdir config 

		- Whenever assumes a config directory exists

	$ whenver
		
		-it will create schedule.rb in the location - /config/schedule.rb

	-open schedule.rb file and add cronjobs

		-sample schedule.rb file

			every 1.day, :at => '4:30 am' do
			  command "backup perform -t my_backup"
			end

		-our schedule.rb file

			every 1.day, :at => '4:30 am' do
			 command "/usr/local/bin/backup perform -t lms_production_db --config-file /home/sysadmin/Backup/config.rb"
			 command "/usr/local/bin/backup perform -t lms_queue_production_db --config-file /home/sysadmin/Backup/config.rb" 
			end

		-After adding schedule.rb file

			$ whenever

				-crontab entry will generate

		-Update crontab

			$ whenever --update-crontab

		-To view crontab entry

			$ crontab -l

		-To clear crontab entries

			$ whenever --clear-crontab

Ref :https://github.com/javan/whenever


___________________________________________________________________________________________________________________________

Passing all rsync backups to BBB server

	rsync -avz -e "ssh -p 2002"  /home/sysadmin/rsync_backups/  sysadmin@192.241.217.****:/arrivu_backups/

____________________________________________________________________________________________________________________________

###Log rotate

	Log rotation is an automated process used in system administration in which dated log files are archived. In general, it is used on servers. Servers which run large applications, such as LAMP stacks, often log every request, and as such, the process of log rotation may be beneficial.

How to rotate

	Log rotate config file

		/etc/logrotate.conf

	Directroy which include log rotate config file

		/etc/logrotate.d

	Add log rotate script to /etc/logrotate.conf or /etc/logrotate.d

		if adding script to /etc/logrotate.d
			
			-it must be included in the /etc/logrotate.conf

				add the following line in /etc/logrotate.conf

					-include /etc/logrotate.d

What are the logs we are rotating manually

	-Nginx

	-lms log




Nginx log rotate script

	/var/log/nginx/*.log {
	  weekly
	  compress
	  delaycompress
	  rotate 10
	  missingok
	  nocreate
	  sharedscripts
	  postrotate
	    test ! -f /var/run/nginx.pid || kill -USR1 `cat /var/run/nginx.pid`
	  endscript
	}

	
lms_current log script

	/var/deploy/lms/current/log/*.log {
	  weekly
	  missingok
	  rotate 70
	  compress
	  delaycompress
	  notifempty
	  copytruncate
	}
		

Log rotate Description

	daily – Rotate the log files each day. You can also use weekly or monthly here instead.

	missingok – If the log file doesn’t exist, ignore it

	rotate 7 – Only keep 7 days of logs around

	compress – GZip the log file on rotation

	delaycompress – Rotate the file one day, then compress it the next day so we can be sure that it won’t interfere with the Rails server

	notifempty – Don’t rotate the file if the logs are empty

        copytruncate – Copy the log file and then empties it. This makes sure that the log file Rails is writing to always exists so you won’t get problems because the file does not actually change. If you don’t use this, you would need to restart your Rails application each time.	



Default log rotation :

	apt  aptitude  consolekit  dpkg  lms  monit  nginx  postgresql-common  redis-server  rsyslog  unattended-upgrades  upstart

How to rotate log by default 

	-if we are rotating nginx log

		sudo logrotate -f -v /etc/logrotate.d/nginx

____________________________________________________________________________________________________________________________________________________

###iptables : 
________________

 please refer following link in github

https://github.com/babinlonston/Ubuntu-Linux-Stuffs/blob/master/iptables1wssrv.sh



____________________________________________________________________________________________________________

###ssh :
___________________

	please refer following link in github

https://github.com/babinlonston/Ubuntu-Linux-Stuffs/wiki/How-to-Change-the-ssh-login-in-servers

_________________________________________________________________________________________________________________


### Fail2ban:
______________

	please refer following link in github

https://github.com/babinlonston/Ubuntu-Linux-Stuffs/wiki/How-to-restrict-SSH-Bruteforce-attack-by-installing-fail2ban

________________________________________________________________________________________________________________________



		
		
	


		









