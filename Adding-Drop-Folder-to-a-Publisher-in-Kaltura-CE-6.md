After Creating a Publisher in Kaltura 

Creat a Transcoding Profile ( I have created in my name as Babin Lonston's transcoding 

Then Configure DropBox for Publisher by choosing configure in Drop menu

And Tick the check box, Content Ingestion - Drop Folder/s (config)

Then Click on configure and change the settings.

Note your Publisher ID from user's list ( My Publisher ID 106) and Create using Type : Local

Drop Folder Name: our Wish ( Here i have used babindrop)

Description: As our Wish (This is Babin Lonston's Drop)

Conversion Profile ID: Choose your created name Here from Drop list

Drop Folder Storage Path: /opt/kaltura/web/content/arrivudrop (or) any folder name

Check file size every (seconds): 10

Save it ... that't it in KMC side..

-------------------------------------------------------------------------

Then in Terminal 

Create a directory named as you have mentioned here (Drop Folder Storage Path: /opt/kaltura/web/content/arrivudrop)

```
Eg : mkdir /opt/kaltura/web/content/arrivudrop
```
Then add a user for FTP
```
# useradd -d /opt/kaltura/web/content/arrivudrop arrivudrop  ( home page of this arrivudrop user is /opt/kaltura/web/content/arrivudrop )
```
(skel file error will be display, we don't need a bash profile so don't mind the error)

```
# passwd arrivudrop

New passwd: ********
Con Passwd: ******
```

Add the user arrivdrop to ftp & kaltura Group
```
# usermod -a -G ftp,kaltura arrivudrop

```
Navigate to directory 
```
# cd /opt/kaltura/web/content
```

Change the Ownership if arrivudrop
```
# chown arrivudrop:kaltura arrivudrop/

```
Restart the ftp server
```
# /etc/init.d/vsftpd restart
```
Login from filezilla 

And upload a video file, it will be uploaded to /opt/kaltura/web/content/arrivudrop

After Completing upload it wait's for 10 seconds and it will move to KMC Content TAB and Start to convert it Using Transcoding profile Which we have created.

That's it ..