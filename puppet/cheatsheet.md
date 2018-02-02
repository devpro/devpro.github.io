# Puppet cheat sheet

## Puppet agent

### Commands (Windows)

```bash
# launch manually the puppet agent
puppet agent --test

# launch locally puppet code (no puppet server needed), see https://puppet.com/docs/puppet/5.3/man/apply.html
puppet apply --modulepath="modules;site" .\manifests\site.pp

# retrieve modules from Puppetfile
r10k puppetfile install

# PDK command lines
pdk new module
pdk new class mymodule
pdk new class mymodule::myfolder::myclass
pdk validate
pdk test unit
```

### Configuration files (Windows)

| File path                                          | Details                          |
| -------------------------------------------------- | -------------------------------- |
| `C:\Windows\System32\drivers\etc\hosts`            | Host file                        |
| `C:\Users\xxxxxxx\.gitconfig`                      | Git configuration file           |
| `C:\ProgramData\PuppetLabs\puppet\etc\puppet.conf` | Puppet server configuration file |

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

# executes r10k
cd /etc/puppetlabs/r10k
sudo /opt/puppetlabs/puppet/bin/r10k deploy environment --puppetfile

# list certificates to be validated
sudo /opt/puppetlabs/puppet/bin/puppet cert list
```

### Configuration files

| File path                            | Details                          |
| ------------------------------------ | -------------------------------- |
| `/etc/sysconfig/puppetserver`        | Puppet server configuration file |
| `/etc/puppetlabs/puppet/puppet.conf` | Puppet agent configuration file  |
| `/etc/puppetlabs/r10k/r10k.yaml`     | r10k configuration               |

### Directory structure

- `/etc/puppetlabs`: base path
- `/etc/puppetlabs/code`: Puppet code managed by git, this is where r10k will
- `/etc/puppetlabs/code/environments`: Definition per environment, this is where r10k will create folders per git repository branches (production, staging, etc.)
- `/etc/puppetlabs/puppet`: Puppet Agent configuration
- `/etc/puppetlabs/puppetserver`: PuppetServer configuration
- `/etc/puppetlabs/r10k`: r10k configuration
- `/opt/puppetlabs`: Internal Puppet stuff, binaries, etc
- `/var/log/messages`: Puppet Agent logs
- `/var/log/puppetlabs`: Other logging
- `/tmp`: Used by the installer (issues if set ‘noexec’)

You can read [Magic directories: a guide to Puppet directory structure](https://puppet.com/blog/magic-directories-guide-to-puppet-directory-structure).
