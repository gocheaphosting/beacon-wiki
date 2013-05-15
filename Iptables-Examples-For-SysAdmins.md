###1: Displaying the Status of Your Firewall

```
iptables -L -n -v
```
Where,

-L : List rules.
-v : Display detailed information. This option makes the list command show the interface name, the rule options, and the TOS masks. The packet and byte counters are also listed, with the suffix 'K', 'M' or 'G' for 1000, 1,000,000 and 1,000,000,000 multipliers respectively.
-n : Display IP address and port in numeric format. Do not use DNS to resolve names. This will speed up listing.


more details at https://www.linode.com/wiki/index.php/Configuring_IPtables_on_ubuntu_server
