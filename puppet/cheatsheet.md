# Puppet cheat sheet

[Home](../readme.md) > [Puppet](./readme.md) > [Cheat sheet](./cheatsheet.md)

## Puppet agent

### Commands (Windows)

```bash
# install the puppet agent
msiexec /qn /norestart /i path\to\puppet-agent-5.3.X-x64.msi PUPPET_MASTER_SERVER=mypuppetmastername /l*v C:\msipuppetlog.txt
timeout 60
sc config "puppet" start= disabled
sc stop "puppet"

# launch manually the puppet agent
puppet agent --test

# launch locally puppet code (no puppet server needed), see https://puppet.com/docs/puppet/5.3/man/apply.html
puppet apply --modulepath="modules;site" --hiera_config="hiera.yaml" .\manifests\site.pp

# display active configuration
puppet config print

# get information on the machine the way Puppet does
facter

# facts ([man page](https://puppet.com/docs/puppet/5.3/man/facts.html))
puppet facts

# retrieve modules from the [Puppetfile](https://github.com/puppetlabs/r10k/blob/master/doc/puppetfile.mkd)
r10k puppetfile install

# PDK command lines
pdk new module
pdk new class mymodule
pdk new class mymodule::myfolder::myclass
pdk validate
pdk test unit

# list all installed applications
puppet resource package
# list of defined services and their status
puppet resource service
```

### Configuration files (Windows)

| File path                                          | Details                          |
| -------------------------------------------------- | -------------------------------- |
| `C:\Windows\System32\drivers\etc\hosts`            | Host file                        |
| `C:\Users\xxxxxxx\.gitconfig`                      | Git configuration file           |
| `C:\ProgramData\PuppetLabs\puppet\etc\puppet.conf` | Puppet agent configuration file  |
| `C:\ProgramData\PuppetLabs\puppet\etc\ssl`         | Puppet client ssl                |

### Directory structure (Windows)

- `C:\ProgramData\PuppetLabs\code\environments`: local copy of environment files

## Puppet server (aka master)

### Commands (CentOS)

```bash
# start puppet server
service puppetserver start
# systemctl start puppetserver.service

# get puppet server service info
service puppetserver status
# shortcut for systemctl status puppetserver.service

# stop puppet server
service puppetserver stop
# systemctl stop puppetserver.service

# get logs from system journal
journalctl -xe

# get puppet agent service info
service puppet status

# executes r10k ([usage](https://github.com/puppetlabs/r10k/blob/master/doc/dynamic-environments/usage.mkd))
cd /etc/puppetlabs/r10k
sudo /opt/puppetlabs/puppet/bin/r10k deploy environment --puppetfile

# list certificates to be validated
sudo /opt/puppetlabs/puppet/bin/puppet cert list

# sign a certificate
sudo /opt/puppetlabs/puppet/bin/puppet cert sign xxxxxx

# follow logs in real time
tail -f /var/log/puppetlabs/puppetserver/puppetserver.log
tail -f /var/log/puppetlabs/puppetserver/puppetserver-access.log
```

### Configuration files ([doc](https://puppet.com/docs/puppetserver/5.1/configuration.html))

| File path                                       | Details                                 |
| ----------------------------------------------- | --------------------------------------- |
| `/etc/sysconfig/puppetserver`                   | Puppet server configuration file        |
| `/etc/puppetlabs/puppetserver/conf.d/auth.conf` | Puppet serveur auth configuration file ([doc](https://puppet.com/docs/puppetserver/5.1/config_file_auth.html)) |
| `/etc/puppetlabs/puppet/puppet.conf`            | Puppet agent configuration file         |
| `/etc/puppetlabs/puppet/hiera.yaml`             | Hiera configuration file ([doc](https://puppet.com/docs/puppet/5.3/hiera_intro.html)) |
| `/etc/puppetlabs/r10k/r10k.yaml`                | r10k configuration                      |

### Directory structure

- `/etc/puppetlabs`: base path
- `/etc/puppetlabs/code`: Puppet code managed by git, this is where r10k will
- `/etc/puppetlabs/code/environments`: Definition per environment, this is where r10k will create folders per git repository branches (production, staging, etc.)
- `/etc/puppetlabs/puppet`: Puppet Agent configuration
- `/etc/puppetlabs/puppetserver`: PuppetServer configuration
- `/etc/puppetlabs/puppetserver/conf.d`: Settings (see [Puppet Server Configuration](https://puppet.com/docs/puppetserver/5.1/configuration.html))
- `/etc/puppetlabs/r10k`: r10k configuration
- `/opt/puppetlabs`: Internal Puppet stuff, binaries, etc
- `/var/log/messages`: Puppet Agent logs
- `/var/log/puppetlabs`: Other logging
- `/tmp`: Used by the installer (issues if set ‘noexec’)

You can read [Magic directories: a guide to Puppet directory structure](https://puppet.com/blog/magic-directories-guide-to-puppet-directory-structure).

## Terminology

- r10k [puppet.com](https://puppet.com/blog/git-workflows-puppet-and-r10k)
