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
[root@bdn ~]# su - postgres
[postgres@bdn ~]$ /usr/local/postgresql/bin/initdb -D /usr/local/postgresql/data/
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

[postgres@bdn ~]$ 
[postgres@bdn ~]$ vim /usr/local/postgresql/data/pg_hba.conf 
# PostgreSQL Client Authentication Configuration File
# ===================================================
#
# Refer to the "Client Authentication" section in the PostgreSQL
# documentation for a complete description of this file.  A short
# synopsis follows.
#
# This file controls: which hosts are allowed to connect, how clients
# are authenticated, which PostgreSQL user names they can use, which
# databases they can access.  Records take one of these forms:
#
# local      DATABASE  USER  METHOD  [OPTIONS]
# host       DATABASE  USER  ADDRESS  METHOD  [OPTIONS]
# hostssl    DATABASE  USER  ADDRESS  METHOD  [OPTIONS]
# hostnossl  DATABASE  USER  ADDRESS  METHOD  [OPTIONS]
#
 17 # (The uppercase items must be replaced by actual values.)
 18 #
 19 # The first field is the connection type: "local" is a Unix-domain
 20 # socket, "host" is either a plain or SSL-encrypted TCP/IP socket,
 21 # "hostssl" is an SSL-encrypted TCP/IP socket, and "hostnossl" is a
 22 # plain TCP/IP socket.
 23 #
 24 # DATABASE can be "all", "sameuser", "samerole", "replication", a
 25 # database name, or a comma-separated list thereof. The "all"
 26 # keyword does not match "replication". Access to replication
 27 # must be enabled in a separate record (see example below).
 28 #
 29 # USER can be "all", a user name, a group name prefixed with "+", or a
 30 # comma-separated list thereof.  In both the DATABASE and USER fields
 31 # you can also write a file name prefixed with "@" to include names
 32 # from a separate file.
 33 #
 34 # ADDRESS specifies the set of hosts the record matches.  It can be a
 35 # host name, or it is made up of an IP address and a CIDR mask that is
 36 # an integer (between 0 and 32 (IPv4) or 128 (IPv6) inclusive) that
 37 # specifies the number of significant bits in the mask.  A host name
 38 # that starts with a dot (.) matches a suffix of the actual host name.
 39 # Alternatively, you can write an IP address and netmask in separate
 40 # columns to specify the set of hosts.  Instead of a CIDR-address, you
 41 # can write "samehost" to match any of the server's own IP addresses,
 42 # or "samenet" to match any address in any subnet that the server is
 43 # directly connected to.
 44 #
 45 # METHOD can be "trust", "reject", "md5", "password", "scram-sha-256",
 46 # "gss", "sspi", "ident", "peer", "pam", "ldap", "radius" or "cert".
 47 # Note that "password" sends passwords in clear text; "md5" or
 48 # "scram-sha-256" are preferred since they send encrypted passwords.
 49 #
 50 # OPTIONS are a set of options for the authentication in the format
 51 # NAME=VALUE.  The available options depend on the different
 52 # authentication methods -- refer to the "Client Authentication"
 53 # section in the documentation for a list of which options are
 54 # available for which authentication methods.
 55 #
 56 # Database and user names containing spaces, commas, quotes and other
 57 # special characters must be quoted.  Quoting one of the keywords
 58 # "all", "sameuser", "samerole" or "replication" makes the name lose
 59 # its special character, and just match a database or username with
 60 # that name.
 61 #
 62 # This file is read on server startup and when the server receives a
 63 # SIGHUP signal.  If you edit the file on a running system, you have to
 64 # SIGHUP the server for the changes to take effect, run "pg_ctl reload",
 65 # or execute "SELECT pg_reload_conf()".
 66 #
 67 # Put your actual configuration here
 68 # ----------------------------------
 69 #
 70 # If you want to allow non-local connections, you need to add more
 71 # "host" records.  In that case you will also need to make PostgreSQL
 72 # listen on a non-local interface via the listen_addresses
 73 # configuration parameter, or via the -i or -h command line switches.
 74 
 75 # CAUTION: Configuring the system for local "trust" authentication
 76 # allows any local user to connect as any PostgreSQL user, including
 77 # the database superuser.  If you do not trust all your local users,
 78 # use another authentication method.
 79 
 80 
 81 # TYPE  DATABASE        USER            ADDRESS                 METHOD
 82 
 83 # "local" is for Unix domain socket connections only
 84 local   all             all                                     trust
 85 # IPv4 local connections:
 86 host    all             all             127.0.0.1/32            trust
 87 # IPv6 local connections:
 88 host    all             all             ::1/128                 trust
 89 # Allow replication connections from localhost, by a user with the
 90 # replication privilege.
 91 local   replication     all                                     trust
 92 host    replication     all             127.0.0.1/32            trust
 93 host    replication     all             ::1/128                 trust
 94 host    all             postgres        0.0.0.0/0               md5
"/usr/local/postgresql/data/pg_hba.conf" 94L, 4581C written                                                                                                                                               
[postgres@bdn ~]$ 
[postgres@bdn ~]$ 
[postgres@bdn ~]$ vim /usr/local/postgresql/data/postgresql.conf 
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
 76 # - Security and Authentication -
 77 
 78 #authentication_timeout = 1min          # 1s-600s
 79 #ssl = off
 80 #ssl_ciphers = 'HIGH:MEDIUM:+3DES:!aNULL' # allowed SSL ciphers
 81 #ssl_prefer_server_ciphers = on
 82 #ssl_ecdh_curve = 'prime256v1'
 83 #ssl_dh_params_file = ''
 84 #ssl_cert_file = 'server.crt'
 85 #ssl_key_file = 'server.key'
 86 #ssl_ca_file = ''
 87 #ssl_crl_file = ''
"/usr/local/postgresql/data/postgresql.conf" 658L, 22763C written                                                                                                                                         
[postgres@bdn ~]$ 
[postgres@bdn ~]$ ls
[postgres@bdn ~]$ 
[postgres@bdn ~]$ /usr/local/postgresql/bin/pg_ctl -D /usr/local/postgresql/data/ -l 
/usr/local/postgresql/bin/pg_ctl: option requires an argument -- 'l'
Try "pg_ctl --help" for more information.
[postgres@bdn ~]$ /usr/local/postgresql/bin/pg_ctl -D /usr/local/postgresql/data/ -l /usr/local/postgresql/data/logfile start
waiting for server to start.... done
server started
[postgres@bdn ~]$ 
[postgres@bdn ~]$ /usr/local/postgresql/bin/psql 
psql (10.3)
Type "help" for help.

postgres=# \password postgres
Enter new password: 
Enter it again: 
postgres=# 
postgres=# exit
postgres-# 
postgres-# quit
postgres-# \q
```
