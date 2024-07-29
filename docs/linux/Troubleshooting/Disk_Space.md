* disk space usage

```
df -h
```

* biggest (10) directories in size (MB)

```
du -mxS / | sort -n | tail -10
```

* clean package cache

```
apt-get clean
```
```
yum clean
```

* ext2, ext3 file systems: reduce default 5% capacity of partition reserved to root (to 1%):

```
tune2fs -m1 /dev/hda1
```

* delete files older than 10 days and bigger than 5MB under a dir (/tmp for ex):

```
find  /tmp -mtime +10 -size +5M -exec rm -f {} \;
```

* compress unused directory

```
tar cvfz dir.tar.gz dir
```

* look at /var/log , set up logrotate if too big

* Email alerts for low disk space

```bash
#!/bin/bash
# script that will send an email to EMAIL when disk use in partition PART is bigger than %MAX
# adapt these 3 parameters to your case
MAX=95
EMAIL=alert@example.com
PART=sda1
 
USE=`df -h |grep $PART | awk '{ print $5 }' | cut -d'%' -f1`
if [ $USE -gt $MAX ]; then
  echo "Percent used: $USE" | mail -s "Running out of disk space" $EMAIL
fi
```

* journald space

```
journalctl --disk-usage
```

* make space reducing to 500MB or last 2 weeks

```
journalctl --vacuum-size=500M
journalctl --vacuum-time=2weeks
```

* settings in /etc/systemd/journald.conf

```
SystemMaxUse=500M
MaxFileSec=14day

systemctl restart systemd-journald
```