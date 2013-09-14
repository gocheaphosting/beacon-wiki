How to install BigBlueButton 0.81 in Ubuntu 10.04 Server


Before You Install


These instructions show how to install BigBlueButton 0.81-beta-2 (referred hereafter as 0.81).

```
Note: The packaging is for Ubuntu 10.04 64-bit only. There is no 32-bit packaging yet for 0.81.
```


The requirements are


```

Ubuntu 10.04 64-bit
4 GB of memory (8 GB is better)
Quad-core 2.6 GHZ CPU (or faster)
Ports 80, 1935, 9123 accessible
Port 80 is not used by another application
500G of free disk space (or more) for recordings

```


In addition to the above, the locale of the server must be en_US.UTF-8. Furthermore, the contents of /etc/default/locale must contain the single line LANG="en_US.UTF-8". You can verify this as below.


```

#cat /etc/default/locale
LANG="en_US.UTF-8"

```



![1](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_001.png)



If you don't see LANG="en_US.UTF-8", then do the following


```


# sudo apt-get install language-pack-en
# sudo update-locale LANG=en_US.UTF-8


```


and the logout and log back into your session. After the above, do cat /etc/default/locale again and verify you see only the single line LANG="en_US.UTF-8". 

Note: if you see an additional line LC_ALL=en_US.UTF-8, then remove the setting for LC_ALL before continuing.

Also, check that you are running under 64-bit.



```

#  uname -m
   x86_64

```


![2](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_002.png)



You should see x86_64.




Installing BigBlueButton 0.81



These instructions assume you do not have a previous version of BigBlueButton installed.




1. Update your server


The following steps will install 0.81.

You first need to give your server access to the BigBlueButton package repository.

In a terminal window, copy and paste the following commands.




```

# Add the BigBlueButton key
wget http://ubuntu.bigbluebutton.org/bigbluebutton.asc -O- | sudo apt-key add -

# Add the BigBlueButton repository URL and ensure the multiverse is enabled
echo "deb http://ubuntu.bigbluebutton.org/lucid_dev_081/ bigbluebutton-lucid main" | sudo tee /etc/apt/sources.list.d/bigbluebutton.list


```

Adding key


![3](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_003.png)


Adding Repo List 


![4](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_004.png)



Next, ensure that you have lucid multiverse in your sources.list. Do the following.


```

# grep "lucid multiverse" /etc/apt/sources.list


```



![5](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_005.png)





If you have the lucid multiverse in your sources.list, you should see


```

#  deb http://us.archive.ubuntu.com/ubuntu/ lucid multiverse


```


If you don't see the deb line for lucid multiverse, execute the following line to add this repository to sources.list.


```

echo "deb http://us.archive.ubuntu.com/ubuntu/ lucid multiverse" | sudo tee -a /etc/apt/sources.list


```


Before proceeding further, do a dist-upgrade to ensure all the current packages on your server are up-to-date.


```


# sudo apt-get update
# sudo apt-get dist-upgrade


```



![6](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_006.png)




If you've not updated in a while, apt-get may recommend you reboot your server after dist-upgrade finishes. Do the reboot before proceeding to the next step.




2. Install LibreOffice



BigBlueButton uses LibreOffice to convert uploaded MS office documents to PDF. LibreOffice does a far better job of converting documents than the default OpenOffice packages in Ubuntu 10.04.

First, we'll install a stub package for openoffice. This will serve as a placeholder for BigBlueButton's dependency on OpenOffice..


```

# wget http://bigbluebutton.googlecode.com/files/openoffice.org_1.0.4_all.deb
sudo dpkg -i openoffice.org_1.0.4_all.deb


```


![8](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_008.png)



If you get an error in the above, check if you have openoffice.org-core installed. If so, remove all the existing openoffice.org pacakges and try to install the above stub package again.


Next, we'll install LibreOffice



```

# sudo apt-get install python-software-properties

# sudo apt-add-repository ppa:libreoffice/libreoffice-4-0
# sudo apt-get update

# sudo apt-get install libreoffice-common
# sudo apt-get install libreoffice


```




![10](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_010.png)




3. Install Ruby


The record and playback infrastructure uses Ruby for the processing of recorded sessions.

Check if you have a previous version of ruby install.




```

# dpkg -l | grep ruby


```



![11](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_011.png)




If you already have ruby installed, check it's version



```

#  ruby -v
ruby 1.9.2p290 (2011-07-09 revision 32553)


```


![12](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_012.png)




If the version of ruby does not match the above, uninstall it before continuing.

Download the following pre-build ruby package.



```

# wget https://bigbluebutton.googlecode.com/files/ruby1.9.2_1.9.2-p290-1_amd64.deb


```



This next install command will give you an error about dependencies not found.




```

# sudo dpkg -i ruby1.9.2_1.9.2-p290-1_amd64.deb


```


To resolve the dependencies, enter  



```


# sudo apt-get install -f



```


After the package installs, run the following two commands to setup the paths to the ruby executable.



```


sudo update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby1.9.2 500 \
                         --slave /usr/bin/ri ri /usr/bin/ri1.9.2 \
                         --slave /usr/bin/irb irb /usr/bin/irb1.9.2 \
                         --slave /usr/bin/erb erb /usr/bin/erb1.9.2 \
                         --slave /usr/bin/rdoc rdoc /usr/bin/rdoc1.9.2
sudo update-alternatives --install /usr/bin/gem gem /usr/bin/gem1.9.2 500


```



![15](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_015.png)




Run the following command to check that ruby is now installed.




```

# ruby -v
ruby 1.9.2p290 (2011-07-09 revision 32553)


```



![16](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_016.png)



And that gem is now installed.



```


# gem -v
1.3.7


```


Finally, to make sure you can install gems, run the commands below to install a test gem. (BigBlueButton does not need the gem hello; rather, we're just testing to makes sure gem is working properly).



```

# sudo gem install hello
Successfully installed hello-0.0.1
1 gem installed
Installing ri documentation for hello-0.0.1...
Installing RDoc documentation for hello-0.0.1...

```





![17](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_017.png)





Make sure you can execute the above three commands without errors before continuing with these instructions.



You might be wondering why not use the default Ruby packages for Ubuntu 10.04? Unfortunately, they are out of date.



4. Install ffmpeg



BigBlueButton uses ffmpeg to process video files for playback. To install ffmpeg, create a file called { install-ffmpeg.sh } and copy and paste in the following script.




```

sudo apt-get install build-essential git-core checkinstall yasm texi2html libvorbis-dev libx11-dev libxfixes-dev zlib1g-dev

LIBVPX_VERSION=1.2.0
FFMPEG_VERSION=0.11.3

if [ ! -d "/usr/local/src/libvpx-${LIBVPX_VERSION}" ]; then
  cd /usr/local/src
  sudo git clone http://git.chromium.org/webm/libvpx.git "libvpx-${LIBVPX_VERSION}"
  cd "libvpx-${LIBVPX_VERSION}"
  sudo git checkout "v${LIBVPX_VERSION}"
  sudo ./configure
  sudo make
  sudo checkinstall --pkgname=libvpx --pkgversion="${LIBVPX_VERSION}" --backup=no --deldoc=yes --default
fi

if [ ! -d "/usr/local/src/ffmpeg-${FFMPEG_VERSION}" ]; then
  cd /usr/local/src
  sudo wget "http://ffmpeg.org/releases/ffmpeg-${FFMPEG_VERSION}.tar.bz2"
  sudo tar -xjf "ffmpeg-${FFMPEG_VERSION}.tar.bz2"
  cd "ffmpeg-${FFMPEG_VERSION}"
  sudo ./configure --enable-version3 --enable-postproc --enable-libvorbis --enable-libvpx
  sudo make
  sudo checkinstall --pkgname=ffmpeg --pkgversion="5:${FFMPEG_VERSION}" --backup=no --deldoc=yes --default
fi

```



Next, run the commands



```

chmod +x install-ffmpeg.sh
./install-ffmpeg.sh


```




![18](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_018.png)




After the script finishes, check that ffmepg is installed by typing the command ffmpeg -version. You should see the following.




```

# ffmpeg -version
ffmpeg version 0.11.3
built on Jul 26 2013 01:34:22 with gcc 4.4.3
configuration: --enable-version3 --enable-postproc --enable-libvorbis --enable-libvpx
libavutil      51. 54.100 / 51. 54.100
libavcodec     54. 23.100 / 54. 23.100
libavformat    54.  6.100 / 54.  6.100
libavdevice    54.  0.100 / 54.  0.100
libavfilter     2. 77.100 /  2. 77.100
libswscale      2.  1.100 /  2.  1.100
libswresample   0. 15.100 /  0. 15.100

```




![19](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_019.png)





5. Install BigBlueButton



We're now ready to install BigblueButton. Type


```

# sudo apt-get install bigbluebutton


```



![20](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_020.png)





This single command is where all the magic happens. This command installs all of BigBlueButton components with their dependencies. The packaging will do all the work for you to install and configure your BigBlueButton server.



If you get an error message



```

...... Error: FreeSWITCH didn't start 


```


you can ignore it as we'll do a clean restart of all the components in step 7.



6. Install API Demos


To interactively test your BigBlueButton server, you can install a set of API demos.




```


# sudo apt-get install bbb-demo


```



![21](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_021.png)




You'll need the bbb-demo package installed if you want to join the Demo Meeting from your BigBlueButton server's welcome page.


Later on, if you wish to remove the API demos, you can enter the command




```

# sudo apt-get purge bbb-demo


```


7. Do a Clean Restart



To ensure BigBlueButton has started cleanly, enter the following commands:



```

# sudo bbb-conf --clean
# sudo bbb-conf --check


```



![23](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_023.png)




The --clean option will clear out all the log files for BigBlueButton. The --check option will grep through the log files looking for errors.

The output from sudo bbb-conf --check will display your current settings and, after the text, " Potential problems described below ", print any potential configuration or startup problems it has detected.


Finished installing and we will get this message if its Installed Correctly ...



![24](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_024.png)




Then Open the Website Using Our IP were we have installed Below Image Shows ..




![25](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_025.png)





After that Login using Demo and Check Weather Working Properly 




![26](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_026.png)






***
***






Upgrading BigBlueButton 0.81



If you have already installed an earlier version of 0.81, to upgrade to the latest version



```

# sudo apt-get update
# sudo apt-get dist-upgrade

# sudo bbb-conf --clean
# sudo bbb-conf --check


```



***
***




### Development in Bigbluebutton to change some modifications


1 .To Remove the Language Selection Option Shown in this Image Below , Navigate to the Dir





![27](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_027.png)






```

# /var/www/bigbluebutton/client/conf


```


And edit the file config.xml



```

# vim config.xml


```




![28](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_028.png)





Change the Line 9 



```

# <language userSelectionEnabled="true" /> to <language userSelectionEnabled="false" />


```




![29](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_029.png)





2. Then Once Restart the All Server Means have to Clean Using Command 



```


# sudo bbb-conf --clean



```




![30](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_030.png)






Now See the image as Below After removal 






![31](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_031.png)






3. To Remove the Right Side Message , Navigate to the Folder 



```

 
# cd /var/lib/tomcat6/webapps/bigbluebutton/WEB-INF/classes


```



![32](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_032.png)




Edit the file bigbluebutton.properties using sudo



```

# sudo vim bigbluebutton.properties


```




![33](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_033.png)





Then remove the Content in Lines 79 & 80 It want to be as Shown below here .
 



```

defaultWelcomeMessage=
defaultWelcomeMessageFooter=

```



![34](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_034.png)




Then Restart as Above Step 2.


Here in Below Image u can see the demo Messages are removed




![35](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_035.png)




4. To change the Default Introduction slide Navigate to Folder 



```

Replace /var/www/bigbluebutton-default/default.pdf by editing default.ppt


```



![36](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_036.png)




Need to Convert the PPT File to PDF after editing after that upload it in /var/www/bigbluebutton-default/ Directory



5. Change video Record and Playback logo Navigate to the Directory



```


# /var/bigbluebutton/playback/presentation/


```

Replace /var/bigbluebutton/playback/presentation/logo.png with new logo of same size





![37](https://github.com/babinlonston/Ubuntu-Linux-Stuffs/raw/master/Installing%20BBB0.081%20And%20Modification/Selection_037.png)




6. To Change the Bottom footer Message and HREF 



Setup Development Environment As Said in Below Link 


```
https://code.google.com/p/bigbluebutton/wiki/081DevelopingBigBlueButton

```


### Method 1


To change footer


```
cd ~/dev/bigbluebutton/bigbluebutton-client

```


Edit the Files 


```
vi src/org/bigbluebutton/main/views/MainApplicationShell.mxml

```

Add the content needed 


```

<mx:Text htmlText="(c) 2013 blablabla blablabla - For more information visit &lt;a href='event:http://www.blablabla.com' &gt;http://www.blablabla.com&lt;/a&gt;" id="copyrightLabel2"/>

```

Then Move to the Directory 



```

cd ~/dev/bigbluebutton/bigbluebutton-client


```


Then Execute the command 



```

# ant


```


After that copy all SWF files from 



```

# ~/dev/bigbluebutton/bigbluebutton-client/client to /var/www/bigbluebutton/client/


```


Then follow the step 2 to Clean and restart 

Then Check out everything will be fine working ...



### Method 2 


OR if you Have the Already modified BBB  


To Change the Title From Bigbluebutton demo Web Conference to Our Wish 
Navigate to this Directory 


```

# /var/www/bigbluebutton/client

```

And Edit the File


```

# sudo vim BigBlueButton.htm

```


And Change the Line 4 To Our title 
In Line number IV 



title   your title here  title



Then Edit the the File inside Directory


```

cd /var/www/bigbluebutton/client

```


Edit this file 



```

# vim bbb_blinker.js

```


And Change this in 2nd line 



```

function setTitle(title){
    document.title="Cosmic";

```


To Change the Footer 
In Dev Folder 

In Line Number 421 change the Footer Web Address are remove it 


```

# /dev/bigbluebutton/bigbluebutton-client/src/org/bigbluebutton/main/views
# sudo vim MainApplicationShell.mxml


```


Then come Back to the bigbluebutton-client Folder and Give the Command 


```

# ant

```

If ant command gives error user command 



```

# source ~/.profile


```

Then try to Run the and command once more it will generate the development after that copy all Swf files and replace it to change the Footer 


To setip use command 


```

# sudo bbb-conf --setip your clent ip here 

# sudo bbb-conf --setip 75.141.187.101

```


Do a Clean Check 


```

# sudo bbb-conf --clean

```

And Check 


```

sudo bbb-conf --check

```



