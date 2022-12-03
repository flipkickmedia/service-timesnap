# service-timesnap

Provides an incremental time based backup service for any deployed system which requires filesystem or database backup

# Add a system to backup

/etc/cme/backup/<name>.config.toml
```
# Edit this config to suit your environment
[Config]
BaseConfig = "/etc/cme/timesnap/default.backup.toml"
Description = "An example service"
Environment = "development"
Name = "Some Service"

[Config.DB]
Driver = "mysql"
Host = "localhost"
Name = "db-name"
Password = "PAssW04D"
Port = 3306
Sock = "/run/mysqld/mysqld.sock"
User = "mysql"

[Config.DB.Backup]
Driver = "mysql"
Host = "localhost"
Name = "db-name"
Password = "PAssW04D"
Port = 3306
Sock = "/run/mysqld/mysqld.sock"
User = "mysql"

[Config.DB.Timesnap]
BinaryDump = true
Databases = ["db1", "somedb2"]
Incremental = true
KeepDays = 30
Profile = "Backup"
SQLDump = false
TargetDir = "/backup/www-backup/db"

[Config.FS.Timesnap]
Excludes = ["/var/www/logs", "/var/www/htdocs"]
KeepDays = 30
Minspace = 95
SourceDir = ["/root", "/var/www"]
TargetHost = "ninja.flipkick.media"
TargetDir = "/backup/www-backup/fs"

[Config.Log]
File = "/var/log/timesnap.backup.log"
```

source the timesnap script into your script
```
$ run_date=$(date +%s)
$ config_file=/etc/cme/timesnap/example.backup.config.toml
$ log_file=/etc/cme/timesnap/example.backup.config.toml
$ timesnap db ${run_date} "${config_file}" "${log_file}"

service apache2 stop
create_db_timesnap "${config_file}" "${log_file}" "$run_date" 95
create_fs_timesnap "${config_file}" "${log_file}" "$run_date" 95
service apache2 start
```

or call the script directly
```
/var/services/gitea/backup /etc/cme/timesnap/gitea.backup.config.toml
```

create_db_timesnap "${config_file}" "${log_file}" "gitea" ${FS_SOURCE} "${FS_TARGET}/current" $run_date 95 $target_dir
create_fs_timesnap "${config_file}" "${log_file}" "" ${FS_SOURCE} "${FS_TARGET}/current" $run_date 95 $target_dir

service gitea start

```
