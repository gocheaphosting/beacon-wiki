Server Name : Portal

Server IP   : 106.187.50.xxx
_______________________________________________

###Database :
_______________________________________________

Database - PostgreSQL-9.2.4 

list of databases and owners
  
	- beacon_production -beacon 
	- canvas_production - canvas
	- canvas_queue_production -canvas
	- casserver_production -rubycas

List of all users

	-postgres  - Superuser
	-backup_user -Superuser
	-beacon
	-canvas
	-rubycas
 
How to start/stop/restart/reload PostgreSQL

 $ sudo /etc/init.d/postgresql start  or sudo service postgresql start

 $ sudo /etc/init.d/postgresql stop  or sudo service postgresql stop

 $ sudo /etc/init.d/postgresql restart  or sudo service postgresql restart

 $ sudo /etc/init.d/postgresql reload  or sudo service postgresql reload


Locations :

Data_directory : /var/lib/postgresql/9.2/main/

Config_files  :  /etc/postgresql/9.2/main/


###Backup Gem 
__________________

what is backup gem

	Backup is a system utility for Linux and Mac OS X, distributed as a RubyGem, that allows you to easily perform backup operations. 

How to install backup gem

	sudo gem install backup 

	-Do not add gem backup to another application's Gemfile.

How to generate a backup model for postgresql database :

	$ backup generate:model --trigger lms_queue_production_db --archives --databases='postgresql' --compressors=gzip \
          --storages='local' synchers='rsync_local,' --notifiers='mail'

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
				   db.name               = "beacon_production"
				    db.username           = "********"
				    db.password           = "********"
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
				       directory.add "~/db_backups/beacon_production_db_backups/../"
				    end
				  end

List of all backup scripts

 location : /home/sysadmin/Backup/models

	auth_log_backups.rb              canvas_production_db_backups.rb                          redis_log_backups.rb
beacon_production_db_backups.rb  canvas_queue_production_db_backups.rb  nginx_log_backups.rb       sys_log_backups.rb
canvas_file_store_backups.rb     cas_log_backups.rb                     portal_log_backups.rb      ufw_log_backups.rb
canvas_log_backups.rb            casserver_production_db_backups.rb     postgresql_log_backups.rb




What are the apllications we are currently taking backups using backup gem

	-auth_logs
	-nginx_logs
      	-redis_logs
	-canvas_logs
	-postgresql_logs
  	-sys_logs
   	-casserver_production_database
	-canvas_production_database
	-canvas_queue_production_database
	-beacon_production
	-casserver_production_database
	-ufw_logs
	-cas_logs

How to perform all these backups

	sudo backup perform -t auth_log_backups

	sudo backup perform -t redis_log_backups

	sudo backup perform -t nginx_log_backups

	sudo backup perform -t sys_log_backups

	sudo backup perform -t portal_log_backups

	sudo backup perform -t ufw_log_backups

	sudo backup perform -t ufw_log_backups

	sudo backup perform -t postgresql_log_backups
	
	sudo backup perform -t canvas_log_backups

	backup perform -t beacon_production_db_backups

	backup perform -t canvas_production_db_backups

	backup perform -t canvas_queue_production_db_backups

	backup perform -t casserver_production_db_backups 

	backup perform -t canvas_file_store_backups


	

Daily Backups

	backup perform -t beacon_production_db_backups

	backup perform -t canvas_production_db_backups

	backup perform -t canvas_queue_production_db_backups

	backup perform -t casserver_production_db_backups 

	backup perform -t canvas_file_store_backups


Weekly Backups
 
	sudo backup perform -t auth_log_backups

	sudo backup perform -t redis_log_backups

	sudo backup perform -t nginx_log_backups

	sudo backup perform -t sys_log_backups

	sudo backup perform -t portal_log_backups

	sudo backup perform -t ufw_log_backups

	sudo backup perform -t ufw_log_backups

	sudo backup perform -t postgresql_log_backups
	
	sudo backup perform -t canvas_log_backups

Ref :https://github.com/meskyanichi/backup

###Automatic Backups(using whenever gem)

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

			every 1.day, :at => '6:00 pm' do
			   command "/usr/local/bin/backup perform -t canvas_production_db_backups --config-file /home/sysadmin/Backup/config.rb"
			   command "/usr/local/bin/backup perform -t beacon_production_db_backups --config-file /home/sysadmin/Backup/config.rb"
			   command "/usr/local/bin/backup perform -t canvas_queue_production_db_backups --config-file /home/sysadmin/Backup/config.rb"
			   command "/usr/local/bin/backup perform -t casserver_production_db_backups --config-file /home/sysadmin/Backup/config.rb"
			   command "/usr/local/bin/backup perform -t canvas_file_store_backups --config-file /home/sysadmin/Backup/config.rb"
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

### Passing all rsync backups to Kaltura server

	sudo rsync -avz   /home/sysadmin/rsyncbackups/ sysadmin@106.187.54.**:/home/sysadmin/portal_backups

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

	-Nginx log

	-lms log
	
	-cas log

	-portal




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

	
canvas log script

	/var/deploy/canvas/current/log/*.log {
	  weekly
	  missingok
	  rotate 10
	  compress
	  delaycompress
	  notifempty
	  copytruncate
	}


Portal log script

	/var/deploy/portal/current/log/*.log {
	  weekly
	  missingok
	  rotate 10
	  compress
	  delaycompress
	  notifempty
	  copytruncate
	}

cas log rotate script

	/var/deploy/cas/current/log/*.log {
	  weekly
	  missingok
	  rotate 10
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

	apport  aptitude     dpkg   newrelic-sysmond      ppp           rsyslog  unattended-upgrades apt         consolekit  monit             postgresql-common  redis-server  ufw      upstart


How to rotate log by default 

	-if we are rotating nginx log then it should like as 

		sudo logrotate -f -v /etc/logrotate.d/nginx

____________________________________________________________________________________________________________

###ssh :
___________________

	please refer following link in github

https://github.com/babinlonston/Ubuntu-Linux-Stuffs/wiki/How-to-Change-the-ssh-login-in-servers

____________________________________________________________________________________________________________

###Fail2ban:
______________

	please refer following link in github

https://github.com/babinlonston/Ubuntu-Linux-Stuffs/wiki/How-to-restrict-SSH-Bruteforce-attack-by-installing-fail2ban

____________________________________________________________________________________________________________
