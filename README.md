scripts
=======

Script backup_postgresql

requirements: 
- setup your database with archive mode on
- Use the follow directory structure
/var/lib/pgsql/9.3
/var/lib/pgsql/backups
/var/lib/pgsql/tablespaces
/var/lib/pgsql/wal
When you use multiple PostgreSQL instances on one server
/var/lib/testdb/9.3
/var/lib/testdb/backups
/var/lib/testdb/tablespaces
/var/lib/testdb/wal
- Create log dir in /var/log
mdkir /var/log/postgres_backups

Usage: backup_postgresql -i<instance name> -p<port number>
  -i  Instance name 
  -p  Port number
  -h  this help screen
Example: backup_postgresql -itestdb -p5432

