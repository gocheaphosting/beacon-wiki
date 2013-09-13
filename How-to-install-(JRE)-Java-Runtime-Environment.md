How to install (JRE) Java Runtime Environment 


Download the 32bit or 64bit Linux "compressed binary file" - it has a ".tar.gz" file extension and uncompress it


Download JRE from Here 

```

# http://www.oracle.com/technetwork/java/javase/downloads/index.html

```


From Download Location we need to move the JRE tar package to the Directory 


```

# #sudo mv jre-7u40-linux-x64.tar.gz /usr/lib/jvm 

```

Navigate to the Directory 


```

#cd /usr/lib/jvm

```


Then uncompress it 


```

#sudo tar -zxvf jre-7u40-linux-x64.tar.gz

```


Then if we need to remove the tar file use below command 


```

#sudo rm jdk-7u21-linux-i586.tar.gz

```

List the files 


```

# ls -l

```


Open the directory 


```

# cd jre1.7.0_40

```


Afterwards run the following to get a list of currently installed java alternatives



```

# sudo update-alternatives --config java

```

We will get   



```

There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java   1071      auto mode
  1            /usr/lib/jvm/java-7-openjdk-amd64/jre/bin/java   1071      manual mode
  2            /usr/lib/jvm/jdk1.7.0_40/bin/java                1         manual mode
  3            /usr/lib/jvm/jre1.7.0_40/bin/java                0         manual mode

Press enter to keep the current choice[*], or type selection number: 3



```

Select Zero For Auto Mode 
Here i have selected 0 


Note : > if there was no previous java installation then the new JRE will be the default and you will not see the above.


```

sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jre1.7.0_40/bin 0

```


This will add your new JRE 7 installation into alternatives list i.e. use the remembered number + 1 i.e. 3 in the example above. Now configure java to use the Oracle Java JRE


Check the version of you new JRE 7 installation


```

#java -version

```

Output Will be like this 


```

java version "1.7.0_25"
OpenJDK Runtime Environment (IcedTea 2.3.10) (7u25-2.3.10-1ubuntu0.13.04.2)
OpenJDK 64-Bit Server VM (build 23.7-b01, mixed mode)

```



Or use the Step Below to get easier 



```
#java -version

#sudo mkdir -p /usr/lib/jvm

#sudo mv jre-7u40-linux-x64.tar.gz /usr/lib/jvm

#cd /usr/lib/jvm

#sudo tar zxvf jre-7u40-linux-x64.tar.gz

#sudo rm jre-7u40-linux-x64.tar.gz

#ls -l

#jre1.7.0_40

#sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jre1.7.0_40/bin/javac" 3

#sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jre1.7.0_40/bin/java" 3

#sudo update-alternatives --set "javac" "/usr/lib/jvm/jre1.7.0_40/bin/javac"

#sudo update-alternatives --set "java" "/usr/lib/jvm/jre1.7.0_40/bin/java"

#sudo vi /etc/profile

Add the following entries to the bottom of your /etc/profile file:

#JAVA_HOME=/usr/lib/jvm/jre1.7.0_40 PATH=$PATH:$JAVA_HOME/bin export JAVA_HOME export PATH

#. /etc/profile

#java -version

```


After this we need to Add the Jre Plugins in Google chorme and mozilla For that 


Follow these instructions to enable Java in your web browser on Ubuntu Linux.

Google Chrome

Become the root user by running the su command and then enter the super-user password. Type: 


```
# sudo -s

```
Create a directory called plugins if you do not have it. Type: 


```
# mkdir -p /opt/google/chrome/plugins

```

Go to Google chrome plugins directory before you make the symbolic link. Type: 


```
# cd /opt/google/chrome/plugins

```

Create a symbolic link. Type: 


```

# ln -s /usr/lib/jvm/jre1.7.0_40/lib/amd64/libnpjp2.so


```


Restart your browser and test Java Here in Below Link 


```

# http://www.java.com/en/download/testjava.jsp

```


Mozilla Firefox

Become the root user by running the su command and then enter the super-user password. Type:


```
 
# sudo -s

```


Create a directory called plugins if you do not have it. Type: 


```

# mkdir -p /usr/lib/mozilla/plugins


```

Go to Google chrome plugins directory before you make the symbolic link. Type: 


```

# cd /usr/lib/mozilla/plugins

```

Create a symbolic link. Type: 


```

# ln -s /usr/lib/jvm/jre1.7.0_40/lib/amd64/libnpjp2.so


```


Restart your browser and test Java in below link


```

# http://www.java.com/en/download/testjava.jsp

```



***
