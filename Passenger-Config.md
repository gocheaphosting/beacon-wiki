## passenger tools
* passenger-config            
* passenger-memory-stats
* passenger-install-apache2-module  
* passenger-status
* passenger-install-nginx-module 

# how to check passenger install dir 

    passenger-config --root


## Nginx configuration

### Disabling passenger without uninstalling

To temporarily unload (disable)  Passenger from the web server, without uninstalling the Passenger files
Connect out the following three lines in the 

# passenger_root /somewhere/passenger-x.x.x;
# passenger_ruby /usr/bin/ruby;
# passenger_max_pool_size 10;
## passenger tools
