
###Basic Requirements 

	-Ubuntu 10.04 64-bit
	-4 GB of memory (8 GB is better)
	-Quad-core 2.6 GHZ CPU (or faster)
	-Ports 80, 1935, 9123 accessible
	-Port 80 is not used by another application
	-500G of free disk space (or more) for recordings

	In addition to the above, the locale of the server must be en_US.UTF-8. Furthermore, the contents of /etc/default/locale must contain the single line LANG="en_US.UTF-8". You can verify this as below.

```
	$ cat /etc/default/locale
	LANG="en_US.UTF-8"

```

If you don't see LANG="en_US.UTF-8", then do the following

```
sudo apt-get install language-pack-en
sudo update-locale LANG=en_US.UTF-8
```

and the logout and log back into your session. After the above, do 

```
      cat /etc/default/locale 

```
again and verify you see only the single line LANG="en_US.UTF-8". Note: if you see an additional line LC_ALL=en_US.UTF-8, then remove the setting for LC_ALL before continuing.



Also, check that you are running under 64-bit

	$ uname -m
	  x86_64



###Installing BigBlueButton 0.81


###1.Update your server


In a terminal window, copy and paste the following commands.
```
#Add the BigBlueButton key
wget http://ubuntu.bigbluebutton.org/bigbluebutton.asc -O- | sudo apt-key add -
# Add the BigBlueButton repository URL and ensure the multiverse is enabled
echo "deb http://ubuntu.bigbluebutton.org/lucid_dev_081/ bigbluebutton-lucid main" | sudo tee /etc/apt/sources.list.d/bigbluebutton.list
```

Next, ensure that you have lucid multiverse in your sources.list. Do the following.


$ grep "lucid multiverse" /etc/apt/sources.list


If you have the lucid multiverse in your sources.list, you should see


deb http://us.archive.ubuntu.com/ubuntu/ lucid multiverse

If you don't see the deb line for lucid multiverse, execute the following line to add this repository to sources.list.


echo "deb http://us.archive.ubuntu.com/ubuntu/ lucid multiverse" | sudo tee -a /etc/apt/sources.list

Before proceeding further, do a dist-upgrade to ensure all the current packages on your server are up-to-date.


sudo apt-get update
sudo apt-get dist-upgrade


If you've not updated in a while, apt-get may recommend you reboot your server after dist-upgrade finishes. Do the reboot before proceeding to the next step.

####2. Install LibreOffice


wget http://bigbluebutton.googlecode.com/files/openoffice.org_1.0.4_all.deb
sudo dpkg -i openoffice.org_1.0.4_all.deb

If you get an error in the above, check if you have openoffice.org-core installed. If so, remove all the existing openoffice.org pacakges and try to install the above stub package again.

Next, we'll install LibreOffice

sudo apt-get install python-software-properties

sudo apt-add-repository ppa:libreoffice/libreoffice-4-0


sudo apt-get update

sudo apt-get install libreoffice-common

sudo apt-get install libreoffice



####3. Install Ruby

The record and playback infrastructure uses Ruby for the processing of recorded sessions.

Check if you have a previous version of ruby install.

   dpkg -l | grep ruby

If you already have ruby installed, check it's version

$ ruby -v

ruby 1.9.2p290 (2011-07-09 revision 32553)

If the version of ruby does not match the above, uninstall it before continuing.

Download the following pre-build ruby package.

wget https://bigbluebutton.googlecode.com/files/ruby1.9.2_1.9.2-p290-1_amd64.deb
This next install command will give you an error about dependencies not found.

  sudo dpkg -i ruby1.9.2_1.9.2-p290-1_amd64.deb
To resolve the dependencies, enter

  sudo apt-get install -f
After the package installs, run the following two commands to setup the paths to the ruby executable.

sudo update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby1.9.2 500 \
                         --slave /usr/bin/ri ri /usr/bin/ri1.9.2 \
                         --slave /usr/bin/irb irb /usr/bin/irb1.9.2 \
                         --slave /usr/bin/erb erb /usr/bin/erb1.9.2 \
                         --slave /usr/bin/rdoc rdoc /usr/bin/rdoc1.9.2
sudo update-alternatives --install /usr/bin/gem gem /usr/bin/gem1.9.2 500

Run the following command to check that ruby is now installed.

$ ruby -v
ruby 1.9.2p290 (2011-07-09 revision 32553)

And that gem is now installed.

$ gem -v
1.3.7

Finally, to make sure you can install gems, run the commands below to install a test gem. (BigBlueButton does not need the gem hello; rather, we're just testing to makes sure gem is working properly).

$ sudo gem install hello
Successfully installed hello-0.0.1
1 gem installed
Installing ri documentation for hello-0.0.1...
Installing RDoc documentation for hello-0.0.1...
Make sure you can execute the above three commands without errors before continuing with these instructions. If you do encounter errors, please post to bigbluebutton-setup and we'll help you resolve the errors.

You might be wondering why not use the default Ruby packages for Ubuntu 10.04? Unfortunately, they are out of date. Thanks to Brendan Ribera for the above script for installing the latest ruby on Ubuntu 10.04 as a package.

4. Install ffmpeg


To install ffmpeg, create a file called install-ffmpeg.sh and copy and paste in the following script.

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
Next, run the commands

chmod +x install-ffmpeg.sh
./install-ffmpeg.sh
After the script finishes, check that ffmepg is installed by typing the command ffmpeg -version. You should see the following
```

$ ffmpeg -version
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
5. Install BigBlueButton
We're now ready to install BigblueButton. Type

   sudo apt-get install bigbluebutton
This single command is where all the magic happens. This command installs all of BigBlueButton components with their dependencies. The packaging will do all the work for you to install and configure your BigBlueButton server.

If you are behind a HTTP Proxy, you will get an error from the package bbb-record-core. You can resolve this by manually installing the gems.

If you get an error message

...... Error: FreeSWITCH didn't start 
you can ignore it as we'll do a clean restart of all the components in step 7.


6. Install API Demos
To interactively test your BigBlueButton server, you can install a set of API demos.

   sudo apt-get install bbb-demo
You'll need the bbb-demo package installed if you want to join the Demo Meeting from your BigBlueButton server's welcome page. This is the same welcome page you see at dev081 demo server.

Later on, if you wish to remove the API demos, you can enter the command

   sudo apt-get purge bbb-demo


7. Do a Clean Restart
To ensure BigBlueButton has started cleanly, enter the following commands:

   sudo bbb-conf --clean
   sudo bbb-conf --check

The --clean option will clear out all the log files for BigBlueButton. The --check option will grep through the log files looking for errors.

	The output from sudo bbb-conf --check will display your current settings and, after the text, " Potential problems described below ", print any potential configuration or startup problems it has detected.


