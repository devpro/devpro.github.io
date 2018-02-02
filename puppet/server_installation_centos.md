# Puppet server installation on CentOS

## Pre-requisites

### git

```bash
sudo yum install git
```

### ntpdate

```bash
yum -y install ntpdate
ntpdate 0.centos.pool.ntp.org
```

## Installation

### Puppet server

```bash
sudo rpm -Uvh https://yum.puppet.com/puppet5/puppet5-release-el-7.noarch.rpm
# for a fresh installation
yum install -y puppetserver
# for an upgrade, see https://puppet.com/docs/puppet/5.3/upgrade_minor.html
yum update puppetserver
```

### r10k

```bash
sudo /opt/puppetlabs/puppet/bin/gem install r10k
```

### Firewall

```bash
firewall-cmd --permanent --zone=public --add-port=8140/tcp
firewall-cmd --reload
```
