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

## mount the hgfs in /etc/fstab
* .host:/share        /mnt/hgfs       vmhgfs     defaults 0 0
  
## mount the hgfs in /etc/init.d/rc.local
* mount.vmhgfs .host:/share /mnt/hgfs
* mount -t vmhgfs .host:/share /mnt/hgfs

**but when I use ubuntu16.04,I test it as below:**
* sudo mkdir -m 777 /mnt/hgfs/share
* sudo vmhgfs-fuse .host:/SHARE /mnt/hgfs/share -o subtype=vmhgfs-fuse,allow_other

**It's not ok as below:**
* sudo vmhgfs-fuse .host:/ /mnt/hgfs/share -o subtype=vmhgfs-fuse,allow_other

So I think mount the file to /mnt/hgfs is not OK.

You could use **umount /mnt/hgfs** umount the mountpoint.

when you use the ubuntu16.04. the vmware tool is too old. will generate
the new question.
Q:
```
 CC [M]  /tmp/modconfig-O5xscq/vmhgfs-only/module.o
/tmp/modconfig-O5xscq/vmhgfs-only/dir.c: In function ‘HgfsPackDirOpenRequest’:
/tmp/modconfig-O5xscq/vmhgfs-only/dir.c:417:26: error: ‘struct file’ has no member named ‘f_dentry’
                      file->f_dentry) < 0) {
                          ^
/tmp/modconfig-O5xscq/vmhgfs-only/dir.c: In function ‘HgfsDirLlseek’:
/tmp/modconfig-O5xscq/vmhgfs-only/dir.c:707:32: error: ‘struct file’ has no member named ‘f_dentry’
    struct dentry *dentry = file->f_dentry;
                                ^
```

A:
Please ensure that latest Workstation 11 is installed.
Make sure open-vm-tools is not installed.
sudo apt-get remove open-vm-tools

1.Make sure the updates are done:
sudo apt-get update

2.Make sure Git is installed
sudo apt-get install git

3.Run the command to get the tools from repository.
sudo git clone https://github.com/rasa/vmware-tools-patches.git
or
sudo git clone https://github.com/rasa/vmware-tools-patches

4.cd to vmware-tools-folder
cd vmware-tools-patches

5.Run the patch
sudo ./download-tools.sh

6.Run the following patch
sudo ./untar-and-patch.sh

7.Run the complie.sh file
sudo ./compile.sh

##when update vim, add the gnome and gtk2.0
the vmware adjust screen will be not used.because vmware can not change 
the screen display's resolustion.

if you add new resolution ,you could do as below:
sudo xrandr --newmode "1920x1080_60.00" 173.00 1920 2048 2248 2576 1080 1083 1088 1120 -hsync +vsync
sudo xrandr --addmode Virtual1 "1920x1080_60.00"

# boot function 
1. edit rc.local
update before exit 0
locate: /etc/rc.local

2. 添加一个Ubuntu的开机启动服务
如果要添加为开机启动执行的脚本文件，
可先将脚本复制或者软连接到/etc/init.d/目录下，
然后用：update-rc.d xxx defaults NN命令(NN为启动顺序)，
将脚本添加到初始化执行的队列中去。
注意如果脚本需要用到网络，则NN需设置一个比较大的数字，如99。
1) 将你的启动脚本复制到 /etc/init.d目录下
 以下假设你的脚本文件名为 test。
2) 设置脚本文件的权限
$ sudo chmod 755 /etc/init.d/test
3) 执行如下命令将脚本放到启动脚本中去：
$ cd /etc/init.d
$ sudo update-rc.d test defaults 95
注：其中数字95是脚本启动的顺序号，按照自己的需要相应修改即可。在你有多个启动脚本，而它们之间又有先后启动的依赖关系时你就知道这个数字的具体作用了。该命令的输出信息参考如下：
```
update-rc.d: warning: /etc/init.d/test missing LSB information
update-rc.d: see <http://wiki.debian.org/LSBInitScripts>
Adding system startup for /etc/init.d/test ...
/etc/rc0.d/K95test -> ../init.d/test
/etc/rc1.d/K95test -> ../init.d/test
/etc/rc6.d/K95test -> ../init.d/test
/etc/rc2.d/S95test -> ../init.d/test
/etc/rc3.d/S95test -> ../init.d/test
/etc/rc4.d/S95test -> ../init.d/test
/etc/rc5.d/S95test -> ../init.d/test
```
卸载启动脚本的方法：
$ cd /etc/init.d
$ sudo update-rc.d -f test remove
