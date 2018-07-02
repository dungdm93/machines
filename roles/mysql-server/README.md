Ansible `MySQL server` role
===========================

# Setup root password
Specify (new) root password
```yaml
mysql_root_password: foobar
```

If current MySQL need password:
```yaml
mysql_old_password: legacy_pass
```
Or if your MySQL already installed with no password:
```yaml
# Just leave it empty
mysql_old_password:
```
Or your temporal password store in log file:
```yaml
mysql_catch_temporary_password: true
```
