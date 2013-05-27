## Install Capistrano 

run the following commands from a Terminal or command line. We recommend to also install the capistrano-ext gem that contains an extra set of tools to make the deployments even easier:
```
gem install capistrano
gem install capistrano-ext
```

##Server Dependencies
It’s important to make sure that your server is POSIX-compilant and has SSH access. It is better to setup the ssh public/private key authentication for the sysadmin user to deploy the code. 
Use the [SSH-With-Public-Key-Authentication](https://github.com/m-narayan/beacon/wiki/Set-Up-SSH-With-Public-Key-Authentication) for public key setup.

##Prepare the Project for Capistrano

Navigate to the application’s root directory (~/rails-project/beacon) in Terminal and run the following command:
```
capify .
```

This command creates a special file called Capfile in your project, and adds a template deployment recipe at config/deploy.rb in your Rails project. Now open the deploy.rb file and delete everything in the template file, as we can fill it with the correct code for a successful deployment.

## How to Create a Capistrano Recipe

In the empty deploy.rb file, enter the name of the app in the first line.

```
set :application, "beacon_portal"
```

Now add a repository to access. Git users can add this:

set :scm, :git
set :repository, "git@github.com:m-narayan/beacon.git"
set :scm_passphrase, ""

Now set the user on the server that we want Capistrano to run commands with and make sure that this user has read & write access to the directory that you specified in the deploy_to variable.

```
set :user, "sysadmin"
```

Begin by including multistage at the top of your deploy.rb file:
```
require 'capistrano/ext/multistage'
```

The capistrano Multistage function comes along with the gem capistrano-ext gem. This allows us to setup one recipe to deploy the code to more than one location. In this example, we’ll deploy to staging and production environments.

Specify your environments, or “stages”: make the staging as the default  as we will often deploy to stage.
```
set :stages, ["staging", "production"]
set :default_stage, "staging"
```

Now populate our production.rb settings:
```
server "beaconlearning.in", :app, :web, :db, :primary => true
set :deploy_to, "/var/capistrano/beacon/portal"
```
And then staging.rb:
```
server "beta.beaconlearning.in", :app, :web, :db, :primary => true
set :deploy_to, "/var/capistrano/beacon/portal_staging"
```
In this example, we have only one server being assigned all three roles (app, web and db) but in real, we may use different servers for these roles.

## Validating the Recipe

For the first time run, Capistrano create the initial directory structure on your server for future deployments. Run the following command from the root directory of the application:

```
cap deploy:setup
```

During this command run, Capistrano will SSH to your server, enter the directory that you specified in the deploy_to variable, and create a special directory structure that’s required for Capistrano to work properly. If something is wrong with the permissions or SSH access, you will get error messages. Look closely at the output Capistrano gives you while the command is running.

The last step before we can do an actual deployment with Capistrano is to make sure that everything is set up correctly on the server from the setup command. There’s a simple way to verify, with the command:

```
cap deploy:check
```

This command will check your local environment and your server and locate likely problems. If you see any error messages, fix them and run the command again. Once you can run cap deploy:check without errors, you can proceed!

## Deploy Using the New Recipe

Once the verification complete on the local machine and server configuration, run the following command:

```
cap deploy
```

This will perform a deployment to your default stage, which is staging. If you want to deploy to production, you’d run the following:

```
cap production deploy
```

##Improve Performance with Remote Cache

Capistrano normally create a new clone/export of the repository on every deploy but the following option makes Capistrano do a single clone of the repository on the server during the first time, then do an git pull on every deploy instead of doing an entire clone/export.

```
set :deploy_via, :remote_cache
```

## Add Custom Deploy Hooks

You can configure events and commands to run after the copying of files completes, such as restart a web-server or run a custom script. Capistrano calls these as “tasks”. For an example, add the following code to your deploy.rb file:
```
namespace :deploy do
  task :restart, :roles => :web do
    run "touch #{ current_path }/tmp/restart.txt"
  end

  task :restart_canvas_init, :roles => :app do
    sudo "monit restart all -g daemons"
  end
end
````
This example includes two custom tasks. The “restart” task is built into Capistrano and will be executed by Capistrano automatically after the deployment is complete. We use the touch tmp/restart.txt technique that’s common to modern Rails applications powered by Passenger.

The second example task is "restart_canvas_init", a custom task that Capistrano won’t run by default. We need to add a hook for it in order for it to run:
```
after "deploy", "deploy:restart_canvas_init" 
```
This command tells Capistrano to execute our task after all deploy operations are complete. You can read more about before and after hooks in the official Capistrano documentation:
* [Before Tasks](https://github.com/capistrano/capistrano/wiki/2.x-DSL-Configuration-Tasks-Before)
* [After Tasks](https://github.com/capistrano/capistrano/wiki/2.x-DSL-Configuration-Tasks-After)

## Associate Git Branches With Environments

Since we have two server environments (staging and production) it is best practice to use branches in Git to these environments, so that you can deploy staging branch to staging environment and master branch to production automatically. add the following line to your production.rb:
```
set :branch, 'production'
```

And the following line to staging.rb:
```
set :branch, 'staging'
```
Now the **_cap deploy_** command will deploy code from the staging branch since staging is our default environment. If you run _**cap production deploy**_ will deploy code from the master branch. 


