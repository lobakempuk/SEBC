### List Cloud Provider used

```

Instance ID
i-00fe9ef839d9f779c
Public DNS (IPv4)
ec2-54-169-222-108.ap-southeast-1.compute.amazonaws.com
Instance state
running
IPv4 Public IP
54.169.222.108
Instance type
m3.xlarge
IPv6 IPs
-
Elastic IPs
Private DNS
ip-172-31-16-218.ap-southeast-1.compute.internal
Availability zone
ap-southeast-1b
Private IPs
172.31.16.218
Security groups
launch-wizard-2. view inbound rules
Secondary private IPs
Scheduled events
No scheduled events
VPC ID
vpc-00d25464
AMI ID
RHEL-6.5_GA-x86_64-9-Hourly2 (ami-c483df96)
Subnet ID
subnet-2be4905d
Platform
-
Network interfaces
eth0
IAM role
-
Source/dest. check
True
Key pair name
cdh_test5
Owner
690627831585
EBS-optimized
False
Launch time
February 17, 2017 at 11:14:44 AM UTC+7 (less than one hour)
Root device type
ebs
Termination protection
False
Root device
/dev/sda1
Lifecycle
normal
Block devices
/dev/sda1
Monitoring
basic
Alarm status
None
Kernel ID
aki-503e7402
RAM disk ID
-
Placement group
-
Virtualization
paravirtual
Reservation
r-05eff4c10df7f8d79
AMI launch index
1
Tenancy
default
Host ID
-
Affinity
-
State transition reason
-
State transition reason message
-


```

### Linux Release used
```
RHEL-6.5_GA-x86_64-9-Hourly2 (ami-c483df96)
```


### yum repolist detail

```
[root@ip-172-31-16-218 ~]# yum repolist enabled
Loaded plugins: amazon-id, rhui-lb, security
repo id                                   repo name                       status
rhui-REGION-client-config-server-6        Red Hat Update Infrastructure 2      6
rhui-REGION-rhel-server-releases          Red Hat Enterprise Linux Server 18,459
rhui-REGION-rhel-server-releases-optional Red Hat Enterprise Linux Server 10,501
rhui-REGION-rhel-server-rh-common         Red Hat Enterprise Linux Server    129
rhui-REGION-rhel-server-rhscl             Red Hat Enterprise Linux Server  7,509
repolist: 36,604
[root@ip-172-31-16-218 ~]#
```


### Setup user and group


### cat /etc/passwd
```
[root@ip-172-31-16-218 ~]# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
.
.
.
oprofile:x:16:16:Special user account to be used by OProfile:/home/oprofile:/sbi                                                                                     n/nologin
ec2-user:x:500:500::/home/ec2-user:/bin/bash
raffles:x:2000:3002::/home/raffles:/bin/bash
fullerton:x:3000:3001::/home/fullerton:/bin/bash
[root@ip-172-31-16-218 ~]#
```

### cat /etc/group
```
[root@ip-172-31-16-218 ~]# cat /etc/group
root:x:0:
bin:x:1:bin,daemon
daemon:x:2:bin,daemon
sys:x:3:bin,adm
adm:x:4:adm,daemon
tty:x:5:
disk:x:6:
.
.
.
.
oprofile:x:16:
ec2-user:x:500:
raffles:x:2000:
fullerton:x:3000:
hotels:x:3001:
shops:x:3002:
[root@ip-172-31-16-218 ~]#
```