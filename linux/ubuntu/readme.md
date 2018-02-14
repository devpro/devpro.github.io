# Ubuntu

[Home](../../readme.md) > [Linux](../readme.md) > [Ubuntu](./readme.md)

## Commands

### General

```bash
# get information on running Ubuntu
/etc/lsb-release
# add myuser in sudo group (sudo permissions)
sudo usermod -a -G sudo myuser
# see status of firewall
sudo ufw status verbose
# list services
sudo systemctl list-units | grep mongo
systemctl list-units --type service
# look at current network ressource allocation
netstat -atun
# look at installed packages
dpkg --get-selections
# look for files or directories matching a pattern
sudo updatedb
locate mongod
```
