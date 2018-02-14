# MongoDB Ops Manager

[Home](../readme.md) > [MongoDB](./readme.md) > [Ops Manager](./opsmanager.md)

**Ops Manager** is an incredible tool provided by MongoDB. You need to get a licence to use it in Production but the benefits clearly worth it (more info on [mongodb.com](https://www.mongodb.com/products/ops-manager)).

You can see it as a dashboard opened to anyone inside your organization, where you can completely manage and automate your MongoDB instances, replica and sharding sets as well as giving many live insights about the usage and the data.

To go further: [Architecture](https://docs.opsmanager.mongodb.com/current/core/system-overview/), [Configuration](https://docs.opsmanager.mongodb.com/current/reference/configuration/), [Example Deployment Topologies](https://docs.opsmanager.mongodb.com/current/core/deployments/).

Content:

- Installation
  - [Simple test deployment on CentOS](#install-simple-test-deployment)
  - [Automation Agent](#automation-agent)
  - [Configuration](#configuration)
  - [Appendix](#appendix)

## Installation

<a id="install-simple-test-deployment"></a>

### Simple test deployment on CentOS ([docs.opsmanager.mongodb.com](https://docs.opsmanager.mongodb.com/current/tutorial/install-simple-test-deployment/))

This is an interesting first step and can be used for non-production environments (dev, staging). You can validate many aspects about the procedure, such as the ports to be opened and OS options.

All Ops Manager components will run on the same server: db, backup, application.

MongoDB needs to be first installed and configured to host Ops Manager data & backup.

#### Pre-requisites (hosts, ntpupdate, mongod, firewall)

:warning: Make sure to correctly update either your DNS or the `/etc/hosts` file of every server, a small typo can lead to complicated investigation (true story).

```bash
# make sure ntp & ntpupdate are installed (this is important for the communication between servers)
sudo yum install ntpupdate
sudo ntpdate 0.centos.pool.ntp.org
```

```bash
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
```

```bash
# create the directories for mongod: application database, backup, logs
sudo mkdir -p /data/my/folder
sudo chown -R mongod:mongod /data/my/folder
sudo chmod 770 -R /data/my/folder
```

```bash
# open Ops Manager ports
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
sudo firewall-cmd --permanent --zone=public --add-port=8443/tcp
sudo firewall-cmd --permanent --zone=public --add-port=27000-28000/tcp
sudo firewall-cmd --permanent --zone=public --add-port=25/tcp
sudo firewall-cmd --reload
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
curl -OL https://downloads.mongodb.com/on-prem-mms/rpm/mongodb-mms-3.6.3.606-1.x86_64.rpm
sudo rpm -ivh mongodb-mms-3.6.3.606-1.x86_64.rpm
# Preparing...                          ################################# [100%]
# Updating / installing...
#    1:mongodb-mms-3.6.3.606-1          ################################# [100%]
# the installation will create the following
# base directory for the Ops Manager software has been created
```

```bash
ll /opt/mongodb/mms
# mongodb-mms user and group have been created
cat /etc/passwd
cat /etc/group
```

```bash
# edit & review properties file (contains the connection string to app db)
sudo vi /opt/mongodb/mms/conf/conf-mms.properties
```

```bash
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
- Click on Register then fill and submit the form, follow the guide for the basic configuration (that can be easily edited after)
- You should land in the Ops manager page!

<a id="automation-agent"></a>

## Automation agent

Ops Manager can automate all your MongoDB instances and servers thanks to the Automation Agent that must be installed on all servers.

### CentOS ([tutorial from mongodb.com](https://docs.opsmanager.mongodb.com/v1.6/tutorial/install-automation-agent-with-rpm-package/))

This procedure has been validated against `CentOS Linux release 7.4.1708 (Core)`

- Log in the server

```bash
# make sure ntp & ntpupdate are installed
sudo yum install ntpupdate
sudo ntpdate 0.centos.pool.ntp.org
```

```bash
# ports should not be used and firewall allows traffic on these ports between the servers
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
sudo firewall-cmd --permanent --zone=public --add-port=8443/tcp
sudo firewall-cmd --permanent --zone=public --add-port=27000-28000/tcp
sudo firewall-cmd --reload
```

```bash
# make sure all URLS are known
sudo vi /etc/hosts
# for example on Ops Manager URL
curl -ivs http://myopsmanagerurl:8080
```

```bash
# download the package file from Ops Manager and install it
curl -OL http://myopsmanagerurl:8080/download/agent/automation/mongodb-mms-automation-agent-manager-latest.x86_64.rpm
sudo rpm -U mongodb-mms-automation-agent-manager-latest.x86_64.rpm
```

```bash
# follow the comments to fill mmsGroupId, mmsApiKey, mmsBaseUrl
sudo vi /etc/mongodb-mms/automation-agent.config
```

```bash
# create data directory
sudo mkdir /data
sudo chown mongod:mongod /data
```

```bash
# start the automation agent
sudo service mongodb-mms-automation-agent start
# check its status
sudo service mongodb-mms-automation-agent status
```

```bash
# investigate issues
ll /var/log/mongodb-mms-automation
sudo more /var/log/mongodb-mms-automation/automation-agent-fatal.log
```

- You should now be able to see your node in Ops Manager > Deployment > Agents

- Known issues

  - _libsasl2.so.2: cannot open shared object file: No such file or directory_

    > /opt/mongodb-mms-automation/bin/mongodb-mms-automation-agent: error while loading shared libraries: libsasl2.so.2: cannot open shared object file: No such file or directory

    Fix:

    ```bash
    cd /lib64
    sudo ln -s libsasl2.so.3.0.0 libsasl2.so.2
    ```

  - _SocketException: Address already in use_

    > E STORAGE  [initandlisten] Failed to set up listener: SocketException: Address already in use  
    > I CONTROL  [initandlisten] now exiting  
    > I CONTROL  [initandlisten] shutting down with code:48

    Fix:

    ```bash
    # look at the process using the port
    sudo netstat -tulpn | grep :27000
    sudo kill processid
    ```

### Ubuntu ([tutorial from mongodb.com](https://docs.opsmanager.mongodb.com/v1.6/tutorial/install-automation-agent-with-deb-package/))

Updates made from [docs.cloudmanager.mongodb.com](https://docs.cloudmanager.mongodb.com/tutorial/install-automation-agent-with-deb-package/).

This procedure has been validated against `Ubuntu 16.04.1 LTS`

- Log in the server

```bash
# make sure ntp & ntpupdate are installed
sudo apt install
sudo ntpdate 0.centos.pool.ntp.org
```

```bash
# make sure ops manager url is known
sudo vi /etc/hosts
curl -ivs http://myopsmanagerurl:8080
```

```bash
# replace the URL
curl -OL http://myopsmanagerurl:8080/download/agent/automation/mongodb-mms-automation-agent-manager_latest_amd64.ubuntu1604.deb
sudo dpkg -i mongodb-mms-automation-agent-manager_latest_amd64.ubuntu1604.deb
# Selecting previously unselected package mongodb-mms-automation-agent-manager.
# (Reading database ... 254031 files and directories currently installed.)
# Preparing to unpack mongodb-mms-automation-agent-manager_latest_amd64.ubuntu1604.deb ...
# Unpacking mongodb-mms-automation-agent-manager (4.5.11.2453-1) ...
# Setting up mongodb-mms-automation-agent-manager (4.5.11.2453-1) ...
```

```bash
# follow the comments to fill mmsGroupId, mmsApiKey, mmsBaseUrl
sudo vi /etc/mongodb-mms/automation-agent.config
```

```bash
# create data directory
sudo mkdir /data
sudo chown mongodb:mongodb /data
```

```bash
# start the automation agent
sudo systemctl start mongodb-mms-automation-agent.service
# check its status
sudo systemctl status mongodb-mms-automation-agent.service
```

```bash
# investigate issues
ll /var/log/mongodb-mms-automation
```

<a id="configuration"></a>

## Configuration

### Monitoring

- Once the servers have configured and seen by Ops Manager, you should install at least one monitoring agent on one node.

### Backup

- Make sure a back agent is installed on a server (easy to install from the server view).

- Create a directory to store the head databases

```bash
mkdir /data/mongodb/backupdaemon
sudo chown mongodb-mms:mongodb-mms /data/mongodb/backupdaemon
sudo sudo chmod 774 -R /data/mongodb/backupdaemon
```

- In Ops Manager, click on Admin in the top right > Backup tab. Follow the prompts to configure the Backup storage. You can go with the storage in MongoDB and fill the connection with `localhost:27018`.

- In Ops Manager, click on Backup at the left and create a new entry.

### Email sending

If you have an error while sending the test message, you can test directly from Ops Manager server.

```bash
sudo yum install telnet
telnet smtp.arkadin.lan 25
```

<a id="appendix"></a>

## Appendix

### Follow what's going on!

You can tail the automataion log file.

```bash
tail -f /var/log/mongodb-mms-automation/automation-agent.log
```

You can also look at the processes running on a MongoDB server.

```bash
ps -ef | grep mongo
# mongod    23947      1  0 14:15 ?        00:00:03 /var/lib/mongodb-mms-automation/mongodb-linux-x86_64-3.6.2/bin/mongod -f /data/rsDev2_12/automation-mongod.conf
```

```bash
sudo more /data/rsDev2_12/automation-mongod.conf
```

```yaml
# THIS FILE IS MAINTAINED BY http://myopsmanager:8080 . DO NOT MODIFY AS IT WILL BE OVERWRITTEN.
# To make changes to your MongoDB deployment, please visit http://myopsmanager:8080 . Your Group ID is xxxxxxxxxxxxxxxxxxxxx .
net:
  bindIp: 0.0.0.0
  port: 27000
processManagement:
  fork: "true"
replication:
  replSetName: rsDev2
storage:
  dbPath: /data/rsDev2_12
systemLog:
  destination: file
  path: /data/rsDev2_12/mongodb.log
```

```bash
sudo tail -f /data/rsDev2_12/mongodb.log
```

### Cleanup a MongoDB server

In case of issues, you may want to clean a server where a MongoDB automation agent was installed, or simply a MongoDB server.

- Remove MongoDB server

  MongoDB files are located here: `/var/lib/mongodb`, `/var/log/mongodb`.

  ```bash
  sudo systemctl stop mongod.service
  sudo systemctl disable mongod.service
  sudo dpkg -P mongod
  sudo systemctl daemon-reload
  sudo systemctl reset-failed
  sudo apt-get clean
  ```

- Remove an automation agent

  MongoDB files are located here: `/var/lib/mongodb-mms-automation`, `/var/log/mongodb-mms-automation`.

  ```bash
  sudo systemctl stop mongodb-mms-automation-agent
  sudo dpkg -P mongodb-mms-automation-agent-manager
  sudo rm -rf /etc/mongodb-mms
  sudo rm -rf /var/lib/mongodb-mms-automation
  sudo rm -rf /var/log/mongodb-mms-automation
  ```

  Wait for 5 minutes so Ops manager consider the agent as down.

### Warnings

If you followed this procedure, you will probably get this warnings.

- _Access control is not enabled for the database_

  > WARNING: Access control is not enabled for the database.  
  > Read and write access to data and configuration is unrestricted.

- _This server is bound to localhost

  > WARNING: This server is bound to localhost.  
  > Remote systems will be unable to connect to this server.  
  > Start the server with --bind_ip &lt;address&gt; to specify which IP  
  > addresses it should serve responses from, or with --bind_ip_all to  
  > bind to all interfaces. If this behavior is desired, start the  
  > server with --bind_ip 127.0.0.1 to disable this warning.
 
- _/sys/kernel/mm/transparent_hugepage/enabled is 'always'_

  > WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.  
  > We suggest setting it to 'never'

- _Using the XFS filesystem is strongly recommended with the WiredTiger storage engine_

  > WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine,  
  > See http://dochub.mongodb.org/core/prodnotes-filesystem,

  You can review the different mount drives by executing: `sudo df -hT`.

  Found an article for CentOS 7 on [thegeekdiary.com](https://www.thegeekdiary.com/centos-rhel-7-how-to-disable-transparent-huge-pages-thp/).
