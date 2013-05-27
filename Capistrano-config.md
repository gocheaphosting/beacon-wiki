## Install Capistrano 

run the following commands from a Terminal or command line. We recommend to also install the capistrano-ext gem that contains an extra set of tools to make the deployments even easier:
```
gem install capistrano
gem install capistrano-ext
```

##Server Dependencies
It’s important to make sure that your server is POSIX-compilant and has SSH access. It is better to setup the ssh public/private key authentication for the sysadmin user to deploy the code. 
Use the [SSH-With-Public-Key-Authentication](https://github.com/m-narayan/beacon/wiki/Set-Up-SSH-With-Public-Key-Authentication) for public key setup.