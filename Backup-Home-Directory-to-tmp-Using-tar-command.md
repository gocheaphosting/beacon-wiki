```
#!/bin/sh
#  Backup home Directory to tmp Using tar command script.

# What to backup. 
backup_files="/home/sysadmin "

# Where to backup to.
dest="/tmp"

# Create archive filename.
day=$(date +%A)
hostname=$(hostname -s)
archive_file="$hostname-$day.tar.gz"

# Print start status message.
echo "Backing up $backup_files to $dest/$archive_file"
date
echo

# Backup the files using tar.
tar zcvf $dest/$archive_file $backup_files

# Print end status message.
echo
echo "Backup finished"
date

# Long listing of files in $dest to check file sizes.
ls -lh $dest

exit
```