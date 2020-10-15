# Ansible GRR
[![Build Status](https://travis-ci.org/supertarto/ansible-grr.svg?branch=master)](https://travis-ci.org/supertarto/ansible-grr)
Install and configure GRR with ansible. Tested with GRR 3.4.1

## Requirement
A webserver, php and a database are required. You can use supertarto.apache, supertarto.php and supertarto.mariadb. But other roles are just fine.
For PHP, those modules are needed:
* php7.3
* php7.3-common
* php7.3-mysql
* php7.3-gd
* php7.3-mbstring
* php7.3-curl
* php7.3-json
* php7.3-ldap
* php7.3-pdo
* php7.3-xml

You can have more information on the application official repository:
https://github.com/JeromeDevome/GRR

## Tested plateform
* Debian 10 (Buster)

## Role variables


## Examples
## Installation
```
ansible-galaxy install supertarto.grr
```
## License
GPL V3.0
