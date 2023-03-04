 #### grubby is a utility for manipulating bootloader-specific configuration files. You can use grubby also for changing the default boot entry, and for adding/removing arguments from a GRUB2 menu entry.  The grubby command-line utility can be used to make persistent changes to the /boot/grub2/grub.cfg file. Modifying /boot/grub2/grub.cfg manually by vi is not recommended

.
 
 #### The GRUB2 arguments can be modified using 2 methods :

 #### 1. Using grubby tool.
 #### 2. Modifying /etc/default/grub file and using command grub2-mkconfig.

 #### Also make sure you do not edit the file /boot/grub.cfg directly. This file gets updated automatically with the changes using the grubby tool.

```
 vi /etc/default/grub

modify  “GRUB_TIMEOUT=15”

[root@ip-172-31-87-111 grub2]# grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Adding boot menu entry for UEFI Firmware Settings ...
done


-o, --output=,FILE/
output generated config to FILE [default=stdout]
```


 #### OS and kernel version info :
 
 ```

[root@ip-172-31-87-111 ~]# cat /etc/redhat-release 
Red Hat Enterprise Linux release 9.0 (Plow)
[root@ip-172-31-87-111 ~]# uname -r
5.14.0-70.43.1.el9_0.x86_64

[root@ip-172-31-87-111 grub2]# rpm -qa | grep kernel
kernel-tools-libs-5.14.0-70.43.1.el9_0.x86_64
kernel-tools-5.14.0-70.43.1.el9_0.x86_64
kernel-core-5.14.0-70.43.1.el9_0.x86_64
kernel-modules-5.14.0-70.43.1.el9_0.x86_64
kernel-5.14.0-70.43.1.el9_0.x86_64


```




#### To list all the installed kernel you can use :

```

[root@ip-172-31-87-111 ~]# grubby --info=ALL | grep ^kernel
kernel="/boot/vmlinuz-5.14.0-70.43.1.el9_0.x86_64"

```

#### To get more details on the installed kernel :

```


[root@ip-172-31-87-111 ~]# grubby --info="/boot/vmlinuz-5.14.0-70.43.1.el9_0.x86_64"
index=0
kernel="/boot/vmlinuz-5.14.0-70.43.1.el9_0.x86_64"
args="console=ttyS0,115200n8 console=tty0 net.ifnames=0 rd.blacklist=nouveau nvme_core.io_timeout=4294967295 crashkernel=1G-4G:192M,4G-64G:256M,64G-:512M $tuned_params"
root="UUID=f0bd585c-eec0-4c88-8363-17bb48dec559"
initrd="/boot/initramfs-5.14.0-70.43.1.el9_0.x86_64.img $tuned_initrd"
title="Red Hat Enterprise Linux (5.14.0-70.43.1.el9_0.x86_64) 9.0 (Plow)"
id="ffffffffffffffffffffffffffffffff-5.14.0-70.43.1.el9_0.x86_64"

```

#### To get the default kernel version use :

```
[root@ip-172-31-87-111 ~]# grubby --default-kernel
/boot/vmlinuz-5.14.0-70.43.1.el9_0.x86_64

```

#### The kernel title is the output which you see in the GRUB2 menu while rebooting the Linux server. So to check the title of your default kernel use :

```
[root@ip-172-31-87-111 ~]# grubby --default-title
Red Hat Enterprise Linux (5.14.0-70.43.1.el9_0.x86_64) 9.0 (Plow)
```



#### To get the index number of all the installed kernels :


```
[root@ip-172-31-87-111 ~]# grubby --info=ALL | grep -E "^kernel|^index"
index=0
kernel="/boot/vmlinuz-5.14.0-70.43.1.el9_0.x86_64"
```



#### The boot entry configuration file is located in the /boot/loader/entries/

```
[root@ip-172-31-87-111 entries]# pwd
/boot/loader/entries

[root@ip-172-31-87-111 entries]# cat ffffffffffffffffffffffffffffffff-5.14.0-70.43.1.el9_0.x86_64.conf 
title Red Hat Enterprise Linux (5.14.0-70.43.1.el9_0.x86_64) 9.0 (Plow)
version 5.14.0-70.43.1.el9_0.x86_64
linux /vmlinuz-5.14.0-70.43.1.el9_0.x86_64
initrd /initramfs-5.14.0-70.43.1.el9_0.x86_64.img $tuned_initrd
options root=UUID=f0bd585c-eec0-4c88-8363-17bb48dec559 console=ttyS0,115200n8 console=tty0 net.ifnames=0 rd.blacklist=nouveau nvme_core.io_timeout=4294967295 crashkernel=1G-4G:192M,4G-64G:256M,64G-:512M $tuned_params
grub_users $grub_users
grub_arg --unrestricted
grub_class rhel
````






