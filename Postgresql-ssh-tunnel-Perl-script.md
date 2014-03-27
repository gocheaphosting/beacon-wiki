This will make a ssh tunnel for the PostgreSQL remote access from pg-admin

```
#!/usr/bin/perl

# PostgreSQL Tunnel Tool for Mac OS X and Linux
# Copyright (c) 2010 Linode, LLC
# Author: Philip C. Paradis <pparadis@linode.com>
# Usage: postgresql-tunnel.pl [start|stop]
# Access a PostgreSQL database server via an SSH tunnel.

$local_ip    = "127.0.0.1";
$local_port  = "5433";
$remote_ip   = "127.0.0.1";
$remote_port = "5432";
$remote_user = "sysadmin";
$remote_host = "192.168.56.15";
$remote_ssh_port = "22";

$a = shift;
$a =~ s/^\s+//;
$a =~ s/\s+$//;

$pid=`ps ax|grep ssh|grep $local_port|grep $remote_port`;
$pid =~ s/^\s+//;
@pids = split(/\n/,$pid);
foreach $pid (@pids)
{
 if ($pid =~ /ps ax/) { next; }
 split(/ /,$pid);
}

if (lc($a) eq "start")
{
 if ($_[0]) { print "Tunnel already running.\n"; exit 1; }
 else
 {
  system "ssh -p $remote_ssh_port -f -L $local_ip:$local_port:$remote_ip:$remote_port $remote_user\@$remote_host -N";
  exit 0;
 }
}
elsif (lc($a) eq "stop")
{
 if ($_[0]) { kill 9,$_[0]; exit 0; }
 else { exit 1; }
}
else
{
 print "Usage: postgresql-tunnel.pl [start|stop]\n";
 exit 1;
}

```