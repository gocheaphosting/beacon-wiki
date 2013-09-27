    $heroku login
    $heroku keys:add
        $heroku keys
        $heroku keys:remove <key name>
    Still having problem
        remove the ssh directory
            $rm -rf ~/.ssh
        Then continue step 2
          eval `ssh-agent -s`
         ssh-add ~/.ssh/id_rsa
    $git push heroku master 