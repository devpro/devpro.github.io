# Puppet server installation on CentOS

[Home](../readme.md) > [Puppet](./readme.md) > [Server installation on CentOS](./server_installation_centos.md)

## Pre-requisites

### git

```bash
# this is required to load code and dependencies
sudo yum install git
```

### ntpdate

```bash
# given the master/agent architecture it is important to have correct dates on the system.
yum -y install ntpdate
ntpdate 0.centos.pool.ntp.org
```

## Installation

### Puppet server

```bash
# targets Puppet 5.3 Open Source
sudo rpm -Uvh https://yum.puppet.com/puppet5/puppet5-release-el-7.noarch.rpm
# for a fresh installation:
yum install -y puppetserver
# or, for an upgrade (see https://puppet.com/docs/puppet/5.3/upgrade_minor.html):
yum update puppetserver

# after the installation a puppet user has been created as well as a puppet group, make sure files and folders permissions ownership in regard of this user/group
sudo chown -R puppet:puppet /etc/puppetlabs
sudo chmod 770 -R /etc/puppetlabs
```

### r10k ([quickstart](https://github.com/puppetlabs/r10k/blob/master/doc/dynamic-environments/quickstart.mkd))

```bash
sudo /opt/puppetlabs/puppet/bin/gem install r10k
mkdir /etc/puppetlabs/r10k
vi `/etc/puppetlabs/r10k/r10k.yaml`
```

```yaml
---

# The 'cachedir' setting controls where cached content, such as mirrored Git repositories, are stored on the local machine.
# This location should be persistent, as environments and modules may rely on these files in order to be updated.
cachedir: '/var/cache/r10k'

sources:
  operations:
    remote: 'git_repository_url_with_authentication'
    basedir: '/etc/puppetlabs/code/environments'
```

```bash
# this is the procedure to run r10k, this will create environment folders as well as loading modules
cd /etc/puppetlabs/r10k
sudo /opt/puppetlabs/puppet/bin/r10k deploy environment --puppetfile
```

### Firewall

```bash
# add exception for Puppet port (8140)
firewall-cmd --permanent --zone=public --add-port=8140/tcp
firewall-cmd --reload
```

To go further, you can look at [Puppet cheat sheet](./cheatsheet.md).
