# service-timesnap

Provides an incremental time based backup service for any deployed system which requires filesystem or database backup

# Add a system to backup

/etc/cme/backup/<name>.config.toml

```
[Config]
Environment = "development"
Name = "Some Service"
Description = "An example service"
BaseConfig = "/etc/gitea/config.toml"

[Config.DB]
Port = 3306
User = "mysql"
Host = "localhost"
Driver = "mysql"
Password = "PAssW04D"
Sock = "/run/mysqld/mysqld.sock"
Name = "db-name"

[Config.DB.Backup]
Port = 3306
User = "mysql"
Host = "localhost"
Driver = "mysql"
Password = "PAssW04D"
Sock = "/run/mysqld/mysqld.sock"
Name = "db-name"

[Config.DB.Timesnap]
KeepDays = 30
Type = "fs"
Profile = "Backup"
TargetDir = "/backup/var/www/flipkick.media/db"
Databases = "flipkick cme"

[Config.FS.Timesnap]
KeepDays = 30
Excludes = ["/var/www/flipkick.media/logs", "/var/www/git.flipkick.media/services"]
SourceDir = ["/home/git", "/var/www/git.flipkick.media"]
TargetDir = "/backup/git.flipkick.media/fs"
Minspace = 95

[Config.Log]
File = "/var/www/git.flipkick.media/logs/timesnap-backup.log"
```

source the timesnap script into your script, or call the script directly
```
$ run_date=$(date +%s)
$ config_file=/etc/cme/timesnap/example.backup.config.toml
$ log_file=/etc/cme/timesnap/example.backup.config.toml
$ timesnap db ${run_date} "${config_file}" "${log_file}"
```

create_db_timesnap "${config_file}" "${log_file}" "gitea" ${FS_SOURCE} "${FS_TARGET}/current" $run_date 95 $target_dir
create_fs_timesnap "${config_file}" "${log_file}" "" ${FS_SOURCE} "${FS_TARGET}/current" $run_date 95 $target_dir

service gitea start

```
