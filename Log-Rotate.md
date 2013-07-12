# Files and Directories used by Logrotate

	/etc/logrotate.conf-->whole configuration and default options of logrotate is stored.

	/var/lib/logrotate.status    or    /var/lib/logrotate/status-->information about the last rotation times of different log files.

# The directory thats usually used by logrotate is

	/etc/logrotate.d

/etc/logrotate.conf -it will include /etc/logrotate.d

	include /etc/logrotate.d

# Log rotating nginx
    No default log rotation like apache so we have to do manually

Sample nginx Log rotate

sudo nano /etc/logrotate.d/nginx


/var/www/vhosts/domain.com/logs/*.log {  (we have to give exact path of nginx log directory-/var/log/nginx)
    daily
    missingok
    rotate 52
    compress
    delaycompress
    notifempty
    create 0640 www-data adm
    sharedscripts
    prerotate
        if [ -d /etc/logrotate.d/httpd-prerotate ]; then \
            run-parts /etc/logrotate.d/httpd-prerotate; \
        fi; \
    endscript
    postrotate
        [ ! -f /var/run/nginx.pid ] || kill -USR1 `cat /var/run/nginx.pid`
    endscript
}

