## Boot from nfs
Put the root  filesystem on an host pc and serve it via a nfs server. This way we can setup a fast development cycle by compiling bigger projects on a fast host machine.  
  

### preparing the host pc

Using Ubuntu on the host machine.
 
Install nfs server with the following commands
```
sudo apt-get install nfs-kernel-server
```
restart the nfs service
```
sudo service nfs-kernel-server restart		
```
verify status nfs service
```
sudo service nfs-kernel-server status
```

A root filesystem must be available on the host machine. This can be done is several ways. The method we use is by copying it from the sd card written from an image. Find the right partition to copy from. Create a directory to copy to eg rootfs
```
sudo cp -ap /media/root/* rootfs
```

Next step it to export the rootfs. Add next line to /etc/exports
```
/home/rootfs *(rw,insecure,no_root_squash,no_subtree_check)
```
Restart the nfs service.  
To be sure the server is working we test it locally by
```
sudo mount -t nfs localhost:<directory>/rootfs /mnt
```
If all is ok we can access the rootfs by listing the /mnt directory.

We must edit the fstab in the rootfs. Else we get a locked system we cannot access en while booting the system will try to fix the disk. (todo: find more details about what is actual happening here)  
In the rootfs edit etc/fstab and comment both the lines for boot and root.
```
#PARTUUID=3ca8960f-01  /boot           vfat    defaults          0       2
#PARTUUID=3ca8960f-02  /               ext4    defaults,noatime  0       1
```


### preparing the rPi

Change the content of the cmdline.txt in the boot partition so it looks like
```
dwc_otg.lpm_enable=0 console=serial0,115200 console=tty1 ip=dhcp root=/dev/nfs nfsroot=192.168.2.13:<directory>/rootfs,vers=3 rw rootwait
```

*ip=dhcp* takes care of requesting a dynamic ip address for the pi.  
*root=/dev/nfs* tells pi where the root file system is  
*nfsroot=192.168.2.13:<directory>/rootfs/* is the ip for the nfs server host, and the directory is the location for the rootfs, this matches the line in the exports file on the host 

