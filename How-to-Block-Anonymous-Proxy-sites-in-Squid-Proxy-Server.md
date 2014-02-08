Just only by Configuring Squid proxy Won't Block Every site's by using Blocked Site's
User can able to bypass the proxy using some sites 
So here we can see how to deny those Anonymous Access.

Issue may we can face:
1. Anonymous proxy sites by-pass proxy servers
2. Squid doesn’t show encoded part of the URL's.

Create a file named anon_sites

```
# vim /etc/squid/anon_sites.squid
```
Insert the Below Content to the Created file

```
browse.php?u=
```

Then edit the Configuration of Squid

```
# vi /etc/squid/squid.conf
```

Make a new ACL for anon_sites.squid.

```
########### Deny anon-proxy-sites ###########
#
acl anon-proxy-sites url_regex –i “/etc/squid/anon_sites.squid”
```
Create Deny access to this newly created acl.

```
########## Deny Access to Anon-Proxy sites ###########
#
http_access deny anon-proxy-sites

```
Save and Close squid.conf file Using wq!

Then Reload the Configuration of Suid Using 

```
# /etc/init.d/squid reload

```
We have done all the above steps, Now we have blocked thousands of anonymous proxy sites.

Monitor Reqularly the access.log of Squid to get the report whom else using proxy bypass.

```
# tail -f /var/log/squid/access.log
```

That's it
