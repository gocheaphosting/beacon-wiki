1.  Sign up in http://heroku.com 
1. Install heroku toolbet
1. $heroku create --stack cedar
1. $git push heroku master
1. $heroku run rake db:migrate
1. $heroku run rake db:seed
                  or
1.   $heroku run rake db:reset
1. $heroku run rake db:populate
1. heroku open
1. herku rename <Your app name >