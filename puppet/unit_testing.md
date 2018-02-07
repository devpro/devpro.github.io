# Unit tests in Puppet

## Documentation

- [RSPEC-PUPPET](http://rspec-puppet.com/)
- [puppetlabs/puppetlabs_spec_helper](https://github.com/puppetlabs/puppetlabs_spec_helper)

## Files

- `.fixture.yml` file is where you can declare dependencies with other modules

  ```ini
  ---
  fixtures:
    forge_modules:
      stdlib: "puppetlabs/stdlib"
    symlinks:
      profile: "#{source_dir}/../../site/profile"
  ```
