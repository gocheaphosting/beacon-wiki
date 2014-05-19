### Setting the Virtual Machines to Auto-Start

Gather the UUIDs of the Virtual Machine you want to auto-start by running 

```
xe vm-list
```

Note: This generates a list of Virtual Machines in your pool or server and their associated UUIDs.

Copy the UUID of the Virtual Machines you want to auto-start, and run the following command for each Virtual Machine to auto-start:


```
xe vm-param-set uuid=UUID other-config:auto_poweron=true
```

Note: Replace UUID with the UUID of the Virtual Machine to auto-start.