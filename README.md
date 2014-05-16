**Requirements:**
* PostgreSQL 9.3 installed on centos6.x
<pre>
yum install http://yum.postgresql.org/9.3/redhat/rhel-6-x86_64/pgdg-redhat93-9.3-1.noarch.rpm
yum install postgresql93-server postgresql93-contrib
</pre> 

* Generate instance with service postgresql-9.3 initdb and auto start by boot.
<pre>
service postgresql-9.3 initdb
chkconfig postgresql-9.3 on
</pre>

* Setup your PostgreSQL instance with archive mode on (postgresql.conf)
<pre>
wal_level = archive
archive_mode = on
archive_command = 'test ! -f /var/lib/pgsql/wal/%f && cp %p /var/lib/pgsql/wal/%f'
archive_timeout = 7200
</pre>

* Use the follow directory structure
<pre>
/var/lib/pgsql/9.3
/var/lib/pgsql/backups
/var/lib/pgsql/tablespaces
/var/lib/pgsql/wal
</pre>

* Create log dir in /var/log
<pre>
# mdkir /var/log/postgres_backups
</pre>

* Settings in pg_hba.conf
<pre>
# "local" is for Unix domain socket connections only
local   all             all                                     peer
local   all         	postgres                          	ident
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
host    all         	postgres    	[localip]/32      	trust
</pre>

* Start instance 
<pre>
service postgresql-9.3 start
</pre>

**How to use:**

* Download the latest backup_postgresql script

<pre>
git clone https://github.com/nickvth-public-repo/Continuous-Archiving-PostgreSQL-9.3-backup-script.git
cd scripts
</pre>

<pre>
Usage: backup_postgresql -i<instance name> -p<port number>
  -i  Instance name default use pgsql, because path is /var/lib/pgsql
  -p  Port number default use 5432
  -h  this help screen
Example: backup_postgresql -ipgsql -p5432
</pre>

* Test run

<pre>
./backup_postgresql -ipgsql -p5432 ;tail -f /var/log/postgres_backups/[logfile]
</pre>

**Sources:**
* http://www.postgresql.org/download/linux/redhat/
* http://www.postgresql.org/docs/9.3/static/continuous-archiving.html
