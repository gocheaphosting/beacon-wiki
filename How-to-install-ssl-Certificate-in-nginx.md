How to install ssl Certificate in nginx

Here we can see how to install the certificate file for nginx, I Have created ssl certitficate and sended to comodo
And i got the crt files in mail, so here is the step to install the certificate what i have recived in the mail

Login to the Server for settingup using ssh

```
# ssh root@192.168.1.200

```

Navigate to the nginx Directory 
Here i have installed nginx in opt/nginx

```
# cd /opt/nginx/

```
Here we can see ssl/ directory and site-enabled & site-available 

Navigate to ssl Directory

```
# cd /opt/nginx/ssl/

```
copy the zip file which has our ssl certificate what we recived in mail from comodo to this current Directory
and unzip it.

```
# unzip STAR_example_website_com.zip
```

Output:

```
wxr-xr-x 14 root root 4096 Jan 17 00:16 ..
-rw-r--r--  1 root root 1143 Jan 17 01:24 example_website_cert_nginx.csr
-rw-r--r--  1 root root 1751 Jan 17 01:11 example_website_cert_nginx.key
-rw-rw-rw-  1 root root 1793 Jan 18  2014 STAR_example_website_com.crt
-rw-r--r--  1 root root 5450 Jan 17 23:46 STAR_example_website_com.zip
```

we will get below files after extract, These are there files will be in zip file.

```
-rw-rw-rw-  1 root root 1521 May 30  2000 AddTrustExternalCARoot.crt
-rw-rw-rw-  1 root root 1757 Feb 16  2012 PositiveSSLCA2.crt
-rw-rw-rw-  1 root root 1793 Jan 18  2014 STAR_example_website_com.crt
```

Then Create the bundle file using cat command, we need to add 3 files in 

```
# cat STAR_example_website_com.crt PositiveSSLCA2.crt AddTrustExternalCARoot.crt > ssl-bundle.crt


-rw-r--r--  1 root root 5071 Jan 17 23:51 ssl-bundle.crt

```
Navigate to 

```
# /opt/nginx/sites-available
```
Edit the file

```
# vim example_website_ssl
```

add the ssl certification file's as below:

```
               ssl on;
               ssl_certificate /opt/nginx/ssl/ssl-bundle.crt;
               ssl_certificate_key /opt/nginx/ssl/example_website_cert_nginx.key;
               ssl_session_timeout  5m;
               #enables SSLv3/TLSv1, but not SSLv2 which is weak and should no longer be used.
	       ssl_protocols SSLv3 TLSv1;
               #Disables all weak ciphers
               ssl_ciphers ALL:!aNULL:!ADH:!eNULL:!LOW:!EXP:RC4+RSA:+HIGH:+MEDIUM;
```

Then Navigate to Site's Available

```
# cd /opt/nginx/sites-enabled/

```
Create a softlink of example_website_com_ssl to site available

```
# ln -s /opt/nginx/sites-available/example_website_com_ssl /opt/nginx/sites-enabled/example_website_com_ssl
```

Then Restart the nginx 

```
# /etc/init.d/nginx restart
Stopping nginx: Enter PEM pass phrase:
nginx.
Starting nginx: Enter PEM pass phrase:
nginx.

# /opt/nginx/sites-enabled# /etc/init.d/nginx status
Checking nginx: Running
nginx.
```

While Restarting it will ask for the pem file password, If we don't need to give the password we can remove the password by following method 
Remove the encryption from the RSA private key.
Take a Backup of original copy of certificate using cp command

```
# cp example_website_cert_nginx.key example_website_cert_nginx.key.original
```

Remove the Password Using 

```
# openssl rsa -in example_website_cert_nginx.key.original -out example_website_cert_nginx.key
```

Make sure the example_website_cert_nginx.key file is only readable by root.

```
# chmod 400 example_website_cert_nginx.key

-r--------  1 root root 1679 Jan 18 01:06 example_website_cert_nginx.key
```
Restart the Nginx 

```
# /opt/nginx/sites-enabled$ sudo /etc/init.d/nginx restart
Stopping nginx: nginx.
Starting nginx: nginx.
```

Check the Certificate Correctly installed using following website 

```
http://www.sslshopper.com/ssl-checker.html

```