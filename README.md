# Ansible GRR
[![CI](https://github.com/supertarto/ansible-grr/actions/workflows/ci.yml/badge.svg)](https://github.com/supertarto/ansible-grr/actions/workflows/ci.yml)

Install and configure GRR with ansible. Tested with GRR *v3.5.1 and v4.1.1*

## Requirement
A webserver, php >= 7.4 and a database are required. You can use supertarto.apache, supertarto.php and supertarto.mariadb. But other roles are just fine.
For PHP, those modules are needed:
* common
* mysql
* gd
* mbstring
* curl
* json
* ldap
* pdo
* xml

You can have more information on the application official repository:
https://github.com/JeromeDevome/GRR

## Tested plateform
* Debian 10 (Buster)
* Debian 11 (Bullseye)
* Debian 12 (Bookworm)

## Role variables

Informations needed for the installation. Force_update will overwite your previous grr directories.
grr_first_install is meant for the first installation only. If set to true, the table.my.sql file will be imported. 
If not set to false after the first run, your DB might be overridden.
```yml
grr_separate_git_dir: "/usr/local/grr-git"
grr_git_repo_url: "https://github.com/JeromeDevome/GRR.git"
grr_content_dest: /var/www/grr/
grr_git_branch_version: "v3.5.1"
grr_force_update: false
grr_first_install: false
```
Your database informations.
```yml
grr_db_host: "localhost"
grr_db_name: "grrbase"
grr_db_user_name: "grruser"
grr_db_password: "grrpassword"
grr_table_prefix: ""
grr_db_port: "3306"
grr_db_apikey: "mypassphrase"
```
If you want to use LDAP for authentification, set grr_use_ldap to true.
The LDAP protocol can be either ldap or ldaps
If you want to use TLS for your LDAP connection, set grr_ldap_tls to "TRUE". But, beware, it must me an sting and not a boolean.
```yml
grr_use_ldap: false
grr_ldap_protocol: "ldap"
grr_ldap_adress: localhost
grr_ldap_port: "389"
grr_ldap_login: ""
grr_ldap_password: ""
grr_ldap_base: ""
grr_ldap_filter: ""
grr_ldap_tls: "FALSE"
```
This value represent the range before and after the actual year. For example, if you set "10" and if the actual year is 2020, you will see calendar from 2010 to 2030.
```yml
grr_config_year_calendar: "10"
```
Set your default timezone
```yml
grr_config_default_timezone: "Europe/Paris"
```
If your ntp server use winter time, set to 1. Else, 0.
```yml
grr_config_correct_winter_time: "1"
```
Assign a default domain base on your client IP adress. Set 1 if you want to use this feature.
```yml
grr_config_default_domain_by_ip: "0"
```
Maximum number (+1) of periodic reservation allowed.
```yml
grr_config_max_periodic_reservation: "365 + 1"
```
The administrator can restore a database from the web interface. Set to 0 to forbid this feature.
```yml
grr_config_restore_db_admin_page: "1"
```
Only set to 1 for debug need.
```yml
grr_config_debug_flag: "0"
```
Set to 1 to allow automatic search of update.
```yml
grr_config_search_update: "1"
```
Set to 1 to allow the possibility to upload modules.
```yml
grr_config_allow_module_upload: "1"
```
Number of day the logs are kept.
```yml
grr_config_max_log_keep: "365"
```
Set a password if you want to allow the authentification via config.inc.php. Empty if you don't want to use this feature.
```yml
grr_config_auth_with_configinc: ""
```
Hide (or not) the sso configuration interface.
```yml
grr_config_sso_hide_interface: "false"
```
Hide (or not) the ldap configuration interface.
```yml
grr_config_ldap_hide_interface: "false"
```
Hide (or not) the imap authentification configuration interface.
```yml
grr_config_imap_hide_interface: "false"
```
Set the fixed URL that will be set as the CAS service parameter. When this method is not called, a phpCAS script uses its own URL.
```yml
grr_config_CAS_setFixedServiceURL: ""
```

## Examples
```yml
- hosts: somehost

  roles:
    - role: supertarto.apache
    - role: supertarto.mariadb
    - role: supertarto.php
    - role: supertarto.grr
  vars:
    php_packages:
      - php7.4
      - php7.4-common
      - php7.4-mysql
      - php7.4-gd
      - php7.4-mbstring
      - php7.4-curl
      - php7.4-json
      - php7.4-ldap
      - php7.4-pdo
      - php7.4-xml
    apache_create_vhosts: true
    apache_vhosts_filename: "grr.conf"
    apache_vhost_config:
      - listen_ip: "*"
        listen_port: 80
        server_name: host1
        documentroot: "{{ grr_content_dest }}"
        serveradmin: admin@localhost
        custom_param: |
          ErrorLog ${APACHE_LOG_DIR}/error.log
          CustomLog ${APACHE_LOG_DIR}/access.log combined
          LogLevel warn
        directory:
          - path: "{{ grr_content_dest }}"
            config: |
              AllowOverride All
              Order deny,allow
              allow from all

    mariadb_use_dump_script: false
    mariadb_databases:
      - name: "GRRBase"

    mariadb_users:
      - name: "{{ grr_db_user_name }}"
        host: "localhost"
        password: "{{ grr_db_password }}"
        priv: "{{ grr_db_name }}.*:SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER,CREATE TEMPORARY TABLES,LOCK TABLES"

    grr_db_name: "GRRBase"
    grr_db_user_name: "grruser"
    grr_db_password: "grrpassword"
    grr_db_apikey: "mypassphrase"
```

## Installation
```
ansible-galaxy role install supertarto.grr
```

## License
GPL V3.0
