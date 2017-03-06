# use_ubuntu
log something when use ubuntu

# update vim

# install vm-tools after use vmware install ubuntu
1. mount vmware-tools
2. tar the vmare-tools'sCD to /tmp
3. run vmware-install.pl
4. update open-vm-tools-dkms
sudo apt-get install open-vm-tools-dkms
5. mount hgfs 

##mount the hgfs in /etc/fstab
* .host:/share        /mnt/hgfs       vmhgfs     defaults 0 0
  
##mount the hgfs in /etc/init.d/rc.local
* mount.vmhgfs .host:/share /mnt/hgfs
* mount -t vmhgfs .host:/share /mnt/hgfs

**but when I use ubuntu16.04,I test it as below:**
* sudo mkdir -m 777 /mnt/hgfs/share
* sudo vmhgfs-fuse .host:/SHARE /mnt/hgfs/share -o subtype=vmhgfs-fuse,allow_other

**It's not ok as below:**
* sudo vmhgfs-fuse .host:/ /mnt/hgfs/share -o subtype=vmhgfs-fuse,allow_other

So I think mount the file to /mnt/hgfs is not OK.

You could use **umount /mnt/hgfs** umount the mountpoint.
    
