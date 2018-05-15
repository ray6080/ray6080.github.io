---
layout: post  
title: Install Postgres-XL on a cluster
comments: true  
categories:
- blog
tags:
- system
---

Here is a short memo of installing [postgres-xl](http://www.postgres-xl.org) on a cluster.

### Prepare

##### 1. Make a plan

I have serven nodes named from `node1` to `node7`. Here is my plan: `node1` will be set up as **GTM**, and `node2`~`node7` will be set up as **datanode** and **coordinator**.

##### 2. Set up user

**Note:You are assumed to have configured hostname correctly**
Since there will be an error if running as root, we should create a user named `pgxl` on each node, and set up ssh login without password from `node1` to others.  
Firstly, log into each node as root, and create user `pgxl`. (Just set the same password)

```shell
root@node1:~ useradd pgxl
root@node1:~ passwd pgxl
root@node1:~ su pgxl
pgxl@node1:~ mkdir ~/.ssh
pgxl@node1:~ chmod 700 ~/.ssh
```

Secondly, return to `node1`, generate rsa public file. 

```shell
root@node1:~ su pgxl
pgxl@node1:~ ssh-keygen -t rsa
```

**Note:Make sure you leave `passphrase` to blank**


```shell
pgxl@node1:~ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
pgxl@node1:~ chmod 600 ~/.ssh/authorized_keys
```

Lastly, for each remote node(`node2`~`node7`), execute command like

```shell
pgxl@node1:~ ssh-copy-id pgxl@node2
```

After this, you should be able to login to other hosts from `pgxl@node1` without password.

```shell
pgxl@node1:~ ssh node2
Warning: Permanently added 'node2, xxx.xxx.xxx.xxx' (RSA) to the list of known hosts.
Last login: Sun Mar 22 02:00:59 2015 from xxx.xxx.xxx.xxx
pgxl@node2:~
```

If not, just execute this command on each node

```shell
pgxl@node1:~ chmod 700 ~/.ssh
pgxl@node1:~ chmod 600 ~/.ssh/authorized_keys
```

##### 3. Essential libraries
Make sure you have installed essential libraries for building, such as `gcc`,`gcc-c++`,`kernel-devel`, ant etc.

```shell
pgxl@node1:~ yum update
pgxl@node1:~ yum install gcc gcc-c++ kernel-devel readline-devel flex bison bison-devel zlib zlib-devel make docbook-style-dsssl jade
```

##### 4. Install from source code

Download postgres-xl's source code, and install them. But before this, make sure you satisfied the [requirements](http://files.postgres-xl.org/documentation/install-requirements.html).
On each node, download source file and install.

```shell
pgxl@node1:~ wget http://sourceforge.net/projects/postgres-xl/files/Releases/Version_9.2rc/postgres-xl-v9.2-src.tar.gz/download
pgxl@node1:~ tar -xf postgres-xl-v9.2-src.tar
pgxl@node1:~ cd postgres-xl
pgxl@node1:~ ./configure --prefix=/usr/local/pgxl/
pgxl@node1:~ make
pgxl@node1:~ make install
```

Add this line to file `/etc/profile`

```shell
export PATH=$PATH:/usr/local/pgxl/bin
```

and `source` it

```shell
pgxl@node1:~ source /etc/profile
```

You can try some commands to see if you have installed correctly.

### Initialize

##### 1. Init gtm
During initialization, there are some configuration files needed: `postgresql.conf` for datanode and coordinator, `pg_hba.conf` for datanode and coordinator, `gtm.conf` for gtm.  
Firstly, initialize **gtm** on `node1`.

```shell
pgxl@node1:~ initgtm -Z gtm -D ~/pgxl/gtm/
```

There will be default configuration file `gtm.conf` under `~/pgxl/gtm/`, modify it as your need. Here is my configuration.

```shell
# ----------------------
# GTM configuration file
# ----------------------
nodename = 'gtm'				# Specifies the node name.
					# (changes requires restart)
listen_addresses = '*'			# Listen addresses of this GTM.
					# (changes requires restart)
port = 20001				# Port number of this GTM.
					# (changes requires restart)

startup = ACT				# Start mode. ACT/STANDBY.
keepalives_idle = 60			# Keepalives_idle parameter.
keepalives_interval = 10		# Keepalives_interval parameter.
keepalives_count = 0			# Keepalives_count internal parameter.
log_file = 'gtm.log'			# Log file name
log_min_messages = WARNING		# log_min_messages.  Default WARNING.
				  	# Valid value: DEBUG, DEBUG5, DEBUG4, DEBUG3,
					# DEBUG2, DEBUG1, INFO, NOTICE, WARNING,
					# ERROR, LOG, FATAL, PANIC
```

##### 2. init datanode and coordinator

It's annoying to repeat doing the same thing on different hosts, So I write a shell file to do all init works.

```shell
#! /usr/bin/expect
spawn ssh root@node2
expect "*password:"
send "aaaaaa\r"
expect "]*"
send "pwd\r"
send "rm -rf ~/pgxl \r"
expect "]*"
send "mkdir -p ~/pgxl/db1 \r"
send "mkdir -p ~/pgxl/coord1 \r"
send "chown -R pgxl ~/pgxl\r"
send "su pgxl\r"
send "initdb -D ~/pgxl/db1 --nodename db1  -E UTF8 --locale=C -U postgres -W\r"
expect "*password："
send "pgxc\r"
expect "*again:"
send "pgxc\r"
expect "]*"
send "initdb -D ~/pgxl/coord1 --nodename coord1 -E UTF8 --locale=C -U postgres -W\r"
expect "*password："
send "pgxc\r"
expect "*again:"
send "pgxc\r"
expect "]*"
send "cd /usr/local \r"
send "exit \r"
send "exit \r"
expect eof
(repeate above shell code to other nodes)
```

After initilization, you should modify the configuration files on `node2`~`node7`, on which the configuration are nearly the same, except `pgxc_node_name`. Here is the **datanode** configuration(`postgresql.conf`) on `node2`.

```shell
listen_addresses = '*'            # what IP address(es) to listen on;
port = 20008                            # (change requires restart)
max_connections = 400                   # (change requires restart)
superuser_reserved_connections = 13     # (change requires restart)
unix_socket_directory = '.'             # (change requires restart)
tcp_keepalives_idle = 60                # TCP_KEEPIDLE, in seconds;
tcp_keepalives_interval = 10            # TCP_KEEPINTVL, in seconds;
tcp_keepalives_count = 10               # TCP_KEEPCNT;
shared_buffers = 2048MB                 # min 128kB
maintenance_work_mem = 512MB            # min 1MB
vacuum_cost_delay = 10ms                # 0-100 milliseconds
vacuum_cost_limit = 10000               # 1-10000 credits
bgwriter_delay = 10ms                   # 10-10000ms between rounds
shared_queues = 64                      # min 16   
shared_queue_size = 2048kB              # min 16KB
synchronous_commit = off                # synchronization level;
wal_buffers = 16384kB                   # min 32kB, -1 sets based on shared_buffers
wal_writer_delay = 10ms         # 1-10000 milliseconds
checkpoint_segments = 128               # in logfile segments, min 1, 16MB each
network_byte_cost = 0.001               # same scale as above
remote_query_cost = 100.0               # same scale as above
effective_cache_size = 96000MB
log_destination = 'csvlog'              # Valid values are combinations of
logging_collector = on          # Enable capturing of stderr and csvlog
log_truncate_on_rotation = on           # If on, an existing log file with the
log_checkpoints = on
log_connections = on
log_disconnections = on
log_error_verbosity = verbose           # terse, default, or verbose messages
log_statement = 'ddl'                   # none, ddl, mod, all
log_timezone = 'PRC'
log_autovacuum_min_duration = 0 # -1 disables, 0 logs all actions and
datestyle = 'iso, mdy'
timezone = 'PRC'
lc_messages = 'C'                       # locale for system error message
lc_monetary = 'C'                       # locale for monetary formatting
lc_numeric = 'C'                        # locale for number formatting
lc_time = 'C'                           # locale for time formatting
default_text_search_config = 'pg_catalog.english'
pooler_port = 20012                     # Pool Manager TCP port
max_pool_size = 100                     # Maximum pool size
pool_conn_keepalive = 60                # Close connections if they are idle
pool_maintenance_timeout = 30           # Launch maintenance routine if pooler
max_coordinators = 16                   # Maximum number of Coordinators
max_datanodes = 16                      # Maximum number of Datanodes
gtm_host = 'node1'                  # Host name or address of GTM
gtm_port = 20001                        # Port of GTM
pgxc_node_name = 'db1'                   # Coordinator or Datanode name
```

Here is the **coordinator** configuration file on `node2`.

```
listen_addresses = '*'            # what IP address(es) to listen on;
port = 20004                            # (change requires restart)
max_connections = 400                   # (change requires restart)
superuser_reserved_connections = 13     # (change requires restart)
unix_socket_directory = '.'             # (change requires restart)
tcp_keepalives_idle = 60                # TCP_KEEPIDLE, in seconds;
tcp_keepalives_interval = 10            # TCP_KEEPINTVL, in seconds;
tcp_keepalives_count = 10               # TCP_KEEPCNT;
shared_buffers = 2048MB                 # min 128kB
maintenance_work_mem = 512MB            # min 1MB
vacuum_cost_delay = 10ms                # 0-100 milliseconds
vacuum_cost_limit = 10000               # 1-10000 credits
bgwriter_delay = 10ms                   # 10-10000ms between rounds
shared_queues = 64                      # min 16   
shared_queue_size = 2048kB              # min 16KB
synchronous_commit = off                # synchronization level;
wal_buffers = 16384kB                   # min 32kB, -1 sets based on shared_buffers
wal_writer_delay = 10ms         # 1-10000 milliseconds
checkpoint_segments = 128               # in logfile segments, min 1, 16MB each
network_byte_cost = 0.001               # same scale as above
remote_query_cost = 100.0               # same scale as above
effective_cache_size = 96000MB
log_destination = 'csvlog'              # Valid values are combinations of
logging_collector = on          # Enable capturing of stderr and csvlog
log_truncate_on_rotation = on           # If on, an existing log file with the
log_checkpoints = on
log_connections = on
log_disconnections = on
log_error_verbosity = verbose           # terse, default, or verbose messages
log_statement = 'ddl'                   # none, ddl, mod, all
log_timezone = 'PRC'
log_autovacuum_min_duration = 0 # -1 disables, 0 logs all actions and
datestyle = 'iso, mdy'
timezone = 'PRC'
lc_messages = 'C'                       # locale for system error message
lc_monetary = 'C'                       # locale for monetary formatting
lc_numeric = 'C'                        # locale for number formatting
lc_time = 'C'                           # locale for time formatting
default_text_search_config = 'pg_catalog.english'
pooler_port = 20010                     # Pool Manager TCP port
max_pool_size = 100                     # Maximum pool size
pool_conn_keepalive = 60                # Close connections if they are idle
pool_maintenance_timeout = 30           # Launch maintenance routine if pooler
max_coordinators = 16                   # Maximum number of Coordinators
max_datanodes = 16                      # Maximum number of Datanodes
gtm_host = 'node1'                  # Host name or address of GTM
gtm_port = 20001                        # Port of GTM
pgxc_node_name = 'coord1'                   # Coordinator or Datanode name
```

**PAY ATTENTION TO THE PORTS**, make sure that **coordinator** and **datanode** have different `port`(including `pooler_port`)

Lastly, don't forget to modify file `pg_hba.conf` under every datanode and coordinator installtion directories.

```shell
# TYPE DATABASE USER ADDRESS METHOD  
local all all trust
host all all 127.0.0.1/32 trust
host all all ::1/128 trust
host all all 0.0.0.0/0 trust
```



### Start and shutdown

After all the init, you can start the whole cluster now.

Start by this sequence:
+ gtm
+ (gtm_standby)
+ (gtm_proxy)
+ datanode
+ coordinator

And stop is in reverse.

Here is the start shell file

```shell
#! /usr/bin/expect
send "cd ~  \r"
send "gtm_ctl start -Z gtm -D ~/pgxl/gtm/ \r"

spawn ssh pgxl@node2
send "pg_ctl start -D ~/pgxl/db1/ -Z datanode \r"
send "pg_ctl start -D ~/pgxl/coord1 -Z coordinator \r"
send  "exit\r"
expect eof
(repeate above shell code to other nodes)
```

**AFTER STARTING**, you can run `ps -ef | grep postgres` or `ps -ef | grep gtm` to check if all of you services are running correctly. Then you should log into db on each node (including datanode and coordinator), and add node information.

```shell
pgxl@node2:~ psql -p 20004 -U postgres  
postgres=# CREATE NODE db1 WITH(type='datanode', host='localhost', PORT=20008, primary=true);  
postgres=# CREATE NODE db2 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord2 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# CREATE NODE db3 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord3 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# CREATE NODE db4 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord4 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# CREATE NODE db5 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord5 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# CREATE NODE db6 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord6 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# ALTER NODE coord1 WITH(PORT=20004);
postgres=# SELECT pgxc_pool_reload();
postgres=# \q

pgxl@node2:~ psql -p 20008 -U postgres 
postgres=# CREATE NODE coord1 WITH(type='datanode', host='localhost', PORT=20004);  
postgres=# CREATE NODE db2 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord2 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# CREATE NODE db3 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord3 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# CREATE NODE db4 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord4 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# CREATE NODE db5 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord5 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# CREATE NODE db6 WITH(type='datanode', host='xxx.xxx.xxx.xxx', PORT=20008);  
postgres=# CREATE NODE coord6 WITH(TYPE='coordinator', HOST='xxx.xxx.xxx.xxx', PORT=20004);
postgres=# ALTER NODE db1 WITH(TYPE='datanode', PORT=20008, PRIMARY=true);
postgres=# SELECT pgxc_pool_reload();
postgres=# \q
```

Here is the shutdown shell file

```shell
#! /usr/bin/expect
spawn ssh pgxl@node2
send "cd ~ \r"
send "pg_ctl stop -D ~/pgxl/coord1 -Z coordinator \r"
send "pg_ctl stop -D ~/pgxl/db1/ -Z datanode \r"
send  "exit\r"
expect eof
(repeate above shell code to other nodes)
```

All DONE.

### Test
Just a simple test example. Create a table, and insert some data into it.

```shell
pgxl@node2:~ psql -p 20004 -U postgres  
postgres=# CREATE DATABASE test;
postgres=# \c test
postgres=# CREATE TABLE test(id int);
postgres=# INSERT INTO test VALUES(1);
postgres=# SELECT * FROM test;
postgres=# \q
```

Log into anther datanode, and see if you can get the inserted data.
```shell
pgxl@node4:~ psql -p 20004 -U postgres  
postgres=# SELECT * FROM test;
postgres=# \q
```

If you can get it, so here we have done!