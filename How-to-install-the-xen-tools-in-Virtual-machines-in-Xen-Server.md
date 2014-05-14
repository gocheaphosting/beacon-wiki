For Installing the xen tool we need to choose the virtual machine from left pane which we need to isntall

And click on install the xen tools

Then nagivate to console

```
# cd /mnt
```

Make a Directory

```
# mkdir xs-tools
```

Then Mount the tools from xvdd to mnt/tools Using below command 

```
mount /dev/xvdd /mnt/xs-tools/
```

Navigate to the Directory


```
cd /mnt/xs-tools/Linux/
```

Install the Xen tools using 


```
bash install.sh 
```
