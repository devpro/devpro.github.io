# Ubuntu

[Home](../readme.md) > [Linux](./linux.md) > [Ubuntu](./ubuntu.md)

## Links

- [Download](https://www.ubuntu.com/download)

## Commands

### General

```bash
# get information on running Ubuntu
more /etc/lsb-release
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
# go back to previous directory
cd -
# search in command history
#Ctrl+R
```
