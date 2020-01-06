# postgresql

<br/>

```shell
[root@zhangyz src]# wget https://ftp.postgresql.org/pub/source/v10.3/postgresql-10.3.tar.gz
[root@zhangyz src]# tar -xf postgresql-10.3.tar.gz 
[root@zhangyz src]# cd postgresql-10.3
[root@zhangyz postgresql-10.3]# ./configure --prefix=/usr/local/postgresql
[root@zhangyz postgresql-10.3]# make && make install
```

#### 添加用户和用户组

```shell
[root@zhangyz ~]# useradd postgres
[root@zhangyz ~]# groupadd postgres
```

#### 生成数据库文件目录

```shell
[root@zhangyz ~]# mkdir /usr/local/postgresql/data
```

#### 更改用户文件夹访问权限

```shell
[root@zhangyz ~]# chown postgres /usr/local/postgresql/data/
[root@zhangyz ~]# chgrp postgres /usr/local/postgresql/data/
```

```shell
[root@zhangyz ~]# su - postgres
[postgres@zhangyz ~]$ /usr/local/postgresql/bin/initdb -D /usr/local/postgresql/data/
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /usr/local/postgresql/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    /usr/local/postgresql/bin/pg_ctl -D /usr/local/postgresql/data/ -l logfile start
```

```shell
[postgres@zhangyz ~]$ vim /usr/local/postgresql/data/pg_hba.conf 
94 host    all             postgres        0.0.0.0/0               md5
```                                                                                                                          
```shell
[postgres@zhangyz ~]$ vim /usr/local/postgresql/data/postgresql.conf 
  1 # -----------------------------
  2 # PostgreSQL configuration file
  3 # -----------------------------
  4 #
  5 # This file consists of lines of the form:
  6 #
  7 #   name = value
  8 #
  9 # (The "=" is optional.)  Whitespace may be used.  Comments are introduced with
 10 # "#" anywhere on a line.  The complete list of parameter names and allowed
 11 # values can be found in the PostgreSQL documentation.
 12 #
 13 # The commented-out settings shown in this file represent the default values.
 14 # Re-commenting a setting is NOT sufficient to revert it to the default value;
 15 # you need to reload the server.
 16 #
 17 # This file is read on server startup and when the server receives a SIGHUP
 18 # signal.  If you edit the file on a running system, you have to SIGHUP the
 19 # server for the changes to take effect, run "pg_ctl reload", or execute
 20 # "SELECT pg_reload_conf()".  Some parameters, which are marked below,
 21 # require a server shutdown and restart to take effect.
 22 #
 23 # Any parameter can also be given as a command-line option to the server, e.g.,
 24 # "postgres -c log_connections=on".  Some parameters can be changed at run time
 25 # with the "SET" SQL command.
 26 #
 27 # Memory units:  kB = kilobytes        Time units:  ms  = milliseconds
 28 #                MB = megabytes                     s   = seconds
 29 #                GB = gigabytes                     min = minutes
 30 #                TB = terabytes                     h   = hours
 31 #                                                   d   = days
 32 
 33 
 34 #------------------------------------------------------------------------------
 35 # FILE LOCATIONS
 36 #------------------------------------------------------------------------------
 37 
 38 # The default values of these variables are driven from the -D command-line
 39 # option or PGDATA environment variable, represented here as ConfigDir.
 40 
 41 #data_directory = 'ConfigDir'           # use data in another directory
 42                                         # (change requires restart)
 43 #hba_file = 'ConfigDir/pg_hba.conf'     # host-based authentication file
 44                                         # (change requires restart)
 45 #ident_file = 'ConfigDir/pg_ident.conf' # ident configuration file
 46                                         # (change requires restart)
 47 
 48 # If external_pid_file is not explicitly set, no extra PID file is written.
 49 #external_pid_file = ''                 # write an extra PID file
 50                                         # (change requires restart)
 51 
 52 
 53 #------------------------------------------------------------------------------
 54 # CONNECTIONS AND AUTHENTICATION
 55 #------------------------------------------------------------------------------
 56 
 57 # - Connection Settings -
 58 
 59 #listen_addresses = 'localhost'         # what IP address(es) to listen on;
 60                                         # comma-separated list of addresses;
 61                                         # defaults to 'localhost'; use '*' for all
 62                                         # (change requires restart)
 63 port = 5432                             # (change requires restart)
 64 max_connections = 100                   # (change requires restart)
 65 #superuser_reserved_connections = 3     # (change requires restart)
 66 #unix_socket_directories = '/tmp'       # comma-separated list of directories
 67                                         # (change requires restart)
 68 #unix_socket_group = ''                 # (change requires restart)
 69 #unix_socket_permissions = 0777         # begin with 0 to use octal notation
 70                                         # (change requires restart)
 71 #bonjour = off                          # advertise server via Bonjour
 72                                         # (change requires restart)
 73 #bonjour_name = ''                      # defaults to the computer name
 74                                         # (change requires restart)
 75 
```

```shell
[postgres@zhangyz ~]$ /usr/local/postgresql/bin/pg_ctl -D /usr/local/postgresql/data/ -l /usr/local/postgresql/data/logfile.log start
waiting for server to start.... done
server started
[postgres@zhangyz ~]$ /usr/local/postgresql/bin/psql 
psql (10.3)
Type "help" for help.

postgres=# \password postgres
Enter new password: 
Enter it again: 
postgres-# \q
```
