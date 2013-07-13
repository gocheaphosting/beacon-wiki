# Backup gem:

## # Installation :-

	gem install backup

	Do not add gem backup to another application's Gemfile. Backup is not a gem library and should not be treated as a dependency of another application.

## The Command line utility:-

	### Help	 

		$ backup help
		
		$ backup help <command name>

	### Check

		-The check command is used to check your Backup configuration. 
		-this command will load your config.rb file, along with all of your model files, and report any Errors or Warnings generated. This allows to you check your configuration files for syntax errors, as well as detecting other errors or warning such as the use of deprecated configuration settings. It is recommended that you run backup check whenever you update backup.

		If your config.rb file is not in the default location of ~/Backup/config.rb, use the --config-file argument to specify it's location.

			$ backup check --config-file /path/to/config.rb


		As a convenience, this check may also be performed by adding the --check option to your backup perform command, in which case the 			trigger	specified will not be performed.

			$ backup perform --trigger my_backup --check

		If the check is successful, this command will exit with status code 0. If there are any errors/warnings, it will exit with status 			code 1.

	### Perform

		The perform action is used to run the backup job(s) you have defined in your model files.

			$ backup perform --trigger my_backup

		The trigger identifies the model and is set when it is defined.

		To asynchronously perform multiple backup models, provide multiple triggers, each separated                               by a comma.

			$ backup perform --triggers my_backup_1,my_backup_2,my_backup_3

		Each trigger will be run in the order specified, one after the other.

	### Generate

		The generate command can be used to generate the main Backup configuration file and new Backup model files.
	
	### Version

		Display the current version of backup

			$ backup version


## Performing Backup

	$ backup perform --trigger my_backup

	This command will load the main configuration file, located by default at ~/Backup/config.rb. The main configuration file config.rb is setup 		to load all models from the models/ subdirectory relative to it's location. So, by default this will be: ~/Backup/models/.

	Therefore, in order for the above command to work, a model must exist which defines the my_backup trigger.

## Command Line Options

	--trigger (aliases: --triggers, -t)

		The --trigger option specifies which backup model you wish to run.

	--config-file (alias: -c)

		The Generator allows you to specify the --config-path when generating a model, if you want to store your configuration in a location 			other than the default ~/Backup. 

		 $ backup perform --trigger my_backup --config-file /path/to/config.rb

	--data-path (alias: -d)
		Backup has a Cycling feature, which can automatically perform backup rotation for you. In order to do this, Backup stores a YAML 			formatted data file with information about your backups, based on the Storages used. This data is stored by default in ~/Backup/data. 			If you are storing this data in another location, it will need to be specified using the --data-path option.

			$ backup perform --trigger my_backup --data-path /path/to/data/dir/

	--tmp-path

		During the backup process, all of the Archives and Databases being processed are stored in ~/Backup/.tmp, where they are packaged and 			optionally Split into chunks before being transferred to your Storages. If you want to specify a different directory for this, use:

			$ backup perform --trigger my_backup --tmp-path /path/to/.tmp/

	--cache-path

		If you're using the Dropbox service, then Backup uses the ~/Backup/.cache directory to store the session cache file used to authorize 			connections to Dropbox. To store this in another location, use:

			$ backup perform --trigger my_backup --cache-path /path/to/.cache/

	--root-path (alias: -r)

		If you are happy with the default directory names, but would like to establish this hierarchy in a location other than ~/Backup, then 			you can specify a new root directory using:

			$ backup perform --trigger my_backup --root-path /path/to/root/dir/

## Logging Options
		--quiet (alias: -q)

		--logfile

			By default, Backup keeps a backup.log file containing all messages produced for all backup jobs. The backup.log file will be 				kept truncated (to ~500KB by default), with older entries being removed.

			Since this is the default behavior, use of this flag is not needed. However, you may use --no-logfile to force Backup not to 				use this log file.

		--log-path (alias: -l)

			$ backup perform --trigger my_backup --log-path /path/to/log/dir/

		--syslog

			Backup also supports sending log message to your system's Syslog facility. Use of this flag enables this option.

			Backup also supports sending log message to your system's Syslog facility. Use of this flag enables this option.

Checking for Configuration Errors

		--check

		The backup perform command will exit with the following status codes:

		0: All triggers were successful and no warnings were issued. 
                1: All triggers were successful, but some had warnings.
                2: All triggers were processed, but some failed. 3:
                3: A fatal error caused Backup to exit. Some triggers may not have been processed.


## Automatic Backup

	-Use Whenever gem

		$ gem install whenever

	Generate a schedule.rb file with the Whenever gem

		$ mkdir config # Whenever assumes a `config` directory exists
		$ wheneverize
		~ [add] writing './config/schedule.rb'
		~ [done] wheneverized!

	Open the config/schedule.rb file and add the following:
		
		every 1.day, :at => '4:30 am' do
		  command "backup perform -t my_backup"
		end

	Run whenever with no arguments see the crontab entry this will create


	$ whenever --update-crontab

	$ crontab -l # to view the crontab entry


## Generator

	$ backup help generate:model



## Sample Config file:-

   $ backup perform -t my_backup [-c <path_to_configuration_file>]

Backup::Model.new(:my_backup, 'Description for my_backup') do

  
  # Split [Splitter]

 
  # Split the backup file in to chunks of 250 megabytes

  # if the backup file size exceeds 250 megabytes

  
  split_into_chunks_of 250

  ##

  # Archive [Archive]

  #

  # Adding a file or directory (including sub-directories):

  #   archive.add "/path/to/a/file.rb"

  #   archive.add "/path/to/a/directory/"

  #

  # Excluding a file or directory (including sub-directories):

  #   archive.exclude "/path/to/an/excluded_file.rb"

  #   archive.exclude "/path/to/an/excluded_directory

  
  # By default, relative paths will be relative to the directory

  # where `backup perform` is executed, and they will be expanded

  # to the root of the filesystem when added to the archive.

 
  # If a `root` path is set, relative paths will be relative to the

  # given `root` path and will not be expanded when added to the archive.

  
  #   archive.root '/path/to/archive/root'

  
  # For more details, please see:

  # https://github.com/meskyanichi/backup/wiki/Archives


  archive :my_archive do |archive|

    # Run the `tar` command using `sudo`

    # archive.use_sudo

   # archive.add "/path/to/a/file.rb"

    archive.add "~/rails/"

   # archive.exclude "/path/to/a/excluded_file.rb"

   # archive.exclude "/path/to/a/excluded_folder"

  end

  ##
  # PostgreSQL [Database]
  #

  database PostgreSQL do |db|

    # To dump all databases, set `db.name = :all` (or leave blank)

    db.name               = "bkpdb"

    db.username           = "bkpuser"

    db.password           = "bkpuser"

    db.host               = "localhost"

    db.port               = 5432

   # db.socket             = "/tmp/pg.sock"

    # When dumping all databases, `skip_tables` and `only_tables` are ignored.

   # db.skip_tables        = ["skip", "these", "tables"]

   # db.only_tables        = ["only", "these", "tables"]

   # db.additional_options = ["-xc", "-E=utf8"]

  end

  ##
  # Local (Copy) [Storage]
  #

  store_with Local do |local|

    local.path       = "~/backups/"

    local.keep       = 5

  end

  ##
  # Gzip [Compressor]
  #
  compress_with Gzip

  ##
  # Mail [Notifier]
  #

  # The default delivery method for Mail Notifiers is 'SMTP'.

  # See the Wiki for other delivery options.

  # https://github.com/meskyanichi/backup/wiki/Notifiers

  #

  #notify_by Mail do |mail|

   # mail.on_success           = true

    #mail.on_warning           = true

    #mail.on_failure           = true

  # mail.from                 = "alfredmca@gmail.com"

   # mail.to                   = "alfredmca@gmail.com"

   # mail.address              = "smtp.gmail.com"

    #mail.port                 = 587

    #mail.domain               = "www.gmail.com"

    #mail.user_name            = "alfredmca@gmail.com"

    #mail.password             = "**********"

    #mail.authentication       = "plain"

    #mail.encryption           = :starttls

  #end

 encrypt_with OpenSSL do |encryption|

    encryption.password      = "admin123$"            # From String

   # encryption.password_file = "/path/to/password/file" # Or from File

    encryption.base64        = true

    encryption.salt          = true

  end

end


# Decrpting the encrpted file :-


	
$ backup decrypt --encryptor openssl --base64 --salt --in 

/home/sysadmin/backups/my_backup/2013.07.13.17.03.43/my_backup.tar.enc  --out 

/home/sysadmin/backups/my_backup/2013.07.13.17.03.43/my_backup


## Automatic Backup

	### install 

		gem install whenever

	### Configure (Wheneverize)

		cd ~/Backup

		mkdir config

	Open the config/schedule.rb file and add the following:

	every :hour do

	  command "backup perform -t my_backup"
	end
                          

	### Schedule to crontab
		whenever -w ~/Backup/config/schedule.rb

	### Update 

		 whenever --update-crontab



