# Ansible Role: Install mysql

[![Build Status](https://travis-ci.org/tschifftner/ansible-role-mysql.svg?branch=master)](https://travis-ci.org/tschifftner/ansible-role-mysql)

Installs mysql 5.7 on Debian/Ubuntu linux servers.

## Requirements

None

## Dependencies

None.

## Installation

```
$ ansible-galaxy install tschifftner.mysql
```

## Example Playbook

    - hosts: server

      roles:
        - { role: tschifftner.mysql }


## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

### Create database users

_Passwords are required!_

```
mysql_group_users:
  - name: 'user1'
    password: '*******'
    priv: '*.*:USAGE'
    hosts:
      - localhost
      - 127.0.0.1

  - name: 'normaltravis'
    password: '*******'
    priv: '*.*:ALL,GRANT'
    hosts:
      - localhost
      - 127.0.0.1
      
  - name: 'old'
    state: absent
    hosts:
      - localhost
      - 127.0.0.1
```

### Create databases

All databases need to be defined in ```mysql_databases```:

```
mysql_host_databases:
  - name: 'testdb'
    collation: utf8_general_ci
    encoding: utf8
```

### Settings user priviledges on databases and tables

To define user priviledges use the following format:
```
db.table:priv1,priv2
```

Idempotent solution for multiple priviledges (@see http://stackoverflow.com/a/22959760)

```
mysql_privileges:
  - db1.*:ALL,GRANT
  - db2.*:USAGE
  
mysql_host_users:  
  - name: 'user1'
    password: 'travis'
    priv={{ mysql_privileges|join('/') }}
    hosts:
      - localhost
      - 127.0.0.1
```

### Available variables for default, hosts and groups
```
mysql_default_users: []
mysql_host_users: []
mysql_group_users: []

mysql_default_databases: []
mysql_host_databases: []
mysql_group_databases: []
```

### Security

This role sets an administrative user and removes root entirely. Please define the following settings:

```
mysql_admin_home: '/root'
mysql_admin_user: 'admin'
mysql_admin_password: 'Set strong password here!'
```

_When a custom admin username is used, a password must be set!_

## Root password lost

[If you cannot login anymore you can reset your credentials.](https://falseisnotnull.wordpress.com/2012/10/31/did-you-lose-your-mysql-root-password-gnulinux/)

## Remove User
To remove a user define ```state: absent```
```
mysql_host_users:
   - name: 'test'
     host: localhost
     password: 'test'
     priv: '*.*:ALL'
     state: 'absent'
```

## Supported OS

 - Debian 9 (Stretch)
 - Debian 8 (Jessie)
 - Ubuntu 18.04 (Bionic Beaver)
 - Ubuntu 16.04 (Xenial Xerus)
 
## Required ansible version

Ansible 2.5+

## License

[MIT License](http://choosealicense.com/licenses/mit/)

## Author Information

 - [Tobias Schifftner](https://twitter.com/tschifftner), [ambimax® GmbH](https://www.ambimax.de)