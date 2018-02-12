# MongoDB Ops Manager

[Home](../readme.md) > [MongoDB](./readme.md) > [Ops Manager](./opsmanager.md)

## Documentation

- [Ops Manager Architecture](https://docs.opsmanager.mongodb.com/current/core/system-overview/)
- [Ops Manager Configuration](https://docs.opsmanager.mongodb.com/current/reference/configuration/)
- [Example Deployment Topologies](https://docs.opsmanager.mongodb.com/current/core/deployments/)

## Installation

### Simple test deployment on CentOS ([docs.opsmanager.mongodb.com](https://docs.opsmanager.mongodb.com/current/tutorial/install-simple-test-deployment/))

Everything will be on the same server: db, backup, application.

MongoDB needs to be first installed and configured to host Ops Manager data & backup.

#### Pre-requisites (wget, ntpupdate, mongod, firewall)

```bash
# make sure wget is installed
sudo yum install wget

# make sure ntp & ntpupdate are installed
sudo yum install ntpupdate
sudo ntpdate 0.centos.pool.ntp.org

# install MongoDB server & shell
echo "[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2013.03/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc" | sudo tee /etc/yum.repos.d/mongodb.repo
sudo yum install -y mongodb-org mongodb-org-shell
# for info: user mongod and group mongod have been created (you can add your user in mongod group)
cat /etc/passwd
cat /etc/group
# service mongod is launched by default, stop it and prevent it from being started on server startup
service mongod status
sudo service mongod stop
sudo chkconfig --level 35 mongod off
# create the directories for mongod: application database, backup, logs
sudo mkdir -p /data/my/folder
sudo chown -R mongod:mongod /data/my/folder
sudo chmod 770 -R /data/my/folder

# open Ops Manager ports
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
sudo firewall-cmd --permanent --zone=public --add-port=8443/tcp
sudo firewall-cmd --reload

# double check the host config
more /etc/hosts
```

#### Configure & launch MongoDB databases for Ops Manager (app & backup)

```bash
# path will depend on your directory strategy, cache size from your server capacity
sudo -u mongod mongod --port 27017 --dbpath /data/mongodb/opsmanager/appdb --logpath /data/mongodb/opsmanager/logs/appdb.log --wiredTigerCacheSizeGB 1 --fork
sudo -u mongod mongod --port 27018 --dbpath /data/mongodb/opsmanager/backup --logpath /data/mongodb/opsmanager/logs/backup.log  --fork
# you can check they are up and running
ps -ef | grep mongod
```

#### Download and install Ops Manager (previously known as MongoDB Monitoring Service, aka MMS)

- Go to [mongodb download page](https://www.mongodb.com/subscription/downloads/ops-manager).
- Select the version (latest stable one in my case) and copy RPM URL,
- For example: `https://downloads.mongodb.com/on-prem-mms/rpm/mongodb-mms-3.6.3.606-1.x86_64.rpm`.

```bash
# retrieve and install the selected version
wget https://downloads.mongodb.com/on-prem-mms/rpm/mongodb-mms-3.6.3.606-1.x86_64.rpm
sudo rpm -ivh mongodb-mms-3.6.3.606-1.x86_64.rpm
# Preparing...                          ################################# [100%]
# Updating / installing...
#    1:mongodb-mms-3.6.3.606-1          ################################# [100%]
# the installation will create the following
# base directory for the Ops Manager software has been created
ll /opt/mongodb/mms
# mongodb-mms user and group have been created
cat /etc/passwd
cat /etc/group
# edit & review properties file (contains the connection string to app db)
sudo vi /opt/mongodb/mms/conf/conf-mms.properties
# start Ops Manager!
sudo service mongodb-mms start
# Generating new Ops Manager private key...
# Starting pre-flight checks
# Successfully finished pre-flight checks
# Migrate Ops Manager data
#    Running migrations...[  OK  ]
# Start Ops Manager server
#    Instance 0 starting...............[  OK  ]
# Starting pre-flight checks
# Successfully finished pre-flight checks
# Start Backup Daemon...[  OK  ]
```

- Open the Ops Manager URL in a browser: `http://<opsManagerHost>:8080`
- Click on Register then fill and submit the form, you should land in the Ops manager page!

#### Backup capabilities (optional)

- Create a directory to store the head databases

```bash
mkdir /data/mongodb/backupdaemon
sudo chown mongodb-mms:mongodb-mms /data/mongodb/backupdaemon
sudo sudo chmod 774 -R /data/mongodb/backupdaemon
```

- In Ops Manager, click on Admin in the top right > Backup tab

Follow the prompts to configure the Backup storage. Ops Manager walks you through the configuration.
When prompted enter `mongodb://localhost:27018`.

## Automation agent

### CentOS ([tutorial from mongodb.com](https://docs.opsmanager.mongodb.com/v1.6/tutorial/install-automation-agent-with-rpm-package/))

- Log in the server

```bash
# make sure ntp & ntpupdate are installed
sudo yum install ntpupdate
sudo ntpdate 0.centos.pool.ntp.org
# make sure ops manager url is known
sudo vi /etc/hosts
curl -ivs http://myopsmanagerurl:8080
# replace the URL
curl -OL http://myopsmanagerurl:8080/download/agent/automation/mongodb-mms-automation-agent-manager-latest.x86_64.rpm
sudo rpm -U mongodb-mms-automation-agent-manager-latest.x86_64.rpm
# follow the comments to fill mmsGroupId, mmsApiKey, mmsBaseUrl
sudo vi /etc/mongodb-mms/automation-agent.config
# create data directory
sudo mkdir /data
sudo chown mongod:mongod /data
# start the automation agent
sudo service mongodb-mms-automation-agent start
sudo service mongodb-mms-automation-agent status
# mongodb-mms-automation-agent is running
# investigate issues
ll /var/log/mongodb-mms-automation
sudo more /var/log/mongodb-mms-automation/automation-agent-fatal.log
```

- You should now be able to see your node in Ops Manager > Deployment > Agents

- Known issues:

> /opt/mongodb-mms-automation/bin/mongodb-mms-automation-agent: error while loading shared libraries: libsasl2.so.2: cannot open shared object file: No such file or directory
Fix:

```bash
cd /lib64
sudo ln -s libsasl2.so.3.0.0 libsasl2.so.2
```

### Ubuntu ([tutorial from mongodb.com](https://docs.opsmanager.mongodb.com/v1.6/tutorial/install-automation-agent-with-deb-package/))

Updates made from [docs.cloudmanager.mongodb.com](https://docs.cloudmanager.mongodb.com/tutorial/install-automation-agent-with-deb-package/).

- Log in the server

```bash
# make sure ntp & ntpupdate are installed
sudo apt install
sudo ntpdate 0.centos.pool.ntp.org
# make sure ops manager url is known
sudo vi /etc/hosts
curl -ivs http://myopsmanagerurl:8080
# replace the URL
curl -OL http://myopsmanagerurl:8080/download/agent/automation/mongodb-mms-automation-agent-manager_latest_amd64.ubuntu1604.deb
sudo dpkg -i mongodb-mms-automation-agent-manager_latest_amd64.ubuntu16 04.deb
# follow the comments to fill mmsGroupId, mmsApiKey, mmsBaseUrl
sudo vi /etc/mongodb-mms/automation-agent.config
# create data directory
sudo mkdir /data
sudo chown mongodb:mongodb /data
# start the automation agent
sudo systemctl start mongodb-mms-automation-agent.service
sudo systemctl status mongodb-mms-automation-agent.service
# investigate issues
ll /var/log/mongodb-mms-automation
```
