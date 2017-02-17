### Hostname of mysql node
```
ip-172-31-16-218.ap-southeast-1.compute.internal		Mysql Server
ip-172-31-31-204.ap-southeast-1.compute.internal		Mysql client
ip-172-31-20-171.ap-southeast-1.compute.internal		Mysql client
ip-172-31-25-116.ap-southeast-1.compute.internal		Mysql client
ip-172-31-26-223.ap-southeast-1.compute.internal		Mysql client
```

### Mysql Version
```
[root@ip-172-31-16-218 ~]# mysql --version
mysql  Ver 14.14 Distrib 5.5.54, for Linux (x86_64) using readline 5.1
[root@ip-172-31-16-218 ~]#
```

### Command and output show Mysql Database
```
[root@ip-172-31-16-218 ~]# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 5.5.54 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| hive               |
| hue                |
| mysql              |
| oozie              |
| performance_schema |
| rman               |
| scm                |
| sentry             |
+--------------------+
9 rows in set (0.00 sec)

mysql>
```