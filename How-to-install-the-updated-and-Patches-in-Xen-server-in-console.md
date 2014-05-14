
1.) First we need to copy the patches and update to the xen server using winscp or filezilla or scp command from linux machines.

2.) Then we need to use the following command to upload the file to the xen server from local

```
xe patch-upload file-name=XS62ESP1.xsupdate
```

3.) To get the host UUID we need to Use the Below Command 

```
xe host-list
```

4.) To Apply the update Use the Update files UUID and Host UUID 


```
xe patch-apply uuid=0850b186-4d47-11e3-a720-001b2151a503 host-uuid=18c16c05-aebd-4b0f-88ce-363775af4d7e
```

5.) Reboot the Xen Server to get updated 


```
reboot
```