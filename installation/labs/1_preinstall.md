### Check vm.swappiness parameter and change to 1
```
[ec2-user@ip-172-31-0-235 ~]$ cat /proc/sys/vm/swappiness
60
[ec2-user@ip-172-31-0-235 ~]$ sudo vi /etc/sysctl.conf
[ec2-user@ip-172-31-7-199 ~]$ sudo sysctl -p
net.ipv4.ip_forward = 0
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.default.accept_source_route = 0
kernel.sysrq = 0
kernel.core_uses_pid = 1
net.ipv4.tcp_syncookies = 1
kernel.msgmnb = 65536
kernel.msgmax = 65536
kernel.shmmax = 68719476736
kernel.shmall = 4294967296
vm.swappiness = 1
[ec2-user@ip-172-31-0-235 ~]$
```

### Show the mount attributes of all volumes
```
[ec2-user@ip-172-31-12-129 ~]$ mount
/dev/xvda1 on / type ext4 (rw)
proc on /proc type proc (rw)
sysfs on /sys type sysfs (rw)
devpts on /dev/pts type devpts (rw,gid=5,mode=620)
tmpfs on /dev/shm type tmpfs (rw,rootcontext="system_u:object_r:tmpfs_t:s0")
none on /proc/sys/fs/binfmt_misc type binfmt_misc (rw)
[ec2-user@ip-172-31-12-129 ~]$ cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Mon Jun  9 15:02:26 2014
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=23cecb9c-baa3-4407-9485-ddf59583c3ed /                       ext4    defaults        1 1
tmpfs                   /dev/shm                tmpfs   defaults        0 0
devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
sysfs                   /sys                    sysfs   defaults        0 0
proc                    /proc                   proc    defaults        0 0

[ec2-user@ip-172-31-7-199 ~]$ lsblk
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  30G  0 disk
└─xvda1 202:1    0   6G  0 part /
[ec2-user@ip-172-31-7-199 ~]$ blkid
/dev/xvda1: UUID="23cecb9c-baa3-4407-9485-ddf59583c3ed" TYPE="ext4"
```

### Show the reserve space of any non-root, ext-based volumes 
```
[ec2-user@ip-172-31-10-153 ~]$ sudo tune2fs -l /dev/xvda1
tune2fs 1.41.12 (17-May-2010)
Filesystem volume name:   <none>
Last mounted on:          /
Filesystem UUID:          23cecb9c-baa3-4407-9485-ddf59583c3ed
Filesystem magic number:  0xEF53
Filesystem revision #:    1 (dynamic)
Filesystem features:      has_journal ext_attr resize_inode dir_index filetype needs_recovery extent flex_bg sparse_super large_file huge_file uninit_bg dir_nlink extra_isize
Filesystem flags:         signed_directory_hash
Default mount options:    user_xattr acl
Filesystem state:         clean
Errors behavior:          Continue
Filesystem OS type:       Linux
Inode count:              393216
Block count:              1572864
Reserved block count:     78643
Free blocks:              1040829
Free inodes:              338367
First block:              0
Block size:               4096
Fragment size:            4096
Reserved GDT blocks:      383
Blocks per group:         32768
Fragments per group:      32768
Inodes per group:         8192
Inode blocks per group:   512
Flex block group size:    16
Filesystem created:       Mon Jun  9 15:02:11 2014
Last mount time:          Mon Feb 13 03:39:35 2017
Last write time:          Mon Jun  9 15:08:29 2014
Mount count:              2
Maximum mount count:      -1
Last checked:             Mon Jun  9 15:02:11 2014
Check interval:           0 (<none>)
Lifetime writes:          2804 MB
Reserved blocks uid:      0 (user root)
Reserved blocks gid:      0 (group root)
First inode:              11
Inode size:               256
Required extra isize:     28
Desired extra isize:      28
Journal inode:            8
First orphan inode:       22329
Default directory hash:   half_md4
Directory Hash Seed:      0be63e36-131c-4087-93e0-e92e41c4adc5
Journal backup:           inode blocks

```
### Disable transparent hugepages

```
[ec2-user@ip-172-31-0-235 ~]$ sudo sysctl -a | grep hugepages
vm.nr_hugepages = 0
vm.nr_hugepages_mempolicy = 0
vm.hugepages_treat_as_movable = 0
vm.nr_overcommit_hugepages = 0
[ec2-user@ip-172-31-0-235 ~]$
```

### List your network interface configuration

```
[ec2-user@ip-172-31-12-129 ~]$ ifconfig -a
eth0      Link encap:Ethernet  HWaddr 02:89:78:2B:74:CF
          inet addr:172.31.12.129  Bcast:172.31.15.255  Mask:255.255.240.0
          inet6 addr: fe80::89:78ff:fe2b:74cf/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9001  Metric:1
          RX packets:2574 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2436 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:222580 (217.3 KiB)  TX bytes:331699 (323.9 KiB)
          Interrupt:165

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 b)  TX bytes:0 (0.0 b)

[ec2-user@ip-172-31-12-129 ~]$
```

### List forward and reverse host lookups using getent or nslookup

```
[ec2-user@ip-172-31-7-199 ~]$ nslookup 172-31-10-153.ap-southeast-1.compute.internal
Server:         172.31.0.2
Address:        172.31.0.2#53

** server can't find 172-31-10-153.ap-southeast-1.compute.internal: NXDOMAIN

[ec2-user@ip-172-31-7-199 ~]$ nslookup 172.31.10.153
Server:         172.31.0.2
Address:        172.31.0.2#53

Non-authoritative answer:
153.10.31.172.in-addr.arpa      name = ip-172-31-10-153.ap-southeast-1.compute.internal.

Authoritative answers can be found from:

[ec2-user@ip-172-31-7-199 ~]$ getent hosts 172.31.10.153
172.31.10.153   ip-172-31-10-153.ap-southeast-1.compute.internal
[ec2-user@ip-172-31-7-199 ~]$ getent hosts ip-172-31-10-153.ap-southeast-1.compute.internal
172.31.10.153   ip-172-31-10-153.ap-southeast-1.compute.internal
[ec2-user@ip-172-31-7-199 ~]$

```

### Install and start nscd and ntpd service
```
[ec2-user@ip-172-31-1-19 yum.repos.d]$ sudo service ntpd start
Starting ntpd:                                             [  OK  ]
[ec2-user@ip-172-31-1-19 yum.repos.d]$ sudo yum clean all
Loaded plugins: amazon-id, rhui-lb, security
Cleaning repos: rhui-REGION-client-config-server-6 rhui-REGION-rhel-server-releases
              : rhui-REGION-rhel-server-releases-optional rhui-REGION-rhel-server-rh-common
              : rhui-REGION-rhel-server-rhscl
Cleaning up Everything
[ec2-user@ip-172-31-1-19 yum.repos.d]$ sudo yum repolist
Loaded plugins: amazon-id, rhui-lb, security
rhui-REGION-client-config-server-6                                                          | 2.9 kB     00:00
rhui-REGION-client-config-server-6/primary_db                                               | 5.9 kB     00:00
rhui-REGION-rhel-server-releases                                                            | 3.5 kB     00:00
rhui-REGION-rhel-server-releases/primary_db                                                 |  52 MB     00:00
rhui-REGION-rhel-server-releases-optional                                                   | 3.5 kB     00:00
rhui-REGION-rhel-server-releases-optional/primary_db                                        | 5.1 MB     00:00
rhui-REGION-rhel-server-rh-common                                                           | 3.8 kB     00:00
rhui-REGION-rhel-server-rh-common/primary_db                                                |  65 kB     00:00
rhui-REGION-rhel-server-rhscl                                                               | 3.5 kB     00:00
rhui-REGION-rhel-server-rhscl/primary_db                                                    | 3.7 MB     00:00
repo id                                    repo name                                                         status
rhui-REGION-client-config-server-6         Red Hat Update Infrastructure 2.0 Client Configuration Server 6        6
rhui-REGION-rhel-server-releases           Red Hat Enterprise Linux Server 6 (RPMs)                          18,457
rhui-REGION-rhel-server-releases-optional  Red Hat Enterprise Linux Server 6 Optional (RPMs)                 10,498
rhui-REGION-rhel-server-rh-common          Red Hat Enterprise Linux Server 6 RH Common (RPMs)                   129
rhui-REGION-rhel-server-rhscl              Red Hat Enterprise Linux Server 6 RHSCL (RPMs)                     7,509
repolist: 36,599
[ec2-user@ip-172-31-1-19 yum.repos.d]$ sudo yum install nscd
Loaded plugins: amazon-id, rhui-lb, security
Setting up Install Process
Resolving Dependencies
--> Running transaction check
---> Package nscd.x86_64 0:2.12-1.192.el6 will be installed
--> Processing Dependency: glibc = 2.12-1.192.el6 for package: nscd-2.12-1.192.el6.x86_64
--> Running transaction check
---> Package glibc.x86_64 0:2.12-1.132.el6_5.2 will be updated
--> Processing Dependency: glibc = 2.12-1.132.el6_5.2 for package: glibc-devel-2.12-1.132.el6_5.2.x86_64
--> Processing Dependency: glibc = 2.12-1.132.el6_5.2 for package: glibc-headers-2.12-1.132.el6_5.2.x86_64
--> Processing Dependency: glibc = 2.12-1.132.el6_5.2 for package: glibc-common-2.12-1.132.el6_5.2.x86_64
---> Package glibc.x86_64 0:2.12-1.192.el6 will be an update
--> Running transaction check
---> Package glibc-common.x86_64 0:2.12-1.132.el6_5.2 will be updated
---> Package glibc-common.x86_64 0:2.12-1.192.el6 will be an update
--> Processing Dependency: tzdata >= 2015g-4 for package: glibc-common-2.12-1.192.el6.x86_64
---> Package glibc-devel.x86_64 0:2.12-1.132.el6_5.2 will be updated
---> Package glibc-devel.x86_64 0:2.12-1.192.el6 will be an update
---> Package glibc-headers.x86_64 0:2.12-1.132.el6_5.2 will be updated
---> Package glibc-headers.x86_64 0:2.12-1.192.el6 will be an update
--> Running transaction check
---> Package tzdata.noarch 0:2014d-1.el6 will be updated
---> Package tzdata.noarch 0:2016j-1.el6 will be an update
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================
 Package                Arch            Version                    Repository                                 Size
===================================================================================================================
Installing:
 nscd                   x86_64          2.12-1.192.el6             rhui-REGION-rhel-server-releases          230 k
Updating for dependencies:
 glibc                  x86_64          2.12-1.192.el6             rhui-REGION-rhel-server-releases          3.8 M
 glibc-common           x86_64          2.12-1.192.el6             rhui-REGION-rhel-server-releases           14 M
 glibc-devel            x86_64          2.12-1.192.el6             rhui-REGION-rhel-server-releases          989 k
 glibc-headers          x86_64          2.12-1.192.el6             rhui-REGION-rhel-server-releases          617 k
 tzdata                 noarch          2016j-1.el6                rhui-REGION-rhel-server-releases          454 k

Transaction Summary
===================================================================================================================
Install       1 Package(s)
Upgrade       5 Package(s)

Total download size: 20 M
Is this ok [y/N]: y
Downloading Packages:
(1/6): glibc-2.12-1.192.el6.x86_64.rpm                                                      | 3.8 MB     00:00
(2/6): glibc-common-2.12-1.192.el6.x86_64.rpm                                               |  14 MB     00:00
(3/6): glibc-devel-2.12-1.192.el6.x86_64.rpm                                                | 989 kB     00:00
(4/6): glibc-headers-2.12-1.192.el6.x86_64.rpm                                              | 617 kB     00:00
(5/6): nscd-2.12-1.192.el6.x86_64.rpm                                                       | 230 kB     00:00
(6/6): tzdata-2016j-1.el6.noarch.rpm                                                        | 454 kB     00:00
-------------------------------------------------------------------------------------------------------------------
Total                                                                              7.8 MB/s |  20 MB     00:02
Running rpm_check_debug
Running Transaction Test
Transaction Test Succeeded
Running Transaction
  Updating   : tzdata-2016j-1.el6.noarch                                                                      1/11
  Updating   : glibc-common-2.12-1.192.el6.x86_64                                                             2/11
  Updating   : glibc-2.12-1.192.el6.x86_64                                                                    3/11
  Updating   : glibc-headers-2.12-1.192.el6.x86_64                                                            4/11
  Updating   : glibc-devel-2.12-1.192.el6.x86_64                                                              5/11
  Installing : nscd-2.12-1.192.el6.x86_64                                                                     6/11
  Cleanup    : glibc-devel-2.12-1.132.el6_5.2.x86_64                                                          7/11
  Cleanup    : glibc-headers-2.12-1.132.el6_5.2.x86_64                                                        8/11
  Cleanup    : glibc-common-2.12-1.132.el6_5.2.x86_64                                                         9/11
  Cleanup    : glibc-2.12-1.132.el6_5.2.x86_64                                                               10/11
  Cleanup    : tzdata-2014d-1.el6.noarch                                                                     11/11
  Verifying  : glibc-devel-2.12-1.192.el6.x86_64                                                              1/11
  Verifying  : glibc-headers-2.12-1.192.el6.x86_64                                                            2/11
  Verifying  : glibc-2.12-1.192.el6.x86_64                                                                    3/11
  Verifying  : tzdata-2016j-1.el6.noarch                                                                      4/11
  Verifying  : nscd-2.12-1.192.el6.x86_64                                                                     5/11
  Verifying  : glibc-common-2.12-1.192.el6.x86_64                                                             6/11
  Verifying  : glibc-headers-2.12-1.132.el6_5.2.x86_64                                                        7/11
  Verifying  : glibc-devel-2.12-1.132.el6_5.2.x86_64                                                          8/11
  Verifying  : glibc-2.12-1.132.el6_5.2.x86_64                                                                9/11
  Verifying  : tzdata-2014d-1.el6.noarch                                                                     10/11
  Verifying  : glibc-common-2.12-1.132.el6_5.2.x86_64                                                        11/11

Installed:
  nscd.x86_64 0:2.12-1.192.el6

Dependency Updated:
  glibc.x86_64 0:2.12-1.192.el6          glibc-common.x86_64 0:2.12-1.192.el6  glibc-devel.x86_64 0:2.12-1.192.el6
  glibc-headers.x86_64 0:2.12-1.192.el6  tzdata.noarch 0:2016j-1.el6

Complete!
[ec2-user@ip-172-31-1-19 yum.repos.d]$ sudo service nscd start
Starting nscd:                                             [  OK  ]
[ec2-user@ip-172-31-1-19 yum.repos.d]$ sudo service nscd status
nscd (pid 25177) is running...
[ec2-user@ip-172-31-1-19 yum.repos.d]$ sudo service ntpd status
ntpd (pid  25101) is running...
[ec2-user@ip-172-31-1-19 yum.repos.d]$
```
