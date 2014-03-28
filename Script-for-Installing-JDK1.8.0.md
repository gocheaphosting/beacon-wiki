```
#!/bin/sh 
set -x

wget ftp://192.168.0.15/arrivu_contents/jdk-8-linux-x64.tar.gz

java -version

sudo mv jdk-8-linux-x64.tar.gz /usr/lib/jvm

cd /usr/lib/jvm

sudo tar -zxvf jdk-8-linux-x64.tar.gz

sudo rm jdk-8-linux-x64.tar.gz

ls -l

sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0/bin/javac" 1

sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0/bin/java" 1

sudo update-alternatives --set "javac" "/usr/lib/jvm/jdk1.8.0/bin/javac"

sudo update-alternatives --set "java" "/usr/lib/jvm/jdk1.8.0/bin/java"

echo "JAVA_HOME=/usr/lib/jvm/jdk1.8.0 PATH=$PATH:$JAVA_HOME/bin export JAVA_HOME export PATH" > /etc/profile

#Add the following entries to the bottom of your /etc/profile file:

. /etc/profile

java -version

exit
```