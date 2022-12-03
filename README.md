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
