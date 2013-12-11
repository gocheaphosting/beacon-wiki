```
#!/bin/bash
#Backup Script
#Description : Makes a Copy of Any Given file at the backup folder 
#Author      : Babin Lonston
#Date        : 11/12/2013
#How to Use  : if we need to backup every files inside a Directory use this command and choose the file then it will backup into backup folder

clear
echo "This is a Backup Script"


#Backup Folder; Set this variable to any folder you have Write Permissions on
#BACKUPFOLDER=~/backup

#The Script will make sure the folder exists
#mkdir -p $BACKUPFOLDER

#Now the Script will copy the given file to the folder 
#cp -a $@ $BACKUPFOLDER

#Request the backup folder from the user:

echo -e "\e[1m\e[32mFile Backup Utility\n\e[39m\e[0mPlease input your backup folder:"
read BACKUPFOLDER

#The script will make sure the folder exists

mkdir -p $BACKUPFOLDER

#Request files to be backed up:

echo -e "\e[47m\e[30mWhich files do you want backed up?\e[39m\e[49m"

read FILES

cp -a $FILES $BACKUPFOLDER

exit
```