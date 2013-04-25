- Download Oracle JDK tar file jdk-7u21-linux-x64.tar.gz from the URL http://www.oracle.com/technetwork/java/javase/downloads/index.html

sudo mkdir -p /usr/local/java

tar -xvf jdk-7u21-linux-x64.tar.gz

mv jdk1.7.0_21 /usr/local/java

cd /usr/local/java

sudo vi /etc/profile

-Add the following lines at the end of file:

JAVA_HOME=/usr/local/java/jdk*

PATH=$PATH:$HOME/bin:$JAVA_HOME/bin

JRE_HOME=/usr/local/java/jre*

PATH=$PATH:$HOME/bin:$JRE_HOME/bin

export JAVA_HOME

export JRE_HOME

export PATH

- Now enter following commands one by one in terminal:
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/local/java/jre1.7.0_21/bin/java" 1

sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/local/java/jdk1.7.0_21/bin/javac" 1

sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/local/java/jre1.7.0_21/bin/javaws" 1

sudo update-alternatives --set java /usr/local/java/jre1.7.0_21/bin/java

sudo update-alternatives --set javac /usr/local/java/jdk1.7.0_21/bin/javac

sudo update-alternatives --set javaws /usr/local/java/jre1.7.0_21/bin/javaws

. /etc/profile

-Create a Mozilla plugins folder in your home directory.

mkdir ~/.mozilla/plugins/

ln -s /usr/local/java/jre1.7.0_21/lib/amd64/libnpjp2.so ~/.mozilla/plugins/