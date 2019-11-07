# vagrant-mysql

(c) Andre Lohmann (and others) 2019

## Maintainer Contact
 * Andre Lohmann
   <lohmann.andre (at) gmail (dot) com>

## content

Vagrant template, to deploy a mysql database

## Usage

### Configuration

Copy the config.yml.example to config.yml and alternate as you need it.

Copy ansible_vagrant/custom_vars.yml.example to ansible_vagrant/custom_vars.yml and alternate as you need it.

### Run

```
vagrant up
```

```
vagrant ssh
```

Mysql will be listening on the public ip on port 3306.
